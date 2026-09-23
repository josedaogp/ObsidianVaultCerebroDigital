DBDIAGRAM.io una herramienta para bases de datos... podría utilizarla.
## Esquema

Representa un gasto en la aplicación
1. ***gasto***
	- **id_gasto** : Primary Key (PK en adelante)
	- **monto** : float not null --> Lo que se ha gastado
	- **nota** : String
	- **fecha_gasto** : Date Not Null
	- **id_categoria** : Foreign Key(categoria_gasto) Uno a uno
	- **presupuesto_mensual_asignado** : float not null --> Se tiene que guardar el presupuesto mensual al que se asignó el gasto porque ese presupuesto viene de un registro que se puede cambiar. Así conservamos el histórico correctamente
	
Representa una categoría que puede tener un gasto
2. ***categoria_gasto***
	- **id_categoria** : Primary Key
	- **nombre_categoria** : String not null
	- **presupuesto_mensual** : float 
	- **id_tipo_categoria** : Foreign Key(tipo_categoria) muchos a uno

Guarda todos los tipos de categoria de un gasto
3. ***tipo_categoria****
	- **id_tipo_categoria** : Primary Key
	- **nombre_tipo_categoria** : String not null

Guarda el sobrante relacionado a un gasto
4. ***sobrante_gasto***
	- **id_sobrante_gasto** : Primary Key
	- **cantidad_sobrante** : float --> not null
	- **id_gasto** : foreign key(gasto) uno a uno
	- **id_monedero** : foreign key(monedero) uno a uno -->será lo que indique a qué monedero se va el sobrante

Guarda el exceso relacionado a un gasto
5. ***exceso_gasto***
	- **id_exceso_gasto** : Primary Key
	- **cantidad_exceso** : float not null
	- **id_gasto** : foreign key(gasto) uno a uno
	- **id_monedero** : foreign key(monedero) uno a uno -->será lo que indique de qué monedero se cogió el exceso (cuando sobra dinero en una categoría un mes, se puede coger de algún monedero)

Guarda un ingreso en concreto
6. ***ingreso***
	- **id_ingreso**: Primary Key
	- **monto**: float not null
	- **nota**: string
	- **fecha_ingreso**: date

Representa esas mini cuentas de ahorro que quiero conseguir
7. ***monedero***
	- **id_monedero** : primary key
	- **nombre_monedero** : String not null
	- **saldo_monedero** : float por defecto será cero en su creación
	- **saldo_objetivo**: float

Todas las cuentas de bancos, efectivo, etc donde guarde dinero
8. ***bien***
	- **id_bien** : primary key
	- **nombre_bien** : string not null
	- **monto_bien**: float por defecto será cero en su creación

## SQL Generado por ChatGPT
Están en orden para que no de error al meterla en el editor de sql.

-- Tabla para guardar un ingreso en concreto
CREATE TABLE ingreso (
    id_ingreso SERIAL PRIMARY KEY,
    monto FLOAT NOT NULL,
    nota VARCHAR,
    fecha_ingreso DATE
);

-- Tabla para representar esas mini cuentas de ahorro
CREATE TABLE monedero (
    id_monedero SERIAL PRIMARY KEY,
    nombre_monedero VARCHAR NOT NULL,
    saldo_monedero FLOAT DEFAULT 0,
    saldo_objetivo FLOAT
);

-- Tabla para representar todas las cuentas de bancos, efectivo, etc., donde se guarda dinero
CREATE TABLE bien (
    id_bien SERIAL PRIMARY KEY,
    nombre_bien VARCHAR NOT NULL,
    monto_bien FLOAT DEFAULT 0
);

-- Tabla para guardar todos los tipos de categoría de un gasto
CREATE TABLE tipo_categoria (
    id_tipo_categoria SERIAL PRIMARY KEY,
    nombre_tipo_categoria VARCHAR NOT NULL
);

-- Tabla para representar una categoría que puede tener un gasto
CREATE TABLE categoria_gasto (
    id_categoria SERIAL PRIMARY KEY,
    nombre_categoria VARCHAR NOT NULL,
    presupuesto_mensual FLOAT,
    id_tipo_categoria INTEGER REFERENCES tipo_categoria(id_tipo_categoria) ON DELETE SET NULL
);

-- Tabla para representar un gasto en la aplicación
CREATE TABLE gasto (
    id_gasto SERIAL PRIMARY KEY,
    monto FLOAT NOT NULL,
    nota VARCHAR,
    fecha_gasto DATE NOT NULL,
    id_categoria INTEGER REFERENCES categoria_gasto(id_categoria) ON DELETE SET NULL,
    presupuesto_mensual_asignado FLOAT NOT NULL
);

-- Tabla para guardar el sobrante relacionado a un gasto
CREATE TABLE sobrante_gasto (
    id_sobrante_gasto SERIAL PRIMARY KEY,
    cantidad_sobrante FLOAT NOT NULL,
    id_gasto INTEGER REFERENCES gasto(id_gasto) ON DELETE SET NULL,
    id_monedero INTEGER REFERENCES monedero(id_monedero) ON DELETE SET NULL
);

-- Tabla para guardar el exceso relacionado a un gasto
CREATE TABLE exceso_gasto (
    id_exceso_gasto SERIAL PRIMARY KEY,
    cantidad_exceso FLOAT NOT NULL,
    id_gasto INTEGER REFERENCES gasto(id_gasto) ON DELETE SET NULL,
    id_monedero INTEGER REFERENCES monedero(id_monedero) ON DELETE SET NULL
);

## Datos de prueba
Para insertarlos, todas las tablas tienen que estar vacías.

-- Inserción de datos de prueba en la tabla 'ingreso'
INSERT INTO ingreso (monto, nota, fecha_ingreso) VALUES
    (100.50, 'Ingreso 1', '2024-04-01'),
    (200.75, 'Ingreso 2', '2024-04-02'),
    (150.20, 'Ingreso 3', '2024-04-03'),
    (300.30, 'Ingreso 4', '2024-04-04');

-- Inserción de datos de prueba en la tabla 'monedero'
INSERT INTO monedero (nombre_monedero, saldo_monedero, saldo_objetivo) VALUES
    ('Monedero 1', 500.00, 1000.00),
    ('Monedero 2', 750.25, NULL),
    ('Monedero 3', 300.50, 600.00),
    ('Monedero 4', 1000.75, 2000.00);

-- Inserción de datos de prueba en la tabla 'bien'
INSERT INTO bien (nombre_bien, monto_bien) VALUES
    ('Bien 1', 5000.00),
    ('Bien 2', 7500.25),
    ('Bien 3', 3000.50),
    ('Bien 4', 10000.75);

-- Inserción de datos de prueba en la tabla 'tipo_categoria'
INSERT INTO tipo_categoria (nombre_tipo_categoria) VALUES
    ('Tipo 1'),
    ('Tipo 2'),
    ('Tipo 3'),
    ('Tipo 4');

-- Inserción de datos de prueba en la tabla 'categoria_gasto'
INSERT INTO categoria_gasto (nombre_categoria, presupuesto_mensual, id_tipo_categoria) VALUES
    ('Categoría 1', 500.00, 1),
    ('Categoría 2', 750.25, 2),
    ('Categoría 3', NULL, 3),
    ('Categoría 4', 1000.75, 4);

-- Inserción de datos de prueba en la tabla 'gasto'
INSERT INTO gasto (monto, nota, fecha_gasto, id_categoria, presupuesto_mensual_asignado) VALUES
    (50.00, 'Gasto 1', '2024-04-01', 1, 100.00),
    (75.25, 'Gasto 2', '2024-04-02', 2, 150.00),
    (30.50, 'Gasto 3', '2024-04-03', 3, 75.00),
    (100.75, 'Gasto 4', '2024-04-04', 4, 200.00);

-- Inserción de datos de prueba en la tabla 'sobrante_gasto'
INSERT INTO sobrante_gasto (cantidad_sobrante, id_gasto, id_monedero) VALUES
    (10.00, 1, 1),
    (20.25, 2, 2),
    (15.50, 3, 3),
    (30.75, 4, 4);

-- Inserción de datos de prueba en la tabla 'exceso_gasto'
INSERT INTO exceso_gasto (cantidad_exceso, id_gasto, id_monedero) VALUES
    (5.00, 1, 1),
    (10.25, 2, 2),
    (7.50, 3, 3),
    (15.75, 4, 4);

