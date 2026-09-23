
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

```sql
-- =====================================================================================
-- SCRIPT DE BASE DE DATOS POSTGRESQL PARA RURAL DELIVERY MVP (SUPABASE)
-- Objetivo: Crear la estructura de tablas y configurar PostGIS para geolocalización.
-- =====================================================================================

-- -------------------------------------------------------------------------------------
-- 1. ACTIVACIÓN DE EXTENSIONES
-- Se activa PostGIS para soportar el tipo de dato GEOGRAPHY (coordenadas GPS).
-- Esto corrige el error "type "geography" does not exist".
-- -------------------------------------------------------------------------------------
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS postgis;

-- -------------------------------------------------------------------------------------
-- 2. TABLA users
-- Utiliza la tabla auth.users de Supabase para la autenticación y la enlaza para el rol.
-- -------------------------------------------------------------------------------------
CREATE TABLE public.users (
    id UUID PRIMARY KEY REFERENCES auth.users(id) NOT NULL,
    email TEXT UNIQUE NOT NULL,
    -- Tipo de usuario/rol para el dashboard
    rol TEXT NOT NULL CHECK (rol IN ('admin', 'restaurante', 'repartidor', 'pendiente')) DEFAULT 'pendiente',
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);

-- Habilitar RLS (Row Level Security)
ALTER TABLE public.users ENABLE ROW LEVEL SECURITY;

-- Política de solo lectura para todos los roles logueados (para buscar IDs)
CREATE POLICY "Enable read access for all authenticated users" ON public.users
FOR SELECT TO authenticated
USING (true);

-- -------------------------------------------------------------------------------------
-- 3. TABLA restaurants
-- -------------------------------------------------------------------------------------
CREATE TABLE public.restaurants (
    id SERIAL PRIMARY KEY,
    owner_user_id UUID REFERENCES public.users(id) NOT NULL,
    name TEXT NOT NULL,
    address TEXT NOT NULL,
    -- Uso del tipo GEOGRAPHY para la ubicación precisa del restaurante (base para las rutas)
    location GEOGRAPHY(Point, 4326) NOT NULL, 
    is_active BOOLEAN NOT NULL DEFAULT TRUE,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);

ALTER TABLE public.restaurants ENABLE ROW LEVEL SECURITY;

-- Política para que los restaurantes solo vean su propio registro
CREATE POLICY "Restaurantes pueden ver y editar sus datos" ON public.restaurants
FOR ALL TO authenticated
USING (auth.uid() = owner_user_id)
WITH CHECK (auth.uid() = owner_user_id);

-- -------------------------------------------------------------------------------------
-- 4. TABLA deliverers (Repartidores)
-- -------------------------------------------------------------------------------------
CREATE TABLE public.deliverers (
    id SERIAL PRIMARY KEY,
    user_id UUID REFERENCES public.users(id) NOT NULL,
    nombre TEXT NOT NULL,
    phone_number TEXT,
    estado_actual TEXT NOT NULL CHECK (estado_actual IN ('libre', 'en-ruta', 'fuera-servicio')) DEFAULT 'libre',
    -- Última ubicación conocida (opcional, para V2)
    last_known_location GEOGRAPHY(Point, 4326), 
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL
);

ALTER TABLE public.deliverers ENABLE ROW LEVEL SECURITY;

-- Política de lectura para todos (el Admin necesita ver todos los repartidores)
CREATE POLICY "Enable read access for all authenticated users on deliverers" ON public.deliverers
FOR SELECT TO authenticated
USING (true);

-- -------------------------------------------------------------------------------------
-- 5. TABLA orders (Pedidos)
-- Usamos INTEGER para el estado para coincidir con la lógica 1-6 de la app.
-- -------------------------------------------------------------------------------------
CREATE TABLE public.orders (
    id SERIAL PRIMARY KEY,
    restaurant_id INTEGER REFERENCES public.restaurants(id) NOT NULL,
    customer_name TEXT NOT NULL,
    customer_phone TEXT NOT NULL,
    delivery_address TEXT NOT NULL,
    -- Lat/Lng del destino del cliente (CRÍTICO para VRP)
    delivery_gps GEOGRAPHY(Point, 4326) NOT NULL, 
    total_amount NUMERIC(10, 2) NOT NULL,
    -- Estado del pedido: 1=Registrado, 3=Listo, 4=Asignado, 5=En Ruta, 6=Finalizado
    status INTEGER NOT NULL CHECK (status >= 1 AND status <= 6) DEFAULT 1, 
    assigned_deliverer_id INTEGER REFERENCES public.deliverers(id),
    created_at TIMESTAMP WITH TIME ZONE DEFAULT timezone('utc'::text, now()) NOT NULL,
    finished_at TIMESTAMP WITH TIME ZONE
    -- La columna 'items' (JSONB) se crearía aquí: items JSONB NOT NULL
);

ALTER TABLE public.orders ENABLE ROW LEVEL SECURITY;

-- Política para que los repartidores puedan ver los pedidos asignados y cambiar su estado.
CREATE POLICY "Deliverers view and update assigned orders" ON public.orders
FOR ALL TO authenticated
USING (assigned_deliverer_id = (SELECT id FROM public.deliverers WHERE user_id = auth.uid()))
WITH CHECK (assigned_deliverer_id = (SELECT id FROM public.deliverers WHERE user_id = auth.uid()));

-- Política para que los restaurantes vean y editen sus propios pedidos (status 1-3)
CREATE POLICY "Restaurants view and update their own orders" ON public.orders
FOR ALL TO authenticated
USING (restaurant_id = (SELECT id FROM public.restaurants WHERE owner_user_id = auth.uid()))
WITH CHECK (restaurant_id = (SELECT id FROM public.restaurants WHERE owner_user_id = auth.uid()));


-- =====================================================================================
-- 6. TRIGGERS Y FUNCIONES DE SOPORTE (Opcional, pero útil)
-- Función para crear automáticamente el perfil de usuario cuando se registra
-- NOTA: Esta función requiere configurar un Trigger en el esquema auth.
-- =====================================================================================
-- CREATE OR REPLACE FUNCTION public.handle_new_user()
-- RETURNS TRIGGER AS $$
-- BEGIN
--   INSERT INTO public.users (id, email, rol)
--   VALUES (NEW.id, NEW.email, 'pendiente');
--   RETURN NEW;
-- END;
-- $$ LANGUAGE plpgsql SECURITY DEFINER;


```

