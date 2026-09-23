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