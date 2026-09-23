# 🐘 Cheatsheet de PostgreSQL

Este cheatsheet cubre los comandos y conceptos más importantes de PostgreSQL, el potente sistema de gestión de bases de datos relacionales de código abierto (RDBMS).

## 🔍 Conceptos Básicos

*   **Base de Datos (Database):** 🗄️ Colección organizada de datos.
*   **Tabla (Table):** 📊 Estructura que almacena datos en filas y columnas.
*   **Columna (Column):** 📏 Atributo de una tabla (ej. nombre, edad, email). Tiene un tipo de dato específico.
*   **Fila (Row):** ↔️ Registro individual en una tabla.
*   **Clave Primaria (Primary Key):** 🔑 Columna (o combinación de columnas) que identifica de forma única cada fila en una tabla.
*   **Clave Foránea (Foreign Key):** 🗝️ Columna en una tabla que hace referencia a la clave primaria de otra tabla. Establece una relación entre tablas.
*   **Índice (Index):** 🔎 Estructura que acelera la búsqueda de datos en una tabla.
*   **Consulta (Query):** ❓ Petición para obtener, insertar, actualizar o eliminar datos.
*   **Transacción (Transaction):** 묶음 Operación o conjunto de operaciones que se ejecutan como una unidad atómica (todo o nada).
*   **Vista (View):** 👀 Tabla virtual basada en el resultado de una consulta.
*   **Función (Function):** ⚙️ Bloque de código reutilizable que puede recibir parámetros y devolver un valor.
*   **Trigger (Disparador):** ⏰ Procedimiento que se ejecuta automáticamente en respuesta a un evento (INSERT, UPDATE, DELETE) en una tabla.
*   **Usuario (User):** 👤 Cuenta con permisos para acceder a la base de datos.  En PostgreSQL, los usuarios también se llaman roles.
*   **Rol (Role):** 👤 Grupo de privilegios que se pueden asignar a usuarios.
*   **Privilegio (Privilege):** ✅ Permiso específico (SELECT, INSERT, UPDATE, DELETE, etc.) otorgado a un usuario o rol.
*   **Esquema (Schema):** 🏘️ Espacio de nombres que contiene objetos de base de datos (tablas, vistas, funciones, etc.).  El esquema por defecto es `public`.
* **Secuencia (Sequence):** Objeto que genera una secuencia de números enteros.

## 🚀 Conexión y Comandos Básicos (`psql`)

1.  **Conectar a PostgreSQL (línea de comandos `psql`):**

    ```bash
    psql -U usuario -d base_de_datos -h host -p puerto  # -U: usuario, -d: base de datos, -h: host, -p: puerto (5432 por defecto)
    psql -U postgres #Conectar como superusuario
    ```

2.  **Mostrar bases de datos:**

    ```sql
    \l  -- (letra ele)
    -- o
    SELECT datname FROM pg_database;
    ```

3.  **Seleccionar una base de datos:**

    ```sql
    \c nombre_base_de_datos
    ```

4.  **Mostrar tablas:**

    ```sql
    \dt
    -- o
    SELECT tablename FROM pg_tables WHERE schemaname = 'public';  -- Tablas del esquema public
    ```
5.  **Mostrar esquemas**
    ```sql
    \dn
    -- o
    SELECT nspname FROM pg_namespace;
    ```

6.  **Describir una tabla (estructura):**

    ```sql
    \d nombre_tabla
    -- o
     SELECT column_name, data_type, character_maximum_length
     FROM information_schema.columns
     WHERE table_name = 'nombre_tabla';
    ```

7.  **Mostrar funciones:**

    ```sql
    \df
    ```

8.  **Mostrar triggers:**

    ```sql
    \dy
    ```
9. **Mostrar vistas:**
    ```sql
        \dv
    ```

10. **Mostrar usuarios/roles:**

    ```sql
    \du
    -- o
    SELECT rolname FROM pg_roles;
    ```

11. **Mostrar la versión de PostgreSQL:**

    ```sql
    SELECT version();
    ```

12. **Mostrar la base de datos actual:**

    ```sql
        SELECT current_database();
    ```
13. **Mostrar el usuario actual**
    ```sql
        SELECT current_user;
    ```

