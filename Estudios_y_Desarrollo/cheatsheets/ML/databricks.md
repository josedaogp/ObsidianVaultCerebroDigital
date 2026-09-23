# 📘 Databricks Cheatsheet

## 🧠 Índice
- [📘 Databricks Cheatsheet](#-databricks-cheatsheet)
  - [🧠 Índice](#-índice)
  - [✨ Comandos mágicos](#-comandos-mágicos)
  - [📓 Tips de Notebooks](#-tips-de-notebooks)
  - [🔍 Spark SQL](#-spark-sql)
  - [🐍 PySpark](#-pyspark)
    - [Funciones útiles](#funciones-útiles)
  - [🛠️ Utils de Databricks](#️-utils-de-databricks)
  - [🪄 Trucos y buenas prácticas](#-trucos-y-buenas-prácticas)
  - [📦 Bonus: Cargar datos fácilmente](#-bonus-cargar-datos-fácilmente)
  - [🧼 Limpieza final](#-limpieza-final)

---

## ✨ Comandos mágicos

```python
# MAGIC %python   - Ejecuta código en Python
# MAGIC %sql       - Ejecuta SQL
# MAGIC %scala     - Ejecuta Scala
# MAGIC %r         - Ejecuta R
# MAGIC %sh        - Ejecuta comandos de shell
# MAGIC %fs        - Accede al sistema de archivos DBFS
# MAGIC %run       - Ejecuta otro notebook
```

Ejemplo:

```sql
%sql
SELECT * FROM tabla LIMIT 10
```

---

## 📓 Tips de Notebooks

- `Shift + Enter`: Ejecutar celda
- `Ctrl + Enter`: Ejecutar sin cambiar de celda
- `Esc + A/B`: Insertar celda Arriba/Abajo
- `Cmd/Ctrl + /`: Comentar/descomentar líneas
- `dbutils.widgets`: Crear widgets interactivos

```python
dbutils.widgets.text("param", "default")
param = dbutils.widgets.get("param")
```

---

## 🔍 Spark SQL

```sql
-- Crear tabla
CREATE TABLE nombre_tabla (id INT, nombre STRING)

-- Leer tabla
SELECT * FROM nombre_tabla

-- Crear vista temporal
CREATE OR REPLACE TEMP VIEW vista_temp AS
SELECT * FROM otra_tabla

-- Funciones útiles
SELECT current_date(), current_timestamp()
SELECT count(*), avg(columna), max(columna)
```

---

## 🐍 PySpark

```python
# Leer archivo CSV
df = spark.read.csv("/path/file.csv", header=True, inferSchema=True)

# Mostrar contenido
df.show()

# Filtrar, seleccionar, ordenar
df.filter("edad > 30").select("nombre").orderBy("edad").show()

# Escribir a parquet
df.write.mode("overwrite").parquet("/ruta/salida")

# Crear vista temporal
df.createOrReplaceTempView("mi_vista")
```

### Funciones útiles

```python
from pyspark.sql.functions import col, when, lit

df = df.withColumn("nueva_col", when(col("valor") > 0, lit("positivo")).otherwise("negativo"))
```

---

## 🛠️ Utils de Databricks

```python
# Ver archivos
%fs ls /databricks-datasets/

# Copiar archivos
%fs cp /ruta/origen /ruta/destino

# DBUtils
dbutils.fs.ls("/path")
dbutils.fs.cp("dbfs:/file1", "dbfs:/file2")
dbutils.fs.rm("dbfs:/file", recurse=True)
dbutils.fs.mkdirs("dbfs:/nuevo_directorio")
```

---

## 🪄 Trucos y buenas prácticas

- **Persistencia**: Cachea datasets grandes si los usas varias veces:
  ```python
  df.cache()
  df.count()
  ```

- **Controlar particiones** para evitar demasiadas tareas pequeñas:
  ```python
  df = df.repartition(10)
  ```

- **Evita usar collect() con datasets grandes** (trae todo al driver y puede colapsar)

- **Evita loops en Spark**, usa funciones vectorizadas como `withColumn`, `select`, etc.

- **Control de versiones con Git**:
  - Habilita integración en "Repos"
  - Usa `%sh git` para comandos directamente desde notebooks

---

## 📦 Bonus: Cargar datos fácilmente

```python
# Desde tabla
df = spark.table("nombre_tabla")

# Desde parquet
df = spark.read.parquet("/ruta/file.parquet")

# Desde CSV
df = spark.read.option("header", "true").csv("/ruta/file.csv")

# Desde Delta
df = spark.read.format("delta").load("/ruta/delta_table")
```

---

## 🧼 Limpieza final

```python
# Eliminar caché
df.unpersist()

# Limpiar tablas temporales
spark.catalog.dropTempView("vista_temp")
```

---
