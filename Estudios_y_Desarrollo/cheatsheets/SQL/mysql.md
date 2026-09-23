# 🐬 Cheatsheet de MySQL

Este cheatsheet cubre los comandos y conceptos más importantes de MySQL, el popular sistema de gestión de bases de datos relacionales (RDBMS).

## 🔍 Conceptos Básicos

*   **Base de Datos (Database):** 🗄️ Colección organizada de datos.
*   **Tabla (Table):** 📊 Estructura que almacena datos en filas y columnas.
*   **Columna (Column):** 📏 Atributo de una tabla (ej. nombre, edad, email).  Tiene un tipo de dato específico.
*   **Fila (Row):** ↔️ Registro individual en una tabla.
*   **Clave Primaria (Primary Key):** 🔑 Columna (o combinación de columnas) que identifica de forma única cada fila en una tabla.
*   **Clave Foránea (Foreign Key):** 🗝️ Columna en una tabla que hace referencia a la clave primaria de otra tabla.  Establece una relación entre tablas.
*   **Índice (Index):** 🔎 Estructura que acelera la búsqueda de datos en una tabla.
*   **Consulta (Query):** ❓ Petición para obtener, insertar, actualizar o eliminar datos.
*   **Transacción (Transaction):** 묶음 Operación o conjunto de operaciones que se ejecutan como una unidad atómica (todo o nada).
*   **Vista (View):** 👀 Tabla virtual basada en el resultado de una consulta.
*   **Procedimiento Almacenado (Stored Procedure):** 📦 Conjunto de instrucciones SQL precompiladas que se pueden ejecutar.
*   **Función (Function):** ⚙️ Similar a un procedimiento almacenado, pero devuelve un valor.
*   **Trigger (Disparador):** ⏰ Procedimiento que se ejecuta automáticamente en respuesta a un evento (INSERT, UPDATE, DELETE) en una tabla.
*   **Usuario (User):** 👤 Cuenta con permisos para acceder a la base de datos.
*   **Privilegio (Privilege):** ✅ Permiso específico (SELECT, INSERT, UPDATE, DELETE, etc.) otorgado a un usuario.
* **Motor de almacenamiento (Storage Engine):** Componente de MySQL que gestiona el almacenamiento y la recuperación de datos. InnoDB es el más común.

## 🚀 Conexión y Comandos Básicos

1.  **Conectar a MySQL (línea de comandos):**

    ```bash
    mysql -u usuario -p -h host  # -u: usuario, -p: contraseña, -h: host (opcional)
    ```

2.  **Mostrar bases de datos:**

    ```sql
    SHOW DATABASES;
    ```

3.  **Seleccionar una base de datos:**

    ```sql
    USE nombre_base_de_datos;
    ```

4.  **Mostrar tablas:**

    ```sql
    SHOW TABLES;
    ```

5.  **Describir una tabla (estructura):**

    ```sql
    DESCRIBE nombre_tabla;
    -- o
    SHOW COLUMNS FROM nombre_tabla;
    ```

6.  **Salir de MySQL:**

    ```sql
    exit;
    -- o
    quit;
    ```

## 💾 Creación y Gestión de Bases de Datos y Tablas

1.  **Crear una base de datos:**

    ```sql
    CREATE DATABASE nombre_base_de_datos;
    CREATE DATABASE IF NOT EXISTS nombre_bd; -- Crea si no existe
    ```

2.  **Eliminar una base de datos:**

    ```sql
    DROP DATABASE nombre_base_de_datos;
    DROP DATABASE IF EXISTS nombre_bd; -- Elimina si existe
    ```

3.  **Crear una tabla:**

    ```sql
    CREATE TABLE nombre_tabla (
        columna1 tipo_dato [NOT NULL] [AUTO_INCREMENT] [PRIMARY KEY],
        columna2 tipo_dato [NOT NULL],
        columna3 tipo_dato,
        ...
        PRIMARY KEY (columna1),  -- Clave primaria (opcional si ya se definió en una columna)
        FOREIGN KEY (columna2) REFERENCES otra_tabla(otra_columna)  -- Clave foránea (opcional)
    );

    CREATE TABLE IF NOT EXISTS nombre_tabla (...);
    ```

4.  **Eliminar una tabla:**

    ```sql
    DROP TABLE nombre_tabla;
    DROP TABLE IF EXISTS nombre_tabla;
    ```