14. **Mostrar la configuración actual**
    ```sql
        SHOW ALL;
        SHOW <parámetro>; --Ej: SHOW TIMEZONE
    ```
15. **Ejecutar un script SQL:**

    ```sql
        \i ruta/al/archivo.sql
    ```

16. **Salir de `psql`:**

    ```sql
    \q
    ```

## 💾 Creación y Gestión de Bases de Datos y Tablas

1.  **Crear una base de datos:**

    ```sql
    CREATE DATABASE nombre_base_de_datos;
     CREATE DATABASE testdb
        WITH OWNER = testuser
        TEMPLATE = template0
        ENCODING = 'UTF8'
        LC_COLLATE = 'en_US.UTF-8'
        LC_CTYPE = 'en_US.UTF-8'
        CONNECTION LIMIT = -1; -- Opciones
    ```

2.  **Eliminar una base de datos:**

    ```sql
    DROP DATABASE nombre_base_de_datos;
    DROP DATABASE IF EXISTS nombre_base_de_datos;
    ```

3.  **Crear una tabla:**

    ```sql
    CREATE TABLE nombre_tabla (
        columna1 tipo_dato [NOT NULL] [DEFAULT valor] [PRIMARY KEY],
        columna2 tipo_dato [NOT NULL],
        columna3 tipo_dato,
        ...
        PRIMARY KEY (columna1),  -- Clave primaria (opcional si ya se definió en una columna)
        FOREIGN KEY (columna2) REFERENCES otra_tabla(otra_columna)  -- Clave foránea (opcional)
        CONSTRAINT nombre_restriccion UNIQUE (columna3) --Restricción UNIQUE
    );
    CREATE TABLE IF NOT EXISTS public.mi_tabla(...); --Con IF NOT EXISTS
    ```

4.  **Eliminar una tabla:**

    ```sql
    DROP TABLE nombre_tabla;
    DROP TABLE IF EXISTS nombre_tabla CASCADE; -- Elimina también objetos dependientes
    ```

5.  **Modificar una tabla (ALTER TABLE):**

    ```sql
    ALTER TABLE nombre_tabla ADD COLUMN nueva_columna tipo_dato;  -- Agregar columna
    ALTER TABLE nombre_tabla DROP COLUMN nombre_columna;  -- Eliminar columna
    ALTER TABLE nombre_tabla ALTER COLUMN nombre_columna TYPE nuevo_tipo_dato;  -- Modificar tipo de dato
    ALTER TABLE nombre_tabla RENAME COLUMN nombre_columna TO nuevo_nombre;  -- Renombrar columna
    ALTER TABLE nombre_tabla ADD PRIMARY KEY (columna);  -- Añadir clave primaria
    ALTER TABLE nombre_tabla DROP CONSTRAINT nombre_restriccion_pk; -- Eliminar clave primaria (nombre de la restricción)
    ALTER TABLE nombre_tabla ADD FOREIGN KEY (columna) REFERENCES otra_tabla(otra_columna);
    ALTER TABLE nombre_tabla DROP CONSTRAINT nombre_restriccion_fk; -- Eliminar clave foránea
    ALTER TABLE nombre_tabla ADD CONSTRAINT nombre_indice UNIQUE (columna); -- Clave única.
    ALTER TABLE nombre_tabla RENAME TO nuevo_nombre; -- Renombrar tabla
    ```

6. **Truncar una tabla:**
    ```sql
        TRUNCATE TABLE nombre_tabla;
    ```

## 🔠 Tipos de Datos

*   **Numéricos:**
    *   `SMALLINT`: Entero pequeño.
    *   `INTEGER` / `INT`: Entero.
    *   `BIGINT`: Entero grande.
    *   `DECIMAL(M, D)` / `NUMERIC(M, D)`: Número decimal exacto (M: dígitos totales, D: decimales).
    *   `REAL`: Número de punto flotante (precisión simple).
    *   `DOUBLE PRECISION`: Número de punto flotante (doble precisión).
    *   `SERIAL`: Entero autoincremental (equivalente a `INTEGER` con una secuencia).
    *   `BIGSERIAL`: Entero grande autoincremental.
*   **Cadenas de texto:**
    *   `CHARACTER(N)` / `CHAR(N)`: Cadena de longitud fija (N caracteres).
    *   `CHARACTER VARYING(N)` / `VARCHAR(N)`: Cadena de longitud variable (hasta N caracteres).
    *   `TEXT`: Cadena de longitud variable ilimitada.
