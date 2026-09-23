# 🍃 Cheatsheet de MongoDB

Este cheatsheet cubre los comandos y conceptos más importantes de MongoDB, la popular base de datos NoSQL orientada a documentos.

## 🔍 Conceptos Básicos

*   **Base de Datos (Database):** 🗄️ Contenedor de colecciones.
*   **Colección (Collection):** 📦 Grupo de documentos (similar a una tabla en bases de datos relacionales).
*   **Documento (Document):** 📄 Unidad básica de datos en MongoDB.  Es un objeto JSON (o BSON, la representación binaria de JSON).
*   **Campo (Field):** 🔑 Clave en un documento (similar a una columna en bases de datos relacionales).
*   **_id:** 🆔 Campo especial que sirve como clave primaria para cada documento.  MongoDB lo genera automáticamente si no se especifica.
*   **MongoDB Shell (`mongo` o `mongosh`):** 💻 Interfaz de línea de comandos para interactuar con MongoDB.  `mongosh` es la shell más moderna y recomendada.
*   **MongoDB Compass:** 🧭 Interfaz gráfica de usuario (GUI) para MongoDB.
*   **Operador (Operator):** ⚙️ Símbolo o palabra clave que se usa en consultas y actualizaciones para realizar operaciones (ej. `$eq`, `$gt`, `$and`, `$or`, `$set`, `$inc`).
*   **Agregación (Aggregation):** 📊 Proceso de transformar y combinar datos de múltiples documentos.
*   **Índice (Index):** 🔎 Estructura que acelera la búsqueda de documentos.
*   **Sharding:** ↔️ Distribución de datos entre múltiples servidores (para escalabilidad horizontal).
*   **Réplica Set (Replica Set):** ⚙️ Grupo de servidores MongoDB que mantienen copias de los mismos datos (para alta disponibilidad y redundancia).
*  **CRUD**: Create, Read, Update, Delete. Operaciones básicas.

## 🚀 Conexión y Comandos Básicos (`mongo` / `mongosh`)

1.  **Conectar a MongoDB (local, por defecto):**

    ```bash
    mongo  # Shell antigua (mongo)
    mongosh # Shell moderna (mongosh) - RECOMENDADA
    ```

2.  **Conectar a un servidor específico:**

    ```bash
    mongosh "mongodb://usuario:contraseña@host:puerto/base_de_datos?opciones"  # URI de conexión
    mongosh --host <host> --port <port> --username <usuario> --password <contraseña> --authenticationDatabase <bd_autenticacion>
    ```

3.  **Mostrar bases de datos:**

    ```javascript
    show dbs
    // o
    show databases
    ```

4.  **Seleccionar una base de datos:**

    ```javascript
    use nombre_base_de_datos
    ```

5.  **Mostrar colecciones:**

    ```javascript
    show collections
    ```

6.  **Mostrar el estado del servidor:**

    ```javascript
      db.serverStatus()
    ```

7. **Mostrar la base de datos actual**
   ```javascript
    db
   ```

8.  **Mostrar el usuario actual:**

    ```javascript
    db.runCommand({ connectionStatus: 1 })
    ```

9.  **Ejecutar un script JavaScript:**
    ```javascript
      load("mi_script.js")
    ```

10. **Salir de la shell:**

    ```javascript
    exit
    // o
    quit()
    ```

## 📝 Operaciones CRUD (Create, Read, Update, Delete)

### ✍️ Create (Insertar)

1.  **`insertOne()`:**  Inserta un documento.

    ```javascript
    db.coleccion.insertOne({
        nombre: "Juan",
        edad: 30,
        ciudad: "Madrid"
    });
    ```

2.  **`insertMany()`:**  Inserta varios documentos.

    ```javascript
    db.coleccion.insertMany([
        { nombre: "Ana", edad: 25 },
        { nombre: "Pedro", edad: 35 }
    ]);
    ```

3.  **`insert()`:**  (Obsoleto en versiones recientes) Inserta uno o varios documentos.

*Nota:* Si no se especifica el campo `_id`, MongoDB lo genera automáticamente como un `ObjectId`.

### 📖 Read (Consultar)

1.  **`find()`:**  Consulta documentos.

    ```javascript
    db.coleccion.find()  // Todos los documentos
    db.coleccion.find({})  // Todos los documentos (equivalente)
    db.coleccion.find({ edad: 30 })  // Documentos donde edad = 30
    db.coleccion.find({ edad: { $gt: 25 } })  // Documentos donde edad > 25 ($gt: greater than)
    db.coleccion.find({ciudad: "Madrid"}, {nombre: 1, edad: 1, _id: 0}) // Proyección: solo los campos nombre y edad
    ```

2.  **`findOne()`:**  Devuelve el primer documento que coincide con el criterio.

    ```javascript
    db.coleccion.findOne({ nombre: "Juan" })
    ```

3.  **`countDocuments()`/`count()`:** Cuenta documentos.
    ```javascript
     db.coleccion.countDocuments({edad: {$gt: 25}})
     db.coleccion.count({edad: {$gt: 25}}) // (Obsoleto en versiones recientes)
    ```

4.  **`distinct()`**: Devuelve valores distintos de un campo.
    ```javascript
    db.coleccion.distinct("ciudad")
    ```

### 🔄 Update (Actualizar)

1.  **`updateOne()`:**  Actualiza el primer documento que coincide con el criterio.

    ```javascript
    db.coleccion.updateOne(
        { nombre: "Juan" },  // Criterio de selección
        { $set: { edad: 31 } }  // Operación de actualización ($set: establece un campo)
    );
    ```

2.  **`updateMany()`:**  Actualiza todos los documentos que coinciden con el criterio.

    ```javascript
    db.coleccion.updateMany(
        { ciudad: "Madrid" },
        { $inc: { edad: 1 } }  // $inc: incrementa un campo
    );
    ```

3.  **`replaceOne()`:**  Reemplaza completamente un documento.

    ```javascript
    db.coleccion.replaceOne(
      { nombre: "Juan" },
      { nombre: "Juan Pérez", edad: 32, ciudad: "Barcelona" } // Nuevo documento
    );
    ```
4. **`update()`:** (Obsoleto en versiones recientes, usar `updateOne` o `updateMany`)

### 🗑️ Delete (Eliminar)

1.  **`deleteOne()`:**  Elimina el primer documento que coincide con el criterio.

    ```javascript
    db.coleccion.deleteOne({ nombre: "Juan" });
    ```

2.  **`deleteMany()`:**  Elimina todos los documentos que coinciden con el criterio.

    ```javascript
    db.coleccion.deleteMany({ ciudad: "Madrid" });
    ```

3.  **`remove()`:** (Obsoleto en versiones recientes, usar `deleteOne` o `deleteMany`)

4. **Eliminar una colección:**
    ```javascript
        db.coleccion.drop()
    ```

## ⚙️ Operadores de Consulta

*   **Comparación:**
    *   `$eq`: Igual a (`=`).
    *   `$ne`: No igual a (`!=`).
    *   `$gt`: Mayor que (`>`).
    *   `$gte`: Mayor o igual que (`>=`).
    *   `$lt`: Menor que (`<`).
    *   `$lte`: Menor o igual que (`<=`).
    *   `$in`: Coincide con cualquiera de los valores en un array.
    *   `$nin`: No coincide con ninguno de los valores en un array.

    ```javascript
    db.coleccion.find({ edad: { $in: [25, 30, 35] } })
    ```

*   **Lógicos:**
    *   `$and`: Y lógico.
    *   `$or`: O lógico.
    *   `$nor`: NO O lógico.
    *   `$not`: NO lógico (invierte el efecto de otro operador).

    ```javascript
    db.coleccion.find({ $and: [{ edad: { $gt: 25 } }, { ciudad: "Madrid" }] })
    db.coleccion.find({ $or: [{ edad: 25 }, { edad: 30 }] })
    ```

*   **De elemento:**
    *   `$exists`: Comprueba si un campo existe.
    *   `$type`: Comprueba el tipo de dato de un campo.

    ```javascript
    db.coleccion.find({ telefono: { $exists: true } })
    db.coleccion.find({ edad: { $type: "number" } })
    ```

*   **De evaluación:**
    *  `$expr`: Permite usar expresiones de agregación en la consulta.
    *   `$regex`:  Expresión regular.
    *   `$text`:  Búsqueda de texto completo.
    * `$mod`: Operador módulo
    * `$where`: Permite usar JavaScript (menos eficiente).

    ```javascript
    db.coleccion.find({ nombre: { $regex: /^J/, $options: "i" } })  // Empieza por J (insensible a mayúsculas/minúsculas)
    ```

*   **De array:**
    *   `$all`: Coincide con arrays que contienen todos los elementos especificados.
    *   `$elemMatch`: Coincide con documentos que contienen un elemento de array que cumple una condición.
    *   `$size`: Coincide con arrays de un tamaño específico.

    ```javascript
    db.coleccion.find({ etiquetas: { $all: ["tecnología", "informática"] } })
    ```
*  **De proyección**
    * `$`: Proyecta el primer elemento que coincida, en un array.
    * `$elemMatch`: Proyecta el primer elemento que coincida
    * `$meta`: Proyecta el metadato de la búsqueda de texto.
    * `$slice`: Limita el número de elementos de un array.

*  **Otros:**
    *  `$comment`: Añade un comentario a la consulta.

## 🔄 Operadores de Actualización

*   **De campos:**
    *   `$set`: Establece el valor de un campo.
    *   `$unset`: Elimina un campo.
    *   `$inc`: Incrementa el valor de un campo.
    *   `$mul`: Multiplica el valor de un campo.
    *   `$rename`: Renombra un campo.
    *   `$min`: Actualiza a un valor si es menor que el valor actual.
    *   `$max`: Actualiza a un valor si es mayor que el valor actual.
    *  `$setOnInsert`: Establece el valor de campos solo en inserción.

    ```javascript
    db.coleccion.updateOne({ _id: 1 }, { $set: { edad: 35, ciudad: "Barcelona" } })
    db.coleccion.updateOne({ _id: 1 }, { $inc: { contador: 1 } })
    ```

*   **De array:**
    *   `$push`: Agrega un elemento a un array.
    *   `$pop`: Elimina el primer o último elemento de un array.
    *   `$pull`: Elimina elementos de un array que coinciden con una condición.
    *   `$pullAll`: Elimina múltiples valores de un array.
    *   `$addToSet`: Agrega un elemento a un array solo si no existe ya (evita duplicados).
      * `$each`: Modificador para `$push` y `$addToSet`, para añadir múltiples elementos.
      * `$position`: Modificador para `$push` que indica la posición donde insertar.
      * `$sort`: Modificador para `$push` que permite ordenar.

    ```javascript
    db.coleccion.updateOne({ _id: 1 }, { $push: { etiquetas: "mongodb" } })
    db.coleccion.updateOne({_id: 1}, {$push: {puntuaciones: {$each: [50, 60, 70]}}})
    ```

*   **Bitwise:**
    *   `$bit`: Realiza operaciones bitwise (AND, OR, XOR).

* **Aislamiento:**
    * `$isolated`: Evita escrituras concurrentes.

## 📊 Agregación (Aggregation Framework)

El framework de agregación permite procesar y transformar datos.  Se basa en *pipelines* (tuberías) de etapas.

```javascript
db.coleccion.aggregate([
  { $match: { edad: { $gt: 25 } } },  // Etapa de filtrado
  { $group: { _id: "$ciudad", total: { $sum: 1 } } },  // Etapa de agrupación
  { $sort: { total: -1 } },  // Etapa de ordenación
  { $project: { _id: 0, ciudad: "$_id", total: 1 } },  // Etapa de proyección
  { $limit: 5 } // Limita los resultados
]);
```

*   **Etapas comunes:**
    *   `$match`: Filtra documentos.
    *   `$group`: Agrupa documentos.
    *   `$project`: Remodela documentos (añade, elimina, renombra campos).
    *   `$sort`: Ordena documentos.
    *   `$limit`: Limita el número de documentos.
    *   `$skip`: Salta un número de documentos.
    *   `$unwind`: "Desenrolla" un array (crea un documento por cada elemento del array).
    *   `$lookup`: Realiza un "left outer join" con otra colección.
    *   `$addFields`: Añade nuevos campos
    *   `$count`: Cuenta documentos.
    *   `$out`: Escribe los resultados a una nueva colección.
    *   `$merge`: Escribe los resultados en una colección, fusionando si ya existen.
    *  `$facet`: Permite multiples pipelines de agregación
    *  `$bucket`: Categoriza los documentos en grupos (buckets).
    * `$bucketAuto`: Como bucket, pero decide los límites automáticamente.
    * `$sample`: Selecciona una muestra aleatoria.
    * `$replaceRoot`: Reemplaza el documento de entrada
    * `$graphLookup`: Join recursivo en un grafo.

*   **Expresiones de agregación:**
    *   `$sum`: Suma valores.
    *   `$avg`: Calcula la media.
    *   `$min`: Obtiene el valor mínimo.
    *   `$max`: Obtiene el valor máximo.
    *   `$first`: Obtiene el primer valor del grupo.
    *   `$last`: Obtiene el último valor del grupo.
    *   `$push`: Agrega valores a un array.
    *   `$addToSet`: Agrega valores únicos a un array.
    *  `$stdDevPop`: Desviación estándar (población).
    *  `$stdDevSamp`: Desviación estándar (muestra).

## 🔎 Índices

```javascript
db.coleccion.createIndex({ campo: 1 });  // Crear índice ascendente
db.coleccion.createIndex({ campo: -1 });  // Crear índice descendente
db.coleccion.createIndex({ campo1: 1, campo2: -1 });  // Índice compuesto
db.coleccion.createIndex({ nombre: "text" }); // Índice de texto
db.coleccion.createIndex({ubicacion: "2dsphere"}); //Indice geoespacial
db.coleccion.createIndex({campo: 1}, {unique: true}); // Indice único
db.coleccion.createIndex({campo: 1}, {sparse: true}); // Indice sparse (no indexa si el campo no existe)
db.coleccion.createIndex({campo: 1}, {expireAfterSeconds: 3600}); // TTL index (autoeliminación)
db.coleccion.getIndexes();  // Mostrar índices
db.coleccion.dropIndex("nombre_indice");  // Eliminar índice
db.coleccion.dropIndexes(); // Elimina todos los índices
```

*   **Tipos de índices:**
    *   **Single Field:**  Índice en un solo campo.
    *   **Compound:**  Índice en múltiples campos.
    *   **Multikey:**  Índice en arrays.
    *   **Text:**  Índice para búsquedas de texto completo.
    *   **Hashed:**  Índice para sharding basado en hash.
    *   **Geospatial:**  Índice para datos geoespaciales.
*  **Propiedades de los índices**
    * `unique`: Que sea único.
    * `sparse`: Que sea sparse.
    * `expireAfterSeconds`: Para índices TTL

## 🗺️ Geoespacial

*   **Tipos de datos:**
    *   **GeoJSON:**  Formato estándar para representar datos geoespaciales (`Point`, `LineString`, `Polygon`, `MultiPoint`, `MultiLineString`, `MultiPolygon`, `GeometryCollection`).
    *   **Legacy Coordinates:**  Pares de coordenadas [longitud, latitud].

*   **Índices:**
    *   `2dsphere`:  Para datos GeoJSON y consultas en una esfera (Tierra).
    *   `2d`:  Para pares de coordenadas legacy y consultas en un plano.

*   **Operadores de consulta:**
    *   `$geoIntersects`:  Intersección geométrica.
    *   `$geoWithin`:  Completamente dentro de una geometría.
    *   `$near`:  Cerca de un punto.
    *   `$nearSphere`:  Cerca de un punto en una esfera.
    *  `$box`, `$polygon`, `$center`, `$centerSphere`: Para definir geometrías.

```javascript
// Ejemplo GeoJSON
{
  type: "Point",
  coordinates: [-73.9857, 40.7484]
}

db.lugares.createIndex({ ubicacion: "2dsphere" });

db.lugares.find({
  ubicacion: {
    $nearSphere: {
       $geometry: {
          type: "Point" ,
          coordinates: [ -73.9667, 40.78 ]
       },
       $maxDistance: 1000  // Distancia máxima en metros
    }
  }
});
```

## 👤 Usuarios y Roles (Autenticación y Autorización)

```javascript
//Habilitar la autenticación (si no está)
mongod --auth

// Conectar como administrador
use admin

// Crear un usuario
db.createUser({
    user: "miusuario",
    pwd: "micontraseña",
    roles: [
      { role: "readWrite", db: "mi_base_de_datos" },  // Rol y base de datos
      { role: "userAdmin", db: "admin" } // Para administrar usuarios
    ]
});

db.updateUser("miusuario", {pwd: "nuevacontraseña"}); // Actualizar usuario
db.dropUser("miusuario"); // Borrar usuario

// Roles predefinidos:  read, readWrite, dbAdmin, userAdmin, root, etc.

db.auth("miusuario", "micontraseña"); // Autenticarse
db.logout(); // Salir

db.getUsers(); // Ver usuarios
db.getRoles({showBuiltinRoles: true}); // Ver roles

// Roles personalizados
db.createRole({
  role: "miRolPersonalizado",
  privileges: [
     { resource: { db: "mi_base_de_datos", collection: "" }, actions: [ "find", "createCollection" ] },
     { resource: { db: "mi_base_de_datos", collection: "productos" }, actions: [ "update", "remove" ] }
  ],
  roles: []
});
db.dropRole("miRolPersonalizado");
```

## 🔄 Replica Set (Alta Disponibilidad y Redundancia)

*   Un replica set es un grupo de servidores MongoDB que mantienen copias de los mismos datos.
*   Hay un nodo *primario* (primary) que recibe todas las escrituras, y nodos *secundarios* (secondaries) que replican los datos del primario.
*   Si el primario falla, un secundario es elegido automáticamente como nuevo primario (failover).
*  Se configura en el fichero `mongod.conf`, o se puede usar `rs.initiate()` en la shell.

```javascript
// En la shell de MongoDB (conectado al primario)
rs.initiate();  // Iniciar un replica set (si es el primer nodo)
rs.conf();  // Ver la configuración
rs.status();  // Ver el estado
rs.add("host:puerto");  // Agregar un miembro
rs.remove("host:puerto"); // Eliminar un miembro
rs.isMaster(); //Comprueba si es el primario
```

## ↔️ Sharding (Escalabilidad Horizontal)

*   Sharding es la distribución de datos entre múltiples servidores (shards).
*   Permite escalar horizontalmente una base de datos MongoDB.
*  Se necesita:
    * **Shard servers:** Almacenan los datos.
    *  **Config servers:** Almacenan metadatos sobre el clúster sharded.
    * **`mongos` routers:** Enrutan las consultas a los shards correctos.

```javascript
// En la shell de MongoDB (conectado a un mongos router)
sh.enableSharding("mi_base_de_datos");  // Habilitar sharding para una base de datos
sh.shardCollection("mi_base_de_datos.mi_coleccion", { _id: "hashed" });  // Shardear una colección (por hash)
sh.addShard("host:puerto"); // Agregar un shard
sh.status();  // Ver el estado del clúster sharded
```

## 🛠️ Otras Herramientas y Comandos

*   **`mongod`:**  El proceso del servidor MongoDB.
*   **`mongos`:**  El proceso del router para clústeres sharded.
*   **`mongodump`:**  Herramienta para hacer copias de seguridad (exporta datos a archivos BSON).
*   **`mongorestore`:**  Herramienta para restaurar copias de seguridad.
*   **`mongoexport`:**  Exporta datos a JSON o CSV.
*   **`mongoimport`:**  Importa datos desde JSON o CSV.
*   **`mongostat`:**  Muestra estadísticas en tiempo real (similar a `top`).
*   **`mongotop`:**  Muestra estadísticas de tiempo de lectura/escritura.
*   **`explain()`:**  Muestra el plan de ejecución de una consulta.

```javascript
db.coleccion.find({ edad: { $gt: 25 } }).explain("executionStats")
```

* **CurrentOp:** Muestra las operaciones en curso.

```javascript
    db.currentOp()
    db.currentOp({ "command.aggregate": "collname" }) //Filtrando
    db.killOp(<opid>) // Matar una operación.
```

Este cheatsheet cubre una amplia gama de comandos, operadores y conceptos de MongoDB. ¡Espero que te sea muy útil!