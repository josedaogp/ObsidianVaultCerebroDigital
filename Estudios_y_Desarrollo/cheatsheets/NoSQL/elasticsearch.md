# 🔎 Cheatsheet de Elasticsearch

Este cheatsheet cubre los aspectos esenciales de Elasticsearch, el motor de búsqueda y análisis distribuido, de código abierto, basado en Lucene.

## 🔍 Conceptos Básicos

*   **Nodo (Node):** ⚙️ Instancia individual de Elasticsearch.
*   **Clúster (Cluster):** 📦 Conjunto de uno o más nodos que trabajan juntos.
*   **Índice (Index):** 🗄️ Colección de documentos con características similares (similar a una base de datos en un RDBMS).  Se divide en *shards*.
*   **Tipo (Type):** (Obsoleto en ES 7.x, eliminado en ES 8.x)  Subdivisión lógica dentro de un índice (similar a una tabla).  En versiones modernas, un índice tiene un solo tipo (`_doc`).
*   **Documento (Document):** 📄 Unidad básica de información en Elasticsearch.  Es un objeto JSON.
*   **Campo (Field):** 🔑 Clave en un documento (similar a una columna en una base de datos relacional).
*   **Shard:** ↔️ Subdivisión de un índice.  Elasticsearch distribuye los shards entre los nodos del clúster para escalabilidad y alta disponibilidad.
*   **Réplica (Replica):** ⚙️ Copia de un shard.  Proporciona alta disponibilidad y mejora el rendimiento de las búsquedas.
*   **Mapeo (Mapping):** 📝 Define cómo se almacena y se indexa un documento y sus campos (tipos de datos, analizadores, etc.).
*   **Analizador (Analyzer):** ⚙️ Procesa el texto para la indexación y la búsqueda (tokenización, stemming, eliminación de stop words, etc.).
*   **Inverted Index:** 🔎 Estructura de datos que mapea términos a los documentos que los contienen (fundamental para la búsqueda rápida).
*   **Relevancia (Relevance):** 📊 Puntuación que indica qué tan bien un documento coincide con una consulta.
* **API REST:** Elasticsearch expone una API REST sobre HTTP.

## 🚀 Instalación y Configuración

*   **Descarga:** [https://www.elastic.co/downloads/elasticsearch](https://www.elastic.co/downloads/elasticsearch)
*   **Ejecución:**  Descomprime el archivo y ejecuta `bin/elasticsearch` (Linux/macOS) o `bin\elasticsearch.bat` (Windows).
*   **Configuración:**  `config/elasticsearch.yml` (principalmente `cluster.name`, `node.name`, `network.host`, `http.port`).
*   **Plugins:**  Se instalan con `bin/elasticsearch-plugin install <plugin_name>`.

## 🌐 API REST

Elasticsearch se maneja principalmente a través de su API REST.  Los ejemplos usarán `curl`, pero puedes usar cualquier cliente HTTP.

*   **URL base:** `http://localhost:9200` (por defecto).
*   **Métodos HTTP:** `GET`, `POST`, `PUT`, `DELETE`.
*   **Formato de datos:** JSON.

## 🗄️ Gestión de Índices

1.  **Crear un índice:**

    ```bash
    PUT /mi_indice  # Nombre del índice en minúsculas
    {
      "settings": {
        "number_of_shards": 3,  # Número de shards (opcional)
        "number_of_replicas": 1  # Número de réplicas (opcional)
      },
      "mappings": {  # Mapeo (opcional, pero recomendado)
          "properties": {
            "campo1": { "type": "text" },
            "campo2": { "type": "integer" }
          }
      }
    }
    ```

2.  **Obtener la configuración de un índice:**

    ```bash
    GET /mi_indice
    GET /mi_indice/_settings
    ```

3.  **Eliminar un índice:**

    ```bash
    DELETE /mi_indice
    ```

4.  **Listar todos los índices:**

    ```bash
    GET /_cat/indices?v
    ```
    *   `v`:  Incluye encabezados.

5.  **Abrir/Cerrar un índice:**

    ```bash
    POST /mi_indice/_close
    POST /mi_indice/_open
    ```

6.  **Refrescar un índice (hacer que los cambios sean visibles para la búsqueda):**

    ```bash
    POST /mi_indice/_refresh
    ```

7. **Vaciar un índice (flush):**

   ```bash
    POST /mi_indice/_flush
   ```

8. **Estadísticas de un índice:**

   ```bash
    GET /mi_indice/_stats
   ```
9. **Alias:** Un nombre secundario para uno o más índices.
    ```bash
    # Crear
    POST /_aliases
    {
        "actions" : [
            { "add" : { "index" : "mi_indice", "alias" : "mi_alias" } }
        ]
    }

    # Eliminar
     POST /_aliases
    {
        "actions" : [
            { "remove" : { "index" : "mi_indice", "alias" : "mi_alias" } }
        ]
    }
    ```

## 📄 API de Documentos

1.  **Indexar (crear o actualizar) un documento:**

    ```bash
    PUT /mi_indice/_doc/1  # _doc es el tipo (único en ES 7.x+), 1 es el ID
    {
      "campo1": "valor1",
      "campo2": 123
    }

    # Auto-generar ID
    POST /mi_indice/_doc
    {
      "campo1": "valor2",
      "campo2": 456
    }
    ```

2.  **Obtener un documento:**

    ```bash
    GET /mi_indice/_doc/1
    ```

3.  **Actualizar un documento:**

    ```bash
    # Actualizar parcialmente (recomendado)
    POST /mi_indice/_update/1
    {
      "doc": {
        "campo2": 789
      }
    }

    # Actualizar con script (más flexible)
    POST /mi_indice/_update/1
    {
      "script": {
        "source": "ctx._source.campo2 += params.incremento",
        "params": {
          "incremento": 10
        }
      }
    }

    #Upsert (actualiza si existe, si no, inserta)
      POST /mi_indice/_update/1
    {
        "script" : {
            "source": "ctx._source.counter += params.count",
            "lang": "painless",
            "params" : {
                "count" : 4
            }
        },
        "upsert" : {
            "counter" : 1
        }
    }
    ```

4.  **Eliminar un documento:**

    ```bash
    DELETE /mi_indice/_doc/1
    ```

5.  **Operaciones masivas (Bulk API):**

    ```bash
    POST /_bulk
    { "index" : { "_index" : "mi_indice", "_id" : "1" } }
    { "campo1" : "valor1", "campo2" : 123 }
    { "update" : { "_index" : "mi_indice", "_id" : "2" } }
    { "doc" : { "campo2" : 456 } }
    { "delete" : { "_index" : "mi_indice", "_id" : "3" } }
    ```

6. **Obtener múltiples documentos:**
    ```bash
        GET /mi_indice/_mget
        {
            "docs" : [
                { "_id" : "1" },
                { "_id" : "2" }
            ]
        }

    # Desde diferentes índices
    GET /_mget
        {
          "docs": [
            {
              "_index": "index1",
              "_id": "1"
            },
            {
              "_index": "index2",
              "_id": "1"
            }
          ]
        }
    ```
7. **Actualizar por query:**

    ```bash
    POST /mi_indice/_update_by_query
    {
        "query": {
            "match": {
                "campo": "valor"
            }
        },
        "script": {
            "source": "ctx._source.campo += 1"
        }
    }
    ```

8. **Eliminar por query:**
    ```bash
    POST /mi_indice/_delete_by_query
       {
           "query": {
               "match": {
                   "campo": "valor"
               }
           }
       }
    ```
9. **Reindexar:** Copia documentos de un índice a otro.

    ```bash
    POST /_reindex
        {
          "source": {
            "index": "origen"
          },
          "dest": {
            "index": "destino"
          }
        }
    ```

## 🔎 Búsqueda (Search API)

```bash
GET /mi_indice/_search
{
  "query": {  # Parte de la consulta
    "match": {  # Tipo de consulta (match, term, range, etc.)
      "campo": "valor"  # Campo y valor a buscar
    }
  },
  "from": 0,  # Paginación: desplazamiento (opcional)
  "size": 10,  # Paginación: tamaño de página (opcional)
  "sort": [  # Ordenación (opcional)
    { "campo": "asc" },  # o "desc"
    "_score"
  ],
  "_source": ["campo1", "campo2"],  # Campos a devolver (opcional)
  "highlight": {  # Resaltado (opcional)
    "fields": {
      "campo": {}
    }
  }
}
```

*   **Tipos de consulta (dentro de `query`):**
    *   **`match`:**  Búsqueda de texto completo (analiza el texto).
    *   **`match_phrase`:**  Busca una frase exacta.
    *   **`match_phrase_prefix`:**  Busca una frase con prefijo en el último término.
    * **`multi_match`**: Busca en varios campos.
    *   **`term`:**  Busca un valor exacto (no analiza el texto).
    *   **`terms`:**  Busca varios valores exactos.
    *   **`range`:**  Busca en un rango de valores (`gte`, `gt`, `lte`, `lt`).
    *   **`exists`:**  Comprueba si un campo existe.
    *   **`prefix`:**  Busca por prefijo.
    *   **`wildcard`:**  Busca con comodines (`*`, `?`).
    *   **`regexp`:**  Busca con expresiones regulares.
    *   **`fuzzy`:**  Búsqueda difusa (corrige errores tipográficos).
    *   **`bool`:**  Combina múltiples consultas (`must`, `should`, `must_not`, `filter`).
    *   **`ids`:**  Busca por IDs.
    * **`query_string`**: Permite una sintaxis avanzada.

*   **`_source`:**  Especifica qué campos se devuelven en el resultado.

*   **`sort`:**  Ordena los resultados.

*   **`highlight`:**  Resalta las coincidencias en los resultados.

*   **`from` y `size`:**  Para paginación.

*  **`track_total_hits`**: Para saber el número total de hits.

**Ejemplos:**

```bash
# Búsqueda de texto completo
GET /mi_indice/_search
{
  "query": {
    "match": {
      "titulo": "búsqueda rápida"
    }
  }
}

# Búsqueda por rango
GET /mi_indice/_search
{
  "query": {
    "range": {
      "edad": {
        "gte": 18,
        "lte": 30
      }
    }
  }
}

# Búsqueda booleana
GET /mi_indice/_search
{
  "query": {
    "bool": {
      "must": [  # AND
        { "match": { "titulo": "elasticsearch" } },
        { "range": { "fecha": { "gte": "2023-01-01" } } }
      ],
      "must_not": [  # NOT
        { "term": { "estado": "borrador" } }
      ],
      "should": [  # OR
        { "match": { "autor": "Juan" } },
        { "match": { "autor": "Ana" } }
      ]
      ,
      "filter": [ # Como must, pero no afecta al scoring
        { "term":  { "status": "published" } }
      ]
    }
  }
}
```

## 📊 Agregaciones (Aggregations)

Las agregaciones permiten calcular estadísticas y agrupar datos.

```bash
GET /mi_indice/_search
{
  "size": 0,  # No devolver documentos (solo agregaciones)
  "aggs": {  # Agregaciones
    "promedio_edad": {  # Nombre de la agregación
      "avg": {  # Tipo de agregación (avg, sum, min, max, terms, etc.)
        "field": "edad"  # Campo a agregar
      }
    },
    "por_ciudad": {
      "terms": {
        "field": "ciudad.keyword", #Usar el campo keyword para agregaciones
        "size": 10  # Número de términos a devolver
      },
      "aggs": {  # Sub-agregaciones (opcional)
        "promedio_edad_por_ciudad": {
          "avg": {
            "field": "edad"
          }
        }
      }
    }
  }
}
```

*   **Tipos de agregación:**
    *   **Métricas:**
        *   `avg`: Media.
        *   `sum`: Suma.
        *   `min`: Mínimo.
        *   `max`: Máximo.
        *   `stats`: Estadísticas básicas (count, min, max, avg, sum).
        *   `extended_stats`: Estadísticas extendidas.
        *   `value_count`: Cuenta valores (incluyendo nulos).
        *  `percentiles`: Calcula percentiles.
        *  `percentile_ranks`: Rango de percentiles.
        *  `cardinality`: Cuenta valores únicos.
        *  `geo_bounds`: Coordenadas extremas de puntos.
        *  `geo_centroid`: Centroide de puntos.
        *  `top_hits`: Devuelve los N documentos más relevantes.
        * `scripted_metric`: Usa un script.

    *   **Bucketing:**
        *   `terms`: Agrupa por términos (valores de un campo).
        *   `range`: Agrupa por rangos de valores.
        *   `date_range`:  Agrupa por rangos de fechas.
        *   `date_histogram`:  Agrupa por intervalos de tiempo (día, mes, año, etc.).
        *   `histogram`:  Agrupa por intervalos numéricos.
        *   `geo_distance`:  Agrupa por distancias geográficas.
        *   `ip_range`:  Agrupa por rangos de IPs.
        * `filter`: Un solo bucket, con un filtro.
        * `filters`: Múltiples buckets, con diferentes filtros.
        * `missing`: Documentos sin un campo.
        * `nested`: Para campos nested.
        * `reverse_nested`: Para volver del contexto nested al principal.
        * `children`: Para relaciones parent-child.
        * `significant_terms`: Términos significativos.

    *   **Pipeline:**
      *  `bucket_selector`: Filtra buckets.
      *  `bucket_sort`: Ordena buckets
      * `serial_diff`: Calcula diferencias entre valores.

*   **`aggs` (o `aggregations`):**  Contiene las definiciones de las agregaciones.

*   Se pueden anidar agregaciones (sub-agregaciones).

## ✍️ Mapeos (Mappings)

El mapeo define cómo se almacena y se indexa un documento y sus campos.

```bash
PUT /mi_indice
{
  "mappings": {
    "properties": {
      "titulo": {
        "type": "text",  # Tipo de dato
        "analyzer": "spanish"  # Analizador (opcional)
      },
      "fecha": {
        "type": "date"
      },
      "edad": {
        "type": "integer"
      },
      "ubicacion": {
        "type": "geo_point"
      },
      "etiquetas": {
        "type": "keyword"  # No se analiza (para agregaciones, ordenación, filtros exactos)
      },
       "autor": {  # Objeto anidado (nested)
        "type": "object",
        "properties": {
          "nombre": { "type": "text" },
          "apellido": { "type": "text" }
        }
      }
    }
  }
}
```

*   **Tipos de datos comunes:**
    *   `text`:  Texto completo (analizado).
    *   `keyword`:  Cadena exacta (no analizado).
    *   `integer`, `long`, `short`, `byte`:  Enteros.
    *   `float`, `double`:  Números de punto flotante.
    *   `boolean`:  Booleano (`true`, `false`).
    *   `date`:  Fecha (con formato).
    *   `geo_point`:  Coordenadas geográficas (latitud, longitud).
    *   `geo_shape`:  Formas geométricas.
    *   `object`:  Objeto JSON anidado.
    *   `nested`:  Array de objetos JSON anidados (permite consultar cada objeto de forma independiente).
    *  `ip`: Dirección IP.
    *  `binary`: Datos binarios.
    *  `range`: Rangos (integer_range, float_range, long_range, double_range, date_range, ip_range)

*   **`analyzer`:**  Especifica el analizador a utilizar (para campos `text`).
* **`fields`**: Permite tener múltiples representaciones de un campo (ej. una versión `text` y otra `keyword`).

    ```
    "mi_campo": {
      "type": "text",
      "fields": {
        "keyword": {
          "type": "keyword"
        }
      }
    }
    ```
## ⚙️ Análisis de Texto (Analyzers)

El análisis de texto es el proceso de convertir texto en *tokens* (términos) que se indexan.

*   **Componentes de un analizador:**
    *   **Character filters:**  Preprocesan el texto (ej. reemplazan caracteres).
    *   **Tokenizer:**  Divide el texto en tokens (ej. por espacios en blanco).
    *   **Token filters:**  Modifican los tokens (ej. convierten a minúsculas, eliminan stop words, aplican stemming).

*   **Analizadores predefinidos:**
    *   `standard`:  Analizador por defecto (divide por palabras, convierte a minúsculas, elimina signos de puntuación).
    *   `simple`:  Divide por caracteres no alfabéticos, convierte a minúsculas.
    *   `whitespace`:  Divide por espacios en blanco.
    *   `stop`:  Como `simple`, pero también elimina *stop words* (palabras comunes).
    *   `keyword`:  No analiza el texto (trata todo el campo como un solo token).
    *   `pattern`:  Divide el texto según una expresión regular.
    *   `language analyzers`:  Analizadores específicos para diferentes idiomas (ej. `english`, `spanish`, `french`).

```bash
# Probar un analizador
POST /_analyze
{
  "analyzer": "spanish",
  "text": "El rápido zorro marrón salta sobre el perro perezoso."
}
```

## ✨ Otros Conceptos y Características

* **Ingest Pipelines:** Permiten preprocesar documentos antes de indexarlos.
* **Aliases:** Nombres alternativos para índices.
* **Templates:** Plantillas para la creación de índices.
* **Rollover API:** Crea automáticamente nuevos índices cuando se cumplen ciertas condiciones.
* **Shrink API:** Reduce el número de shards de un índice.
* **Split API:** Aumenta el número de shards de un índice.
* **Snapshot and Restore:** Copias de seguridad y restauración.
* **Watcher:** Alertas y notificaciones.
* **Graph:** Exploración de relaciones.
* **Machine Learning:** Detección de anomalías, clasificación, regresión, etc.
* **Canvas:** Visualización de datos.
* **SQL Access:** Permite consultar Elasticsearch con SQL.
* **Security:** Autenticación, autorización, encriptación.
* **Monitoring:** Monitorización del clúster.

Este cheatsheet cubre una amplia variedad de comandos, conceptos y características de Elasticsearch. ¡Guárdalo y consúltalo a menudo!