*   **Fechas y horas:**
    *   `DATE`: Fecha (YYYY-MM-DD).
    *   `TIME`: Hora (HH:MI:SS).
    *   `TIMESTAMP`: Fecha y hora (YYYY-MM-DD HH:MI:SS).
    *   `TIMESTAMP WITH TIME ZONE`: Fecha y hora con zona horaria.
    *   `INTERVAL`:  Representa un intervalo de tiempo.
*   **Booleanos:**
    *   `BOOLEAN`:  `TRUE`, `FALSE`, o `NULL`.
*   **Otros:**
    *   `UUID`: Identificador único universal.
    *   `JSON` / `JSONB`:  Datos en formato JSON (JSONB es binario, más eficiente).
    *   `XML`: Datos en formato XML
    *   `INET`: Dirección IPv4 o IPv6.
    *   `CIDR`:  Bloque de red CIDR.
    *   `MACADDR`: Dirección MAC.
    *   `ARRAY`:  Arrays (ej. `INTEGER[]`, `TEXT[]`).
    * `ENUM`: Tipos enumerados.
    * `TSVECTOR` y `TSQUERY`: Para búsqueda de texto completo.
    * Geométricos: `POINT`, `LINE`, `LSEG`, `BOX`, `PATH`, `POLYGON`, `CIRCLE`
    * Rangos: `INT4RANGE`, `INT8RANGE`, `NUMRANGE`, `TSRANGE`, `TSTZRANGE`, `DATERANGE`

## ❓ Consultas (SELECT)

```sql
SELECT [DISTINCT] columna1, columna2, ...
FROM nombre_tabla
[WHERE condicion]
[GROUP BY columna]
[HAVING condicion_agrupada]
[ORDER BY columna [ASC | DESC]]
[LIMIT numero_filas [OFFSET desplazamiento]]
[FOR UPDATE]; -- Bloqueo de filas para actualización (dentro de una transacción)
```

*   `SELECT *`: Selecciona todas las columnas.
*   `DISTINCT`:  Devuelve valores únicos.
*   `WHERE`: Filtra los resultados según una condición.
*   `GROUP BY`: Agrupa los resultados por una o más columnas.
*   `HAVING`: Filtra los resultados agrupados.
*   `ORDER BY`: Ordena los resultados.
*   `LIMIT`: Limita el número de filas devueltas.
*   `OFFSET`: Especifica el desplazamiento.
*   `FOR UPDATE`:  Bloquea las filas seleccionadas para actualización (dentro de una transacción).  Evita que otras transacciones las modifiquen hasta que la transacción actual termine.

**Ejemplos:**

```sql
SELECT * FROM empleados;
SELECT nombre, edad FROM empleados WHERE edad > 30;
SELECT ciudad, COUNT(*) AS total FROM clientes GROUP BY ciudad HAVING total > 10;
SELECT * FROM productos ORDER BY precio DESC LIMIT 5;
SELECT * FROM pedidos LIMIT 10 OFFSET 5;
SELECT DISTINCT ciudad FROM clientes;
```

## ➕ Operadores

*   **Aritméticos:** `+`, `-`, `*`, `/`, `%`, `^` (exponenciación).
*   **De comparación:** `=`, `<>`, `!=`, `>`, `<`, `>=`, `<=`, `BETWEEN`, `IN`, `NOT IN`, `LIKE`, `ILIKE` (LIKE insensible a mayúsculas/minúsculas), `NOT LIKE`, `IS NULL`, `IS NOT NULL`, `IS DISTINCT FROM`, `IS NOT DISTINCT FROM`.
*   **Lógicos:** `AND`, `OR`, `NOT`.
*   **De concatenación:** `||`

**Ejemplos:**

```sql
SELECT * FROM productos WHERE precio BETWEEN 10 AND 20;
SELECT * FROM clientes WHERE ciudad IN ('Madrid', 'Barcelona');
SELECT * FROM empleados WHERE nombre LIKE 'J%';
SELECT * FROM empleados WHERE nombre ILIKE 'j%'; -- Insensible a mayúsculas/minúsculas
SELECT * FROM empleados WHERE apellido NOT LIKE '%ez';
SELECT * FROM productos WHERE stock IS NULL;
SELECT * FROM empleados WHERE nombre IS NOT DISTINCT FROM 'Juan'; -- Considera NULLs
SELECT 'Hola' || ' ' || 'Mundo'; -- Concatenación
```