5.  **Modificar una tabla (ALTER TABLE):**

    ```sql
    ALTER TABLE nombre_tabla ADD COLUMN nueva_columna tipo_dato;  -- Agregar columna
    ALTER TABLE nombre_tabla DROP COLUMN nombre_columna;  -- Eliminar columna
    ALTER TABLE nombre_tabla MODIFY COLUMN nombre_columna nuevo_tipo_dato;  -- Modificar tipo de dato
    ALTER TABLE nombre_tabla CHANGE COLUMN nombre_columna nuevo_nombre nuevo_tipo; -- Renombrar y/o modificar
    ALTER TABLE nombre_tabla ADD PRIMARY KEY (columna); -- Añadir clave primaria
    ALTER TABLE nombre_tabla DROP PRIMARY KEY; -- Eliminar clave primaria
    ALTER TABLE nombre_tabla ADD FOREIGN KEY (columna) REFERENCES otra_tabla(otra_columna); -- Añadir clave foránea
    ALTER TABLE nombre_tabla DROP FOREIGN KEY nombre_restriccion; -- Eliminar clave foránea.
    ALTER TABLE nombre_tabla ADD INDEX nombre_indice (columna); -- Añadir indice
    ALTER TABLE nombre_tabla RENAME TO nuevo_nombre; -- Renombrar
    ```
6. **Truncar una tabla (eliminar todos los datos, pero no la estructura):**
    ```sql
    TRUNCATE TABLE nombre_tabla;
    ```

## 🔠 Tipos de Datos

*   **Numéricos:**
    *   `TINYINT`: Entero muy pequeño.
    *   `SMALLINT`: Entero pequeño.
    *   `MEDIUMINT`: Entero mediano.
    *   `INT` / `INTEGER`: Entero.
    *   `BIGINT`: Entero grande.
    *   `FLOAT`: Número de punto flotante (precisión simple).
    *   `DOUBLE`: Número de punto flotante (doble precisión).
    *   `DECIMAL(M, D)` / `NUMERIC(M, D)`: Número decimal exacto (M: dígitos totales, D: decimales).
*   **Cadenas de texto:**
    *   `CHAR(N)`: Cadena de longitud fija (N caracteres).
    *   `VARCHAR(N)`: Cadena de longitud variable (hasta N caracteres).
    *   `TINYTEXT`: Texto muy pequeño.
    *   `TEXT`: Texto.
    *   `MEDIUMTEXT`: Texto mediano.
    *   `LONGTEXT`: Texto largo.
    *   `ENUM('valor1', 'valor2', ...)`:  Enumeración (solo permite valores específicos).
    *   `SET('valor1', 'valor2', ...)`: Conjunto (permite múltiples valores de una lista).
*   **Fechas y horas:**
    *   `DATE`: Fecha (YYYY-MM-DD).
    *   `TIME`: Hora (HH:MM:SS).
    *   `DATETIME`: Fecha y hora (YYYY-MM-DD HH:MM:SS).
    *   `TIMESTAMP`: Marca de tiempo (YYYY-MM-DD HH:MM:SS), se actualiza automáticamente.
    *   `YEAR`: Año (YYYY).
*   **Booleanos:**  No hay un tipo booleano nativo.  Se usa `TINYINT(1)` (0 para falso, 1 para verdadero).
*  **Binarios:**
    *   `BINARY(N)`
    *  `VARBINARY(N)`
    *  `TINYBLOB`
    *  `BLOB`
    * `MEDIUMBLOB`
    * `LONGBLOB`
* **JSON**: Para almacenar datos en formato JSON.

## ❓ Consultas (SELECT)

```sql
SELECT columna1, columna2, ...
FROM nombre_tabla
[WHERE condicion]
[GROUP BY columna]
[HAVING condicion_agrupada]
[ORDER BY columna [ASC | DESC]]
[LIMIT numero_filas [OFFSET desplazamiento]];
```

*   `SELECT *`: Selecciona todas las columnas.
*   `WHERE`: Filtra los resultados según una condición.
*   `GROUP BY`: Agrupa los resultados por una o más columnas.
*   `HAVING`: Filtra los resultados agrupados.
*   `ORDER BY`: Ordena los resultados.
    *   `ASC`: Ascendente (por defecto).
    *   `DESC`: Descendente.
*   `LIMIT`: Limita el número de filas devueltas.
*   `OFFSET`:  Especifica el desplazamiento (desde qué fila empezar).

**Ejemplos:**

```sql
SELECT * FROM empleados;  -- Todas las columnas y filas de la tabla empleados
SELECT nombre, edad FROM empleados WHERE edad > 30;
SELECT ciudad, COUNT(*) AS total FROM clientes GROUP BY ciudad HAVING total > 10;
SELECT * FROM productos ORDER BY precio DESC LIMIT 5;  -- 5 productos más caros
SELECT * FROM pedidos LIMIT 10 OFFSET 5; -- Desde el sexto pedido, 10 registros.
```

## ➕ Operadores

*   **Aritméticos:** `+`, `-`, `*`, `/`, `%`.
*   **De comparación:** `=`, `!=` (`<>`), `>`, `<`, `>=`, `<=`, `BETWEEN`, `IN`, `NOT IN`, `LIKE`, `NOT LIKE`, `IS NULL`, `IS NOT NULL`.
*   **Lógicos:** `AND`, `OR`, `NOT`.

**Ejemplos:**

```sql
SELECT * FROM productos WHERE precio BETWEEN 10 AND 20;
SELECT * FROM clientes WHERE ciudad IN ('Madrid', 'Barcelona');
SELECT * FROM empleados WHERE nombre LIKE 'J%';  -- Nombres que empiezan por J
SELECT * FROM empleados WHERE apellido NOT LIKE '%ez'; -- Apellidos que no terminen en "ez"
SELECT * FROM productos WHERE stock IS NULL;  -- Productos sin stock
```

## ✍️ Inserción, Actualización y Eliminación de Datos

1.  **INSERT (insertar):**

    ```sql
    INSERT INTO nombre_tabla (columna1, columna2, ...) VALUES (valor1, valor2, ...);
    INSERT INTO productos (nombre, precio) VALUES ('Teclado', 50), ('Ratón', 25); -- Múltiples filas

    -- Insertar desde otra tabla
    INSERT INTO tabla_destino (col1, col2)
    SELECT col1, col2 FROM tabla_origen;
    ```

2.  **UPDATE (actualizar):**

    ```sql
    UPDATE nombre_tabla SET columna1 = valor1, columna2 = valor2, ... WHERE condicion;
    UPDATE empleados SET salario = salario * 1.10 WHERE departamento = 'Ventas';  -- Aumento del 10%
    ```
     *¡Cuidado!*  Si no usas `WHERE`, se actualizarán *todas* las filas.

3.  **DELETE (eliminar):**

    ```sql
    DELETE FROM nombre_tabla WHERE condicion;
    DELETE FROM clientes WHERE ciudad = 'Valencia';
    ```
    *¡Cuidado!*  Si no usas `WHERE`, se eliminarán *todas* las filas.

## 🔗 JOINs (Combinar Tablas)

*   **`INNER JOIN`:** Devuelve filas cuando hay una coincidencia en ambas tablas.
*   **`LEFT JOIN`:** Devuelve todas las filas de la tabla izquierda, y las filas coincidentes de la tabla derecha (o NULL si no hay coincidencia).
*   **`RIGHT JOIN`:** Devuelve todas las filas de la tabla derecha, y las filas coincidentes de la tabla izquierda (o NULL si no hay coincidencia).
*   **`FULL OUTER JOIN`:** Devuelve todas las filas de ambas tablas (MySQL no lo soporta directamente, se puede emular con `LEFT JOIN` y `UNION`).

```sql
-- INNER JOIN
SELECT p.nombre AS producto, c.nombre AS categoria
FROM productos p
INNER JOIN categorias c ON p.categoria_id = c.id;

-- LEFT JOIN
SELECT c.nombre AS cliente, p.fecha AS fecha_pedido
FROM clientes c
LEFT JOIN pedidos p ON c.id = p.cliente_id;

-- Ejemplo de emulación de FULL OUTER JOIN (en MySQL)
SELECT * FROM tabla1
LEFT JOIN tabla2 ON tabla1.id = tabla2.id
UNION
SELECT * FROM tabla1
RIGHT JOIN tabla2 ON tabla1.id = tabla2.id;
```

## ⚙️ Funciones

1.  **Funciones de agregación:**

    *   `COUNT()`: Cuenta el número de filas.
    *   `SUM()`: Suma los valores de una columna.
    *   `AVG()`: Calcula la media de una columna.
    *   `MIN()`: Devuelve el valor mínimo.
    *   `MAX()`: Devuelve el valor máximo.

    ```sql
    SELECT COUNT(*) FROM empleados;  -- Número total de empleados
    SELECT AVG(salario) FROM empleados;  -- Salario medio
    ```