---
### V3
```sql
-- =====================================================================================
-- SCRIPT DE BASE DE DATOS POSTGRESQL PARA RURAL DELIVERY MVP (SUPABASE)
-- Modelo Completo: Incluye todos los tipos ENUM, logística de tareas y contabilidad.
-- Usa DROP/CREATE OR REPLACE para reejecución segura.
-- =====================================================================================

-- -------------------------------------------------------------------------------------
-- 0. ELIMINACIÓN PREVIA (PARA RE-EJECUCIÓN SEGURA)
-- * Se eliminan las tablas en orden inverso a su dependencia, luego los tipos.
-- * Comentar o eliminar esta sección si es la primera ejecución.
-- -------------------------------------------------------------------------------------

DROP TABLE IF EXISTS public.shipment_status_logs CASCADE;
DROP TABLE IF EXISTS public.incident_reports CASCADE;
DROP TABLE IF EXISTS public.driver_shifts CASCADE;
DROP TABLE IF EXISTS public.driver_tasks CASCADE;
DROP TABLE IF EXISTS public.batches CASCADE;
DROP TABLE IF EXISTS public.shipment_dishes CASCADE;
DROP TABLE IF EXISTS public.shipments CASCADE;
DROP TABLE IF EXISTS public.dishes CASCADE;
DROP TABLE IF EXISTS public.drivers CASCADE;
DROP TABLE IF EXISTS public.restaurants CASCADE;
DROP TABLE IF EXISTS public.users CASCADE;

DROP TYPE IF EXISTS public.shift_status;
DROP TYPE IF EXISTS public.assignment_type;
DROP TYPE IF EXISTS public.task_type;
DROP TYPE IF EXISTS public.payment_type;
DROP TYPE IF EXISTS public.shipment_status;
DROP TYPE IF EXISTS public.driver_status;
DROP TYPE IF EXISTS public.user_role;

-- -------------------------------------------------------------------------------------
-- 1. EXTENSIONES Y CONFIGURACIÓN INICIAL
-- -------------------------------------------------------------------------------------
-- Habilita la extensión PostGIS (requerida para el tipo GEOGRAPHY)
CREATE EXTENSION IF NOT EXISTS postgis;
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- -------------------------------------------------------------------------------------
-- 2. CREACIÓN DE TIPOS (ENUMS)
-- ** Usamos CREATE TYPE, pero si usas CREATE OR REPLACE TYPE con Supabase, asegúrate de
-- ** que no haya dependencias directas. Para mayor seguridad en reejecución, aquí
-- ** se usa DROP previo y luego CREATE TYPE.
-- -------------------------------------------------------------------------------------

-- Tipo de Rol de Usuario
CREATE TYPE user_role AS ENUM (
  'restaurant',
  'driver',
  'admin'
);

-- Estado Actual del Repartidor
CREATE TYPE driver_status AS ENUM (
  'ONLINE',
  'OFFLINE'
);

-- Flujo de Estado del Envío (Pedido)
CREATE TYPE shipment_status AS ENUM (
  'REGISTERED',       -- Pedido creado por el restaurante, pendiente de confirmar
  'PREPARING',        -- Cocina ha confirmado y está preparando
  'READY_FOR_PICKUP', -- Cocina ha terminado, esperando repartidor (CRÍTICO para asignación)
  'IN_TRANSIT',       -- Repartidor ha recogido
  'DELIVERED',        -- Entregado
  'CANCELLED'         -- Cancelado
);

-- Tipo de Pago (Cobro)
CREATE TYPE payment_type AS ENUM (
  'PRE_PAID',
  'CASH_ON_DELIVERY',
  'TPV_ON_DELIVERY'
);

-- Tipo de Tarea Logística
CREATE TYPE task_type AS ENUM (
  'PICKUP',
  'DELIVERY'
);

-- Origen de la Asignación de Lote
CREATE TYPE assignment_type AS ENUM (
  'MOTOR',
  'ADMIN_MANUAL'
);

-- Estado del Turno de Repartidor
CREATE TYPE shift_status AS ENUM (
  'ACTIVE',
  'LIQUIDATED'
);


-- -------------------------------------------------------------------------------------
-- 3. TABLAS CENTRALES (USUARIOS Y ROLES)
-- -------------------------------------------------------------------------------------

-- Tabla: users (Simula la tabla de perfiles que tiene la FK al ID de Auth de Supabase)
CREATE TABLE public.users (
  user_id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  "role" user_role NOT NULL,
  is_active BOOLEAN DEFAULT true,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Tabla: restaurants
CREATE TABLE public.restaurants (
  restaurant_id SERIAL PRIMARY KEY,
  user_id INT UNIQUE NOT NULL REFERENCES public.users(user_id) ON DELETE CASCADE,
  name VARCHAR(255) NOT NULL,
  address TEXT NOT NULL,
  -- PostGIS: 4326 es el SRID estándar para Lat/Lng (WGS 84)
  "location" geography(Point, 4326) NOT NULL,
  phone_number VARCHAR(50),
  commission_flat_fee DECIMAL(10, 2) DEFAULT 0.00,
  commission_percentage DECIMAL(5, 2) DEFAULT 0.00
);

-- Tabla: drivers
CREATE TABLE public.drivers (
  driver_id SERIAL PRIMARY KEY,
  user_id INT UNIQUE NOT NULL REFERENCES public.users(user_id) ON DELETE CASCADE,
  first_name VARCHAR(100) NOT NULL,
  last_name VARCHAR(100) NOT NULL,
  phone_number VARCHAR(50) NOT NULL,
  current_status driver_status DEFAULT 'OFFLINE',
  current_location geography(Point, 4326)
);


-- -------------------------------------------------------------------------------------
-- 4. TABLAS DE PRODUCTO Y PEDIDOS (EL "QUÉ")
-- -------------------------------------------------------------------------------------

-- Tabla: dishes (Carta)
CREATE TABLE public.dishes (
  dish_id SERIAL PRIMARY KEY,
  restaurant_id INT NOT NULL REFERENCES public.restaurants(restaurant_id) ON DELETE CASCADE,
  name VARCHAR(255) NOT NULL,
  description TEXT,
  price DECIMAL(10, 2) NOT NULL,
  prep_time_minutes INT NOT NULL,
  is_available BOOLEAN DEFAULT true
);

-- Tabla: shipments (Envíos/Pedidos)
CREATE TABLE public.shipments (
  shipment_id SERIAL PRIMARY KEY,
  restaurant_id INT NOT NULL REFERENCES public.restaurants(restaurant_id),
  driver_id INT REFERENCES public.drivers(driver_id) ON DELETE SET NULL,
  status shipment_status NOT NULL DEFAULT 'REGISTERED',
  client_name VARCHAR(255) NOT NULL,
  client_phone VARCHAR(50) NOT NULL,
  client_address_text TEXT NOT NULL,
  client_location geography(Point, 4326) NOT NULL,
  notes_from_restaurant TEXT,
  payment_type payment_type NOT NULL,
  amount_to_collect DECIMAL(10, 2) NOT NULL DEFAULT 0.00,
  base_prep_time_minutes INT NOT NULL,
  extra_prep_time_minutes INT NOT NULL DEFAULT 0,
  commission_charged DECIMAL(10, 2),
  time_created TIMESTAMPTZ DEFAULT NOW(),
  time_preparation_started TIMESTAMPTZ,
  time_ready_for_pickup TIMESTAMPTZ,
  time_picked_up TIMESTAMPTZ,
  time_delivered TIMESTAMPTZ,
  
  -- Columna JSONB para items (Alineación con la especificación de la app de React)
  items JSONB
);

-- Tabla: shipment_dishes (Tabla Pívot)
CREATE TABLE public.shipment_dishes (
  shipment_dish_id SERIAL PRIMARY KEY,
  shipment_id INT NOT NULL REFERENCES public.shipments(shipment_id) ON DELETE CASCADE,
  dish_id INT NOT NULL REFERENCES public.dishes(dish_id),
  quantity INT NOT NULL DEFAULT 1,
  price_at_time_of_order DECIMAL(10, 2) NOT NULL
);


-- -------------------------------------------------------------------------------------
-- 5. TABLAS DE LOGÍSTICA Y TAREAS (EL "CÓMO")
-- -------------------------------------------------------------------------------------

-- Tabla: batches (Lotes)
CREATE TABLE public.batches (
  batch_id SERIAL PRIMARY KEY,
  driver_id INT NOT NULL REFERENCES public.drivers(driver_id),
  assigned_by assignment_type NOT NULL,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Tabla: driver_tasks (Hoja de Ruta)
CREATE TABLE public.driver_tasks (
  task_id SERIAL PRIMARY KEY,
  batch_id INT NOT NULL REFERENCES public.batches(batch_id) ON DELETE CASCADE,
  driver_id INT NOT NULL REFERENCES public.drivers(driver_id),
  sequence INT NOT NULL,
  task_type task_type NOT NULL,
  restaurant_id INT REFERENCES public.restaurants(restaurant_id),
  shipment_id INT REFERENCES public.shipments(shipment_id),
  status shipment_status NOT NULL DEFAULT 'REGISTERED', -- Usamos shipment_status para coherencia de estados
  completed_at TIMESTAMPTZ,
  -- Constraint para asegurar que los FKs son correctos según el tipo
  CONSTRAINT check_task_type CHECK (
    (task_type = 'PICKUP' AND restaurant_id IS NOT NULL AND shipment_id IS NULL) OR
    (task_type = 'DELIVERY' AND restaurant_id IS NULL AND shipment_id IS NOT NULL)
  )
);

-- -------------------------------------------------------------------------------------
-- 6. TABLAS DE CONTABILIDAD Y LOGS (EL "CONTROL")
-- -------------------------------------------------------------------------------------

-- Tabla: driver_shifts (Turnos)
CREATE TABLE public.driver_shifts (
  shift_id SERIAL PRIMARY KEY,
  driver_id INT NOT NULL REFERENCES public.drivers(driver_id),
  start_time TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  end_time TIMESTAMPTZ,
  initial_float DECIMAL(10, 2) NOT NULL,
  status shift_status NOT NULL DEFAULT 'ACTIVE',
  liquidated_at TIMESTAMPTZ,
  liquidated_by_admin_id INT REFERENCES public.users(user_id),
  liquidation_notes TEXT
);

-- Tabla: incident_reports (Incidencias)
CREATE TABLE public.incident_reports (
  incident_id SERIAL PRIMARY KEY,
  shipment_id INT NOT NULL REFERENCES public.shipments(shipment_id),
  driver_id INT NOT NULL REFERENCES public.drivers(driver_id),
  notes TEXT NOT NULL,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Tabla: shipment_status_logs (Historial)
CREATE TABLE public.shipment_status_logs (
  log_id SERIAL PRIMARY KEY,
  shipment_id INT NOT NULL REFERENCES public.shipments(shipment_id) ON DELETE CASCADE,
  old_status shipment_status,
  new_status shipment_status NOT NULL,
  changed_by_user_id INT REFERENCES public.users(user_id) ON DELETE SET NULL,
  "timestamp" TIMESTAMPTZ DEFAULT NOW()
);


-- -------------------------------------------------------------------------------------
-- 7. ÍNDICES (CRUCIALES PARA EL MOTOR DE OPTIMIZACIÓN)
-- -------------------------------------------------------------------------------------

CREATE INDEX idx_shipments_status ON public.shipments (status);
CREATE INDEX idx_shipments_restaurant_id ON public.shipments (restaurant_id);
CREATE INDEX idx_drivers_status ON public.drivers (current_status);
CREATE INDEX idx_driver_tasks_batch_id ON public.driver_tasks (batch_id);
CREATE INDEX idx_driver_tasks_driver_id ON public.driver_tasks (driver_id);
CREATE INDEX idx_driver_shifts_driver_id ON public.driver_shifts (driver_id);

-- Índices espaciales GiST para PostGIS (Permite búsquedas geográficas rápidas)
CREATE INDEX idx_drivers_location ON public.drivers USING GIST (current_location);
CREATE INDEX idx_restaurants_location ON public.restaurants USING GIST ("location");
CREATE INDEX idx_shipments_client_location ON public.shipments USING GIST (client_location);

-- -------------------------------------------------------------------------------------
-- 8. SEGURIDAD (ROW LEVEL SECURITY - RLS)
-- -------------------------------------------------------------------------------------

ALTER TABLE public.users ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.restaurants ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.drivers ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.shipments ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.dishes ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.shipment_dishes ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.batches ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.driver_tasks ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.driver_shifts ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.incident_reports ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.shipment_status_logs ENABLE ROW LEVEL SECURITY;

-- Nota: Las políticas de RLS no se incluyen aquí.

-- -------------------------------------------------------------------------------------
-- FIN DEL SCRIPT
-- -------------------------------------------------------------------------------------

```