## ✍️ Inserción, Actualización y Eliminación de Datos

1.  **INSERT (insertar):**

    ```sql
    INSERT INTO nombre_tabla (columna1, columna2, ...) VALUES (valor1, valor2, ...);
    INSERT INTO productos (nombre, precio) VALUES ('Teclado', 50), ('Ratón', 25);
    INSERT INTO tabla_destino (col1, col2) SELECT col1,col2 FROM tabla_origen;
    ```

2.  **UPDATE (actualizar):**

    ```sql
    UPDATE nombre_tabla SET columna1 = valor1, columna2 = valor2, ... WHERE condicion;
    UPDATE empleados SET salario = salario * 1.10 WHERE departamento = 'Ventas';
    ```
     *¡Cuidado!*  Si no usas `WHERE`, se actualizarán *todas* las filas.

3.  **DELETE (eliminar):**

    ```sql
    DELETE FROM nombre_tabla WHERE condicion;
    DELETE FROM clientes WHERE ciudad = 'Valencia';
    ```
    *¡Cuidado!*  Si no usas `WHERE`, se eliminarán *todas* las filas.

4. **UPSERT (Insertar o Actualizar):**

    ```sql
    INSERT INTO productos (id, nombre, precio)
    VALUES (1, 'Teclado', 50)
    ON CONFLICT (id) DO UPDATE SET nombre = EXCLUDED.nombre, precio = EXCLUDED.precio;
    -- EXCLUDED se refiere a los valores que se intentaron insertar
    ```

## 🔗 JOINs (Combinar Tablas)

*   **`INNER JOIN`:**
*   **`LEFT [OUTER] JOIN`:**
*   **`RIGHT [OUTER] JOIN`:**
*   **`FULL [OUTER] JOIN`:**
*  **`CROSS JOIN`**: Producto cartesiano.
*  **`NATURAL JOIN`**: Join implícito, basado en columnas con el mismo nombre.

```sql
-- INNER JOIN
SELECT p.nombre AS producto, c.nombre AS categoria
FROM productos p
INNER JOIN categorias c ON p.categoria_id = c.id;

-- LEFT JOIN
SELECT c.nombre AS cliente, p.fecha AS fecha_pedido
FROM clientes c
LEFT JOIN pedidos p ON c.id = p.cliente_id;
```

## ⚙️ Funciones

1.  **Funciones de agregación:**

    *   `COUNT()`
    *   `SUM()`
    *   `AVG()`
    *   `MIN()`
    *   `MAX()`

    ```sql
    SELECT COUNT(*) FROM empleados;
    SELECT AVG(salario) FROM empleados;
    ```

2.  **Funciones de cadena:**

    *   `CONCAT()` / `||`
    *   `LENGTH()` / `CHAR_LENGTH()` / `CHARACTER_LENGTH()`
    *   `UPPER()`
    *   `LOWER()`
    *   `SUBSTRING(cadena, inicio, longitud)`
    *   `TRIM()`
    *   `REPLACE()`
    *   `INITCAP()`:  Convierte la primera letra de cada palabra a mayúscula.
    * `POSITION(subcadena IN cadena)`: Posición de una subcadena
    * `LPAD()` y `RPAD()`: Rellenan una cadena

    ```sql
    SELECT CONCAT(nombre, ' ', apellido) AS nombre_completo FROM empleados;
    SELECT nombre || ' ' || apellido AS nombre_completo FROM empleados; -- Concatenación
    ```

3.  **Funciones de fecha y hora:**

    *   `NOW()`
    *   `CURRENT_DATE`
    *   `CURRENT_TIME`
    *   `CURRENT_TIMESTAMP`
    *   `EXTRACT(parte FROM fecha)`  (ej. `EXTRACT(YEAR FROM fecha_nacimiento)`)
    *   `DATE_TRUNC(parte, fecha)`
    *   `AGE(fecha1, fecha2)`
    * `TO_CHAR(fecha, formato)`

    ```sql
    SELECT TO_CHAR(fecha_nacimiento, 'DD/MM/YYYY') AS fecha_formateada FROM empleados;
    ```

4.  **Funciones de control de flujo:**
    *  `CASE WHEN condicion1 THEN valor1 WHEN condicion2 THEN valor2 ... ELSE valor_else END`
    * `COALESCE(valor1, valor2, ...)`:  Devuelve el primer valor no nulo.
    * `NULLIF(valor1, valor2)`:  Devuelve NULL si valor1 = valor2, de lo contrario devuelve valor1.

    ```sql
     SELECT nombre,
       CASE
           WHEN edad > 18 THEN 'Mayor de edad'
           ELSE 'Menor de edad'
       END AS estado
     FROM personas;
    ```

5. **Funciones matemáticas:**
    *  `ABS()`
    *  `ROUND()`
    *  `FLOOR()`
    * `CEIL()` / `CEILING()`
    * `MOD()`
    * `POWER()`
    * `SQRT()`
    * `RANDOM()`
    *  `GREATEST()`
    *  `LEAST()`
    * `TRUNC()`

6. **Funciones de conversión de tipo:**
    * `CAST(expresión AS tipo)`
    * `expresión::tipo` (sintaxis abreviada de PostgreSQL)

    ```sql
        SELECT CAST('123' AS INTEGER);
        SELECT '123'::INTEGER;
    ```

7. **Funciones de arrays:**
    *  `array_append()`, `array_prepend()`, `array_cat()`, `array_remove()`, `array_replace()`, `array_length()`, `array_position()`, `array_positions()`, `unnest()`

8. **Funciones de JSON:**
   * `->`, `->>`, `#>`, `#>>`, `json_array_length()`, `json_each()`, `json_each_text()`, `json_extract_path()`, `json_extract_path_text()`, `json_object_keys()`, `json_populate_record()`, `json_array_elements()`, `jsonb_set()`, `jsonb_insert()`, `jsonb_pretty()`

## 📝 Control de Flujo (CASE)

```sql
-- CASE
SELECT
    producto,
    CASE
        WHEN precio < 10 THEN 'Barato'
        WHEN precio < 50 THEN 'Medio'
        ELSE 'Caro'
    END AS rango_precio
FROM productos;
```

## 🔒 Transacciones

```sql
BEGIN;  -- Iniciar transacción (o START TRANSACTION)
-- Instrucciones SQL
COMMIT;  -- Confirmar los cambios
ROLLBACK;  -- Deshacer los cambios
```

*   **Niveles de aislamiento:**  `READ UNCOMMITTED`, `READ COMMITTED`, `REPEATABLE READ`, `SERIALIZABLE`.  (Por defecto: `READ COMMITTED`).

## 👀 Vistas

```sql
CREATE VIEW vista_empleados_ventas AS
SELECT e.nombre, e.apellido, d.nombre AS departamento
FROM empleados e
JOIN departamentos d ON e.departamento_id = d.id
WHERE d.nombre = 'Ventas';

SELECT * FROM vista_empleados_ventas;

DROP VIEW vista_empleados_ventas;

CREATE OR REPLACE VIEW vista_empleados_ventas AS ... ; --Modificar
```

## ⚙️ Funciones (Definidas por el Usuario)

```sql
CREATE FUNCTION sumar(a INTEGER, b INTEGER)
RETURNS INTEGER
AS $$
BEGIN
    RETURN a + b;
END;
$$ LANGUAGE plpgsql;

SELECT sumar(5, 3);

DROP FUNCTION sumar;
```

*   `CREATE [OR REPLACE] FUNCTION`:  Crea o reemplaza una función.
*   `RETURNS`:  Especifica el tipo de dato de retorno.
*   `$$`:  Delimitador de la función (se puede usar cualquier cadena, pero `$$` es común).
*   `LANGUAGE`:  Lenguaje de la función (ej. `plpgsql`, `sql`, `plpython3u`, etc.).
*   `IMMUTABLE`, `STABLE`, `VOLATILE`:  Indican el comportamiento de la función con respecto a la optimización de consultas.

## ⏰ Triggers (Disparadores)

```sql
-- 1. Crear la función trigger
CREATE FUNCTION actualizar_fecha_modificacion()
RETURNS TRIGGER
AS $$
BEGIN
    NEW.fecha_modificacion = NOW();  -- NEW se refiere a la nueva fila
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- 2. Crear el trigger
CREATE TRIGGER actualizar_fecha_modificacion_trigger
BEFORE UPDATE ON productos
FOR EACH ROW
EXECUTE FUNCTION actualizar_fecha_modificacion();

DROP TRIGGER actualizar_fecha_modificacion_trigger ON productos;
```

*   `BEFORE` / `AFTER`: Cuándo se ejecuta el trigger.
*   `INSERT`, `UPDATE`, `DELETE`: El evento.
*   `FOR EACH ROW` / `FOR EACH STATEMENT`:  Nivel de ejecución.
*   `NEW`:  Nuevos valores de la fila.
*   `OLD`:  Valores antiguos de la fila.
*   `EXECUTE FUNCTION`:  La función que se ejecuta.
*  `WHEN`: Se puede añadir una condición.

## 👤 Usuarios y Roles

```sql
CREATE ROLE nuevo_usuario WITH LOGIN PASSWORD 'contraseña';  -- Crear usuario (rol con LOGIN)
CREATE ROLE grupo; -- Crear rol
GRANT SELECT, INSERT ON mi_tabla TO nuevo_usuario;  -- Otorgar privilegios
REVOKE DELETE ON mi_tabla FROM nuevo_usuario;  -- Revocar privilegios
GRANT grupo TO nuevo_usuario;  -- Asignar rol a usuario
ALTER ROLE nuevo_usuario WITH CREATEDB; --Dar permisos de creación de base de datos.
ALTER ROLE nuevo_usuario WITH SUPERUSER; -- Hacerlo superusuario (cuidado)
ALTER USER nuevo_usuario WITH PASSWORD 'nueva_clave'; -- Cambiar clave
DROP ROLE nuevo_usuario;  -- Eliminar usuario/rol
```

*   `CREATE ROLE`:  Crea un usuario o un rol.
*   `LOGIN`:  Permite al rol iniciar sesión (es decir, es un usuario).
*   `PASSWORD`:  Establece la contraseña.
*   `GRANT`:  Otorga privilegios.
*   `REVOKE`:  Revoca privilegios.
*   `ALTER ROLE`:  Modifica un rol.

## 🏘️ Esquemas

*   Un esquema es un espacio de nombres que contiene objetos de base de datos (tablas, vistas, funciones, etc.).
*   El esquema por defecto es `public`.

```sql
CREATE SCHEMA mi_esquema;
SET search_path TO mi_esquema, public;  -- Establecer el orden de búsqueda de esquemas
SELECT * FROM mi_esquema.mi_tabla; --Acceder a la tabla en un esquema específico.
DROP SCHEMA mi_esquema;
```

## 🔎 Índices
```sql
    CREATE INDEX idx_nombre ON empleados (nombre);
    CREATE UNIQUE INDEX idx_email ON clientes (email);
    CREATE INDEX idx_compuesto ON tabla (col1, col2); -- Indice compuesto
    DROP INDEX idx_nombre;
    -- Tipos de índices:  B-tree (por defecto), Hash, GiST, SP-GiST, GIN, BRIN
    CREATE INDEX idx_gin ON tabla USING GIN (columna);
```

## ✨ Otros Comandos y Características

*   `EXPLAIN [ANALYZE] SELECT ...`:  Muestra el plan de ejecución de una consulta.
*   `VACUUM [FULL] [ANALYZE] tabla`:  Recupera espacio y actualiza estadísticas.
*   `ANALYZE tabla`:  Actualiza estadísticas para el optimizador de consultas.
*   `COPY`:  Importa/exporta datos desde/hacia archivos.
*  `\copy`: Lo mismo que `COPY`, pero desde el cliente `psql`.
*   `pg_dump` y `pg_restore`:  Herramientas para copias de seguridad y restauración.
*   Extensiones:  Amplían la funcionalidad de PostgreSQL (ej. `pg_trgm`, `hstore`, `uuid-ossp`, `postgis`).
*  `CREATE EXTENSION nombre_extension;`

Este cheatsheet de PostgreSQL es muy completo, y abarca una gran cantidad de comandos, conceptos y características. ¡Cópialo, pégalo y úsalo como referencia!