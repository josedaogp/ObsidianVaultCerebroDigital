## Problema de que se tiene que meter el precio después
- Puedo meter un parámetro en la bbdd que sea si se ha seteado el precio de cada tipo de aceituna en la temporada actual. En principio estará a False, y cuando se modifique pasará a true. Entonces modificará todos los albaranes que se hayan metido en esa temporada, y entonces pasará a True. Cuando se cierre temporada, volverá a False.
- Otra opción bastante viable, es meter el tipo de aceituna todos los años, incluso asociado al id de temporada, y que al principio pueda ser null.
- También se puede hacer es que, al abrir temporada, el precio esté a nulo. En algún momento de la temporada, el usuario querrá meter el precio. En ese momento, todos los albaranes que tengan el id de aceituna que se ha modificado, añadirán el precio. (Supongo que calcularán el total del albarán). Hasta que esto no se haga, no se podrá cerrar temporada y no se podrá hacer las liquidaciones automáticas. Una vez se cierre la temporada, se volverá a poner a nulo el precio de la aceituna.

## Esquema BBDD
CREATE TABLE socios (
    id_socio SERIAL PRIMARY KEY,
    # id_localidad INT, # BORRADO
    # id_banco INT REFERENCES bancos(id_banco), # BORRADO JUNTO A LA FOREIGN KEY CONSTRAINT
    numero_cliente VARCHAR(50) UNIQUE NOT NULL,
    codigo_postal VARCHAR(10),
    activo BOOLEAN DEFAULT TRUE,
    dni VARCHAR(20) UNIQUE NOT NULL,
    nombre VARCHAR(255) NOT NULL,
    direccion TEXT,
    telefono1 VARCHAR(15),
    telefono2 VARCHAR(15),
    fax VARCHAR(15),
    movil VARCHAR(15),
    notas TEXT,
    numero_cuenta VARCHAR(34), -- IBAN
    saldo DECIMAL(12, 2) DEFAULT 0
);

CREATE TABLE albaranes (
    id_albaran SERIAL PRIMARY KEY,
    id_temporada INT REFERENCES temporadas(id_temporada),
    id_socio INT REFERENCES socios(id_socio),
    numero_albaran VARCHAR(50) UNIQUE NOT NULL,
    fecha_albaran DATE NOT NULL,
    tpc_iva DECIMAL(5, 2),
    base_imponible DECIMAL(12, 2),
    cuota_iva DECIMAL(12, 2),
    subtotal DECIMAL(12, 2),
    total_albaran DECIMAL(12, 2),
    observacion TEXT,
    bruto DECIMAL(12, 2),
    tara DECIMAL(12, 2),
    seleccionado BOOLEAN DEFAULT FALSE
);

CREATE TABLE anticipos (
    id_anticipo SERIAL PRIMARY KEY,
    id_temporada INT REFERENCES temporadas(id_temporada),
    id_socio INT REFERENCES socios(id_socio),
    numero_anticipo VARCHAR(50) UNIQUE NOT NULL,
    fecha_anticipo DATE NOT NULL,
    concepto TEXT,
    base_imponible DECIMAL(12, 2),
    iva DECIMAL(12, 2),
    compensacion DECIMAL(12, 2),
    total_factura DECIMAL(12, 2),
    retencion_irpf DECIMAL(5, 2),
    retenido DECIMAL(12, 2),
    importe DECIMAL(12, 2),
    cheque_numero VARCHAR(50)
);

CREATE TABLE tipos_aceituna (
    id_tipo_aceituna SERIAL PRIMARY KEY,
    id_variedad INT REFERENCES variedades(id_variedad),
    calibre VARCHAR(50),
    tipo_aceituna VARCHAR(255) NOT NULL,
    precio_por_kg DECIMAL(10, 2),
    gastos DECIMAL(10, 2),
    comision DECIMAL(5, 2),
    comentario TEXT
);

CREATE TABLE variedades (
    id_variedad SERIAL PRIMARY KEY,
    variedad VARCHAR(255) NOT NULL UNIQUE
);

CREATE TABLE temporadas (
    id_temporada SERIAL PRIMARY KEY,
    nombre_temporada VARCHAR(50) NOT NULL,
    fecha_inicio DATE NOT NULL,
    fecha_fin DATE
);

CREATE TABLE saldos (
    id_saldo SERIAL PRIMARY KEY,
    id_agricultor INT REFERENCES socios(id_socio),
    id_temporada INT REFERENCES temporadas(id_temporada),
    total_kg DECIMAL(12, 2),
    saldo DECIMAL(12, 2),
    fecha_liquidacion DATE
);

CREATE TABLE bancos (
    id_banco SERIAL PRIMARY KEY,
    codigo VARCHAR(50),
    nombre_banco VARCHAR(255) NOT NULL,
    telefono1 VARCHAR(15),
    telefono2 VARCHAR(15),
    fax VARCHAR(15),
    direccion TEXT,
    notas TEXT
);

CREATE TABLE parametros (
    id_parametro SERIAL PRIMARY KEY,
    iva DECIMAL(5, 2),
    unidad VARCHAR(50),
    comision DECIMAL(5, 2),
    retencion_irpf DECIMAL(5, 2),
);