2.  **Funciones de cadena:**

    *   `CONCAT()`: Concatena cadenas.
    *   `LENGTH()`: Devuelve la longitud de una cadena.
    *   `UPPER()`: Convierte a mayúsculas.
    *   `LOWER()`: Convierte a minúsculas.
    *   `SUBSTRING()`: Extrae una subcadena.
    *   `TRIM()`: Elimina espacios en blanco al principio y al final.
    *   `REPLACE()`: Reemplaza subcadenas.

    ```sql
    SELECT CONCAT(nombre, ' ', apellido) AS nombre_completo FROM empleados;
    ```

3.  **Funciones de fecha y hora:**

    *   `NOW()`: Devuelve la fecha y hora actuales.
    *   `CURDATE()`: Devuelve la fecha actual.
    *   `CURTIME()`: Devuelve la hora actual.
    *   `DATE()`: Extrae la parte de la fecha.
    *   `YEAR()`: Extrae el año.
    *   `MONTH()`: Extrae el mes.
    *   `DAY()`: Extrae el día.
    *   `DATE_FORMAT()`: Formatea una fecha.

    ```sql
    SELECT DATE_FORMAT(fecha_nacimiento, '%d/%m/%Y') AS fecha_formateada FROM empleados;
    ```

4.  **Funciones de control de flujo:**
    *   `IF(condicion, valor_si_true, valor_si_false)`
    *   `CASE WHEN condicion1 THEN valor1 WHEN condicion2 THEN valor2 ... ELSE valor_else END`

    ```sql
    SELECT nombre, IF(edad > 18, 'Mayor de edad', 'Menor de edad') AS estado FROM personas;
    ```
5. **Funciones matemáticas:**
   * `ABS()`: Valor absoluto
   * `ROUND()`: Redondea
   * `FLOOR()`: Redondea hacia abajo
   * `CEIL()`/`CEILING()`: Redondea hacia arriba
   * `MOD()`: Resto de la división
   * `POW()`/`POWER()`: Potencia
   * `SQRT()`: Raíz cuadrada
   * `RAND()`: Número aleatorio.
   * `GREATEST()`: Mayor valor de una lista.
   * `LEAST()`: Menor valor de una lista.

6. **Funciones de conversión de tipo:**
    * `CAST(expresión AS tipo)`
    * `CONVERT(expresión, tipo)`

## 📝 Control de Flujo (IF, CASE)

```sql
-- IF
SELECT nombre, IF(edad >= 18, 'Adulto', 'Menor') AS estado FROM personas;

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
START TRANSACTION;  -- Iniciar transacción
-- Instrucciones SQL (INSERT, UPDATE, DELETE, etc.)
COMMIT;  -- Confirmar los cambios (hacerlos permanentes)
ROLLBACK;  -- Deshacer los cambios
```
*  Las transacciones garantizan que un conjunto de operaciones se ejecuten como una unidad atómica.  Si alguna operación falla, se deshacen todas (rollback).

## 👀 Vistas

*   Son tablas virtuales basadas en el resultado de una consulta `SELECT`.
*   No almacenan datos, sino que muestran los datos de las tablas subyacentes.
*   Útiles para simplificar consultas complejas, restringir el acceso a datos, etc.

```sql
CREATE VIEW vista_empleados_ventas AS
SELECT e.nombre, e.apellido, d.nombre AS departamento
FROM empleados e
JOIN departamentos d ON e.departamento_id = d.id
WHERE d.nombre = 'Ventas';

SELECT * FROM vista_empleados_ventas;  -- Usar la vista como una tabla
DROP VIEW vista_empleados_ventas; -- Eliminar
```

## 📦 Procedimientos Almacenados

*   Conjunto de instrucciones SQL precompiladas que se pueden ejecutar.
*   Pueden recibir parámetros de entrada y devolver valores.
*   Útiles para encapsular lógica de negocio, mejorar el rendimiento, etc.

```sql
DELIMITER //  -- Cambiar el delimitador (porque el cuerpo usa ;)

CREATE PROCEDURE obtener_empleados_por_departamento(IN departamento_nombre VARCHAR(255))
BEGIN
    SELECT *
    FROM empleados
    WHERE departamento_id = (SELECT id FROM departamentos WHERE nombre = departamento_nombre);
END //

DELIMITER ;  -- Restaurar el delimitador

CALL obtener_empleados_por_departamento('Ventas');  -- Llamar al procedimiento

DROP PROCEDURE obtener_empleados_por_departamento; -- Eliminar
```

## ⏰ Triggers (Disparadores)

*   Procedimientos que se ejecutan automáticamente en respuesta a un evento (INSERT, UPDATE, DELETE) en una tabla.
*   Útiles para auditoría, validación de datos, etc.

```sql
DELIMITER //

CREATE TRIGGER actualizar_fecha_modificacion
BEFORE UPDATE ON productos
FOR EACH ROW
BEGIN
    SET NEW.fecha_modificacion = NOW();  -- NEW se refiere a la nueva fila
END //

DELIMITER ;

DROP TRIGGER actualizar_fecha_modificacion; --Eliminar
```

*   `BEFORE` / `AFTER`:  Cuándo se ejecuta el trigger (antes o después del evento).
*   `INSERT`, `UPDATE`, `DELETE`:  El evento que desencadena el trigger.
*   `FOR EACH ROW`:  El trigger se ejecuta para cada fila afectada.
*   `NEW`:  Se refiere a los nuevos valores de la fila (en INSERT y UPDATE).
*   `OLD`:  Se refiere a los valores antiguos de la fila (en UPDATE y DELETE).

## 👤 Usuarios y Privilegios

```sql
CREATE USER 'nuevo_usuario'@'localhost' IDENTIFIED BY 'contraseña';  -- Crear usuario
GRANT SELECT, INSERT ON mi_base_de_datos.* TO 'usuario'@'localhost';  -- Otorgar privilegios
REVOKE DELETE ON mi_base_de_datos.* FROM 'usuario'@'localhost';  -- Revocar privilegios
SHOW GRANTS FOR 'usuario'@'localhost';  -- Mostrar privilegios
DROP USER 'usuario'@'localhost';  -- Eliminar usuario
SET PASSWORD FOR 'usuario'@'localhost' = PASSWORD('nueva_contraseña'); -- Cambiar contraseña.
FLUSH PRIVILEGES; -- Recargar los privilegios (importante después de cambios)
```
*   `'usuario'@'localhost'`:  Usuario y host (localhost, %, o una IP específica).
*   `ALL PRIVILEGES`:  Todos los privilegios (no recomendado para producción).

## 🔎 Índices
*   Mejoran la velocidad de las consultas, pero pueden ralentizar las operaciones de escritura (INSERT, UPDATE, DELETE).

```sql
CREATE INDEX idx_nombre ON empleados (nombre); -- Crear índice en la columna nombre
CREATE UNIQUE INDEX idx_email ON clientes (email); -- Crear índice único
SHOW INDEX FROM empleados; -- Listar los índices.
DROP INDEX idx_nombre ON empleados; -- Eliminar índice.
ALTER TABLE empleados ADD INDEX idx_apellido (apellido); -- Añadir usando ALTER
```

## ⚙️ Motores de Almacenamiento

*   **InnoDB:**  El motor por defecto.  Soporta transacciones, claves foráneas, bloqueo a nivel de fila.  Recomendado para la mayoría de los casos.
*   **MyISAM:**  No soporta transacciones ni claves foráneas.  Bloqueo a nivel de tabla.  Puede ser más rápido para lecturas en algunos casos.
*   **MEMORY:**  Almacena los datos en memoria (rápido, pero los datos se pierden al reiniciar el servidor).
*   **CSV:** Almacena en formato CSV.

```sql
CREATE TABLE mi_tabla (...) ENGINE = InnoDB;  -- Especificar el motor al crear la tabla
SHOW ENGINES; -- Ver motores disponibles.
```

## ✨ Otros Comandos y Características

*   `EXPLAIN SELECT ...`:  Muestra el plan de ejecución de una consulta (útil para optimización).
*   `USE information_schema`:  Accede al esquema de información (metadatos sobre bases de datos, tablas, columnas, etc.).
*   `SET GLOBAL ... / SET SESSION ...`:  Configura variables globales o de sesión.
*   `SHOW VARIABLES`:  Muestra variables del servidor.
*   `SHOW STATUS`:  Muestra el estado del servidor.
*   `mysqldump`:  Herramienta para hacer copias de seguridad de bases de datos.
*   `LOAD DATA INFILE`:  Importa datos desde un archivo.

Este completo cheatsheet de MySQL en Markdown cubre una amplia gama de comandos, conceptos y características, desde lo más básico hasta aspectos más avanzados. ¡Está listo para ser copiado, pegado y usado como referencia rápida!