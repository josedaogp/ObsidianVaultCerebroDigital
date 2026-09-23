## Docker-compose finanzasJoseda
version: '3.8'

services:
  postgres:
    image: postgres:latest
    container_name: finanzasJosedaBBDD
    environment:
      POSTGRES_USER: joseda
      POSTGRES_PASSWORD: JosedafinanzasBBDD
      POSTGRES_DB: postgres
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:

## Conexión pgadmin:
![[Pasted image 20240416181011.png]]
## Esquema
1. **Tabla de Categorías de Gasto:**
    
    - **id_categoria** (Primary Key): Identificador único de la categoría.
    - **nombre_categoria**: Nombre descriptivo de la categoría de gasto.
2. **Tabla de Presupuestos:**
    
    - **id_presupuesto** (Primary Key): Identificador único del presupuesto.
    - **id_categoria** (Foreign Key): Referencia a la categoría de gasto asociada.
    - **presupuesto_mensual**: Presupuesto asignado para esta categoría en un mes.
    - **presupuesto_anual**: Presupuesto asignado para esta categoría en un año.
3. **Tabla de Gastos:**
    
    - **id_gasto** (Primary Key): Identificador único del gasto.
    - **id_categoria** (Foreign Key): Referencia a la categoría de gasto a la que pertenece este gasto.
    - **monto**: Cantidad de dinero gastada.
    - **fecha**: Fecha en que se realizó el gasto.
    - **nota**: Campo opcional para notas adicionales.
4. **Tabla de Monederos:**
    
    - **id_monedero** (Primary Key): Identificador único del monedero.
    - **nombre_monedero**: Nombre descriptivo del monedero.
    - **saldo**: Saldo actual del monedero.
    - **id_categoria** (Foreign Key, Opcional): Referencia a la categoría de gasto asociada con este monedero.
5. **Tabla de Ingresos:**
    
    - **id_ingreso** (Primary Key): Identificador único del ingreso.
    - **monto**: Cantidad de dinero ingresada.
    - **fecha**: Fecha en que se recibió el ingreso.
    - **nota**: Campo opcional para notas adicionales.

## SQL para crear las tablas de la BBDD
-- Crear la tabla de Categorías de Gasto
CREATE TABLE categorias_gasto (
    id_categoria SERIAL PRIMARY KEY,
    nombre_categoria VARCHAR(100) NOT NULL
);

-- Crear la tabla de Presupuestos
CREATE TABLE presupuestos (
    id_presupuesto SERIAL PRIMARY KEY,
    id_categoria INT REFERENCES categorias_gasto(id_categoria),
    presupuesto_mensual NUMERIC(10, 2),
    presupuesto_anual NUMERIC(10, 2)
);

-- Crear la tabla de Gastos
CREATE TABLE gastos (
    id_gasto SERIAL PRIMARY KEY,
    id_categoria INT REFERENCES categorias_gasto(id_categoria),
    monto NUMERIC(10, 2) NOT NULL,
    fecha DATE NOT NULL,
    nota TEXT
);

-- Crear la tabla de Monederos
CREATE TABLE monederos (
    id_monedero SERIAL PRIMARY KEY,
    nombre_monedero VARCHAR(100) NOT NULL,
    saldo NUMERIC(10, 2) NOT NULL,
    id_categoria INT REFERENCES categorias_gasto(id_categoria)
);

-- Crear la tabla de Ingresos
CREATE TABLE ingresos (
    id_ingreso SERIAL PRIMARY KEY,
    monto NUMERIC(10, 2) NOT NULL,
    fecha DATE NOT NULL,
    nota TEXT
);

## SQL Creación TODA la BD (Copilot a partir de las notas de la tablet 17/4)
-- Tabla de gastos
CREATE TABLE gasto (
    id_gasto INT PRIMARY KEY,
    nota VARCHAR(255) NOT NULL,
    fecha DATE NOT NULL,
    monto DECIMAL(10,2) NOT NULL,
    id_categoria INT,
    presupuesto_mensual_asignado DECIMAL(10,2),
    FOREIGN KEY (id_categoria) REFERENCES categoria_gastos(id_categoria)
);

-- Tabla de categorías de gastos
CREATE TABLE categoria_gasto (
    id_categoria INT PRIMARY KEY,
    nombre_categoria VARCHAR(255) NOT NULL
    presupuesto_mensual DECIMAL(10,2) NOT NULL
    FOREIGN KEY (id_tipo_categoria) REFERENCES tipo_categoria(id_tipo_categoria) 
);

-- Tabla de exceso o sobrante
CREATE TABLE exceso_sobrante (
    id_exceso_sobrante INT PRIMARY KEY,
    cantidad DECIMAL(10,2) NOT NULL,
    id_gasto INT,
    FOREIGN KEY (id_gasto) REFERENCES gastos(id_gasto)
    id_monedero INT,
    FOREIGN KEY (id_monedero) REFERENCES monedero(id_monedero)
);

-- Tabla de ingresos
CREATE TABLE ingreso (
    id SERIAL PRIMARY KEY,
    ingreso_ps FLOAT,
    monto FLOAT,
    fecha DATE,
    hora TIME,
    usuario VARCHAR(255),
    descripcion TEXT
);

-- Tabla de relación entre gastos e ingresos
CREATE TABLE mes_gastos_ingresos (
    id SERIAL PRIMARY KEY,
    ms_gasto_id INT REFERENCES gastos(id),
    ingreso_id INT REFERENCES ingresos(id)
);

-- Tabla de números
CREATE TABLE numeros (
   id SERIAL PRIMARY KEY,
   numero VARCHAR(255),
   anho INT,
   saldo FLOAT
);

-- Tabla de bienes
CREATE TABLE bienes (
   id SERIAL PRIMARY KEY,
   nombre_bien VARCHAR(255),
   monto FLOAT
);