```sql
-- -----------------------------------------------------------------
-- Script de Creación de BBDD V1.5 (PostgreSQL para Supabase)
-- -----------------------------------------------------------------
--
-- IMPORTANTE: Antes de ejecutar este script, ve a tu dashboard de
-- Supabase > Database > Extensions y habilita la extensión `postgis`.
--
-- -----------------------------------------------------------------

-- ----------------------------------------
-- 1. CREACIÓN DE TIPOS (ENUMS)
-- ----------------------------------------
-- Es necesario crear los tipos ENUM antes de usarlos en las tablas.

CREATE TYPE user_role AS ENUM (
  'restaurant',
  'driver',
  'admin'
);

CREATE TYPE driver_status AS ENUM (
  'ONLINE',
  'OFFLINE'
);

CREATE TYPE shipment_status AS ENUM (
  'REGISTERED',       -- Pedido creado por el restaurante, pendiente de confirmar
  'PREPARING',        -- Cocina ha confirmado y está preparando
  'READY_FOR_PICKUP', -- Cocina ha terminado, esperando repartidor
  'IN_TRANSIT',       -- Repartidor ha recogido
  'DELIVERED',        -- Entregado
  'CANCELLED'         -- Cancelado
);

CREATE TYPE payment_type AS ENUM (
  'PRE_PAID',           -- Pagado por Bizum, etc.
  'CASH_ON_DELIVERY',   -- Cobro en efectivo
  'TPV_ON_DELIVERY'     -- Cobro con TPV
);

CREATE TYPE task_type AS ENUM (
  'PICKUP',
  'DELIVERY'
);

CREATE TYPE assignment_type AS ENUM (
  'MOTOR',
  'ADMIN_MANUAL'
);

CREATE TYPE shift_status AS ENUM (
  'ACTIVE',
  'LIQUIDATED'
);


-- ----------------------------------------
-- 2. TABLAS CENTRALES (USUARIOS Y ROLES)
-- ----------------------------------------

-- Tabla: users
-- Almacena la información de login para todos los roles [CORE-01]
CREATE TABLE users (
  user_id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  "role" user_role NOT NULL,
  is_active BOOLEAN DEFAULT true,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
-- Nota: En Supabase, esta tabla a menudo es gestionada por `auth.users`.
-- Podrías usar la tabla `auth.users` de Supabase y añadir una tabla `profiles`
-- con una FK a `auth.users.id` y una columna `role`.
-- Por simplicidad del script, la creamos aquí.

-- Tabla: restaurants
-- Información del negocio del restaurante
CREATE TABLE restaurants (
  restaurant_id SERIAL PRIMARY KEY,
  user_id INT UNIQUE NOT NULL REFERENCES users(user_id) ON DELETE CASCADE,
  name VARCHAR(255) NOT NULL,
  address TEXT NOT NULL,
  -- PostGIS: 4326 es el SRID estándar para Lat/Lng (WGS 84)
  "location" geography(Point, 4326) NOT NULL,
  phone_number VARCHAR(50),
  commission_flat_fee DECIMAL(10, 2) DEFAULT 0.00,
  commission_percentage DECIMAL(5, 2) DEFAULT 0.00
);

-- Tabla: drivers
-- Información del repartidor
CREATE TABLE drivers (
  driver_id SERIAL PRIMARY KEY,
  user_id INT UNIQUE NOT NULL REFERENCES users(user_id) ON DELETE CASCADE,
  first_name VARCHAR(100) NOT NULL,
  last_name VARCHAR(100) NOT NULL,
  phone_number VARCHAR(50) NOT NULL,
  current_status driver_status DEFAULT 'OFFLINE',
  -- PostGIS: Almacena la última ubicación conocida
  current_location geography(Point, 4326)
);

-- ----------------------------------------
-- 3. TABLAS DE PRODUCTO Y PEDIDOS (EL "QUÉ")
-- ----------------------------------------

-- Tabla: dishes (Carta)
-- Platos que cada restaurante puede vender [R-00]
CREATE TABLE dishes (
  dish_id SERIAL PRIMARY KEY,
  restaurant_id INT NOT NULL REFERENCES restaurants(restaurant_id) ON DELETE CASCADE,
  name VARCHAR(255) NOT NULL,
  description TEXT,
  price DECIMAL(10, 2) NOT NULL,
  prep_time_minutes INT NOT NULL, -- Tiempo medio de preparación [R-00.3]
  is_available BOOLEAN DEFAULT true
);

-- Tabla: shipments (Envíos)
-- La tabla más importante. El pedido de envío.
CREATE TABLE shipments (
  shipment_id SERIAL PRIMARY KEY,
  restaurant_id INT NOT NULL REFERENCES restaurants(restaurant_id),
  driver_id INT REFERENCES drivers(driver_id) ON DELETE SET NULL, -- Puede ser NULL al inicio
  status shipment_status NOT NULL DEFAULT 'REGISTERED',
  client_name VARCHAR(255) NOT NULL,
  client_phone VARCHAR(50) NOT NULL,
  client_address_text TEXT NOT NULL,
  client_location geography(Point, 4326) NOT NULL, -- Coordenadas del cliente [R-03.3]
  notes_from_restaurant TEXT,
  payment_type payment_type NOT NULL,
  amount_to_collect DECIMAL(10, 2) NOT NULL DEFAULT 0.00,
  base_prep_time_minutes INT NOT NULL, -- Calculado de los platos [R-03.2]
  extra_prep_time_minutes INT NOT NULL DEFAULT 0, -- Botones +5, +10 [R-02.6]
  commission_charged DECIMAL(10, 2), -- Se calcula al entregar [R-07]
  time_created TIMESTAMPTZ DEFAULT NOW(),
  time_preparation_started TIMESTAMPTZ,
  time_ready_for_pickup TIMESTAMPTZ,
  time_picked_up TIMESTAMPTZ,
  time_delivered TIMESTAMPTZ
);

-- Tabla: shipment_dishes (Tabla Pívot)
-- Qué platos y qué cantidad van en cada envío.
CREATE TABLE shipment_dishes (
  shipment_dish_id SERIAL PRIMARY KEY,
  shipment_id INT NOT NULL REFERENCES shipments(shipment_id) ON DELETE CASCADE,
  dish_id INT NOT NULL REFERENCES dishes(dish_id), -- ON DELETE RESTRICT (para no borrar platos si hay pedidos)
  quantity INT NOT NULL DEFAULT 1,
  price_at_time_of_order DECIMAL(10, 2) NOT NULL -- "Congela" el precio
);

-- ----------------------------------------
-- 4. TABLAS DE LOGÍSTICA Y TAREAS (EL "CÓMO")
-- ----------------------------------------

-- Tabla: batches (Lotes)
-- Agrupación de tareas para un repartidor (decisión del motor/admin)
CREATE TABLE batches (
  batch_id SERIAL PRIMARY KEY,
  driver_id INT NOT NULL REFERENCES drivers(driver_id),
  assigned_by assignment_type NOT NULL,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Tabla: driver_tasks (Hoja de Ruta)
-- La lista de pasos ordenada que ve el repartidor [D-02]
CREATE TABLE driver_tasks (
  task_id SERIAL PRIMARY KEY,
  batch_id INT NOT NULL REFERENCES batches(batch_id) ON DELETE CASCADE,
  driver_id INT NOT NULL REFERENCES drivers(driver_id),
  sequence INT NOT NULL, -- Orden del paso (1, 2, 3...)
  task_type task_type NOT NULL,
  restaurant_id INT REFERENCES restaurants(restaurant_id), -- NULL si es 'DELIVERY'
  shipment_id INT REFERENCES shipments(shipment_id), -- NULL si es 'PICKUP'
  status ENUM('PENDING', 'COMPLETED') NOT NULL DEFAULT 'PENDING',
  completed_at TIMESTAMPTZ,
  -- Constraint para asegurar que los FKs son correctos según el tipo
  CONSTRAINT check_task_type CHECK (
    (task_type = 'PICKUP' AND restaurant_id IS NOT NULL AND shipment_id IS NULL) OR
    (task_type = 'DELIVERY' AND restaurant_id IS NULL AND shipment_id IS NOT NULL)
  )
);

-- ----------------------------------------
-- 5. TABLAS DE CONTABILIDAD Y LOGS (EL "CONTROL")
-- ----------------------------------------

-- Tabla: driver_shifts (Turnos)
-- Sesión de trabajo de un repartidor, clave para la liquidación
CREATE TABLE driver_shifts (
  shift_id SERIAL PRIMARY KEY,
  driver_id INT NOT NULL REFERENCES drivers(driver_id),
  start_time TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  end_time TIMESTAMPTZ,
  initial_float DECIMAL(10, 2) NOT NULL, -- "Bote" de cambio inicial [D-07]
  status shift_status NOT NULL DEFAULT 'ACTIVE',
  liquidated_at TIMESTAMPTZ,
  liquidated_by_admin_id INT REFERENCES users(user_id), -- El admin que liquidó
  liquidation_notes TEXT -- Notas del admin sobre descuadres [A-04.3]
);

-- Tabla: incident_reports (Incidencias)
-- El "buzón" de problemas del repartidor [D-06] y [A-05]
CREATE TABLE incident_reports (
  incident_id SERIAL PRIMARY KEY,
  shipment_id INT NOT NULL REFERENCES shipments(shipment_id),
  driver_id INT NOT NULL REFERENCES drivers(driver_id),
  notes TEXT NOT NULL,
  created_at TIMESTAMT_TZ DEFAULT NOW()
);

-- Tabla: shipment_status_logs (Historial)
-- (Opcional pero recomendada) Historial de cada cambio de estado
CREATE TABLE shipment_status_logs (
  log_id SERIAL PRIMARY KEY,
  shipment_id INT NOT NULL REFERENCES shipments(shipment_id) ON DELETE CASCADE,
  old_status shipment_status,
  new_status shipment_status NOT NULL,
  changed_by_user_id INT REFERENCES users(user_id) ON DELETE SET NULL, -- Quién lo cambió
  "timestamp" TIMESTAMPTZ DEFAULT NOW()
);

-- ----------------------------------------
-- 6. ÍNDICES (PARA OPTIMIZAR CONSULTAS)
-- ----------------------------------------
-- El motor logístico necesitará consultar esto muy rápido.

CREATE INDEX idx_shipments_status ON shipments (status); -- Para el Kanban [R-02] y el Motor [MOTOR-01]
CREATE INDEX idx_shipments_restaurant_id ON shipments (restaurant_id);
CREATE INDEX idx_drivers_status ON drivers (current_status); -- Para el Admin [A-01]
CREATE INDEX idx_driver_tasks_batch_id ON driver_tasks (batch_id);
CREATE INDEX idx_driver_tasks_driver_id ON driver_tasks (driver_id);
CREATE INDEX idx_driver_shifts_driver_id ON driver_shifts (driver_id);

-- Crear índices espaciales (GiST) para PostGIS (¡CRUCIAL para el motor!)
-- Esto permite búsquedas geográficas rápidas (ej. "qué repartidores hay cerca")
CREATE INDEX idx_drivers_location ON drivers USING GIST (current_location);
CREATE INDEX idx_restaurants_location ON restaurants USING GIST ("location");
CREATE INDEX idx_shipments_client_location ON shipments USING GIST (client_location);

-- ----------------------------------------
-- FIN DEL SCRIPT
-- ----------------------------------------

```