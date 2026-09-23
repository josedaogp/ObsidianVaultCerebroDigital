# 🔴 Cheatsheet de Redis

Este cheatsheet cubre los aspectos esenciales de Redis, la popular base de datos en memoria, de código abierto, que se usa como base de datos, caché, broker de mensajes, y motor de streaming.

## 🔍 Conceptos Básicos

*   **Clave-Valor (Key-Value):** 🔑 Modelo de datos fundamental de Redis.  Cada dato se almacena asociado a una clave única.
*   **En Memoria (In-Memory):** 🚀 Redis almacena los datos principalmente en memoria RAM, lo que permite un acceso extremadamente rápido.
*   **Persistencia (Persistence):** 💾 Opcional. Redis puede guardar los datos en disco para recuperarlos en caso de reinicio (RDB y AOF).
*   **Tipos de Datos (Data Types):** 🧮 Redis soporta varios tipos de datos (strings, lists, sets, sorted sets, hashes, bitmaps, hyperloglogs, geospatial indexes, streams).
*   **Transacciones (Transactions):** 묶음 Permiten ejecutar un conjunto de comandos como una unidad atómica.
*   **Pub/Sub (Publish/Subscribe):** 📢 Patrón de mensajería donde los publicadores envían mensajes a canales, y los suscriptores reciben los mensajes de los canales a los que están suscritos.
*   **Scripting Lua:** 📜 Permite ejecutar scripts Lua del lado del servidor para realizar operaciones complejas de forma atómica.
*   **Redis CLI (`redis-cli`):** 💻 Interfaz de línea de comandos para interactuar con Redis.
* **Pipeline:** Envía múltiples comandos al servidor sin esperar las respuestas, mejorando el rendimiento.

## 🚀 Conexión y Comandos Básicos (`redis-cli`)

1.  **Conectar a Redis (local, por defecto):**

    ```bash
    redis-cli
    ```

2.  **Conectar a un servidor específico:**

    ```bash
    redis-cli -h host -p puerto -a contraseña  # -h: host, -p: puerto (6379 por defecto), -a: contraseña
    ```

3.  **Seleccionar una base de datos (por defecto es 0):**

    ```
    SELECT número_base_de_datos  # Redis soporta múltiples bases de datos (0-15 por defecto)
    ```

4.  **Enviar un comando:**

    ```
    PING  # Comprueba si la conexión está activa (debería devolver PONG)
    ```

5. **Autenticación:**

    ```
    AUTH contraseña
    ```
6. **Información del servidor:**
    ```
        INFO
        INFO <sección> #Ej: INFO memory
    ```
7. **Monitorizar los comandos:**

    ```
    MONITOR #Muestra todos los comandos en tiempo real (cuidado en producción)
    ```

8. **Configuración:**
    ```
    CONFIG GET <parámetro>
    CONFIG SET <parámetro> <valor>
    CONFIG REWRITE # Guarda la configuración en el fichero.
    ```

9. **Clientes conectados:**
   ```
    CLIENT LIST
    CLIENT KILL <ip:puerto>
    CLIENT SETNAME <nombre>
    CLIENT GETNAME
   ```

10. **Latencia:**
    ```
    redis-cli --latency # Muestra la latencia
    redis-cli --latency-history # Muestra la latencia con el tiempo
    redis-cli --latency-dist # Distribución estadística.
    ```

11. **Salir de `redis-cli`:**

    ```
    QUIT
    -- o
    exit
    ```

## 🔑 Comandos de Clave (Key Commands)

*   `SET key value`: Establece el valor de una clave.
*   `GET key`: Obtiene el valor de una clave.
*   `DEL key [key ...]`: Elimina una o más claves.
*   `EXISTS key [key ...]`: Comprueba si una o más claves existen.
*   `EXPIRE key seconds`: Establece un tiempo de expiración (TTL) para una clave (en segundos).
*   `TTL key`: Obtiene el tiempo de vida restante de una clave (en segundos).  -1 si no tiene expiración, -2 si no existe.
*   `PERSIST key`: Elimina la expiración de una clave.
*   `KEYS pattern`: Busca claves que coinciden con un patrón (ej. `KEYS *`, `KEYS mi_clave*`).  *¡Usar con cuidado en producción!* Puede bloquear el servidor.
*   `RANDOMKEY`: Devuelve una clave aleatoria.
*   `RENAME key newkey`: Renombra una clave.
*   `RENAMENX key newkey`: Renombra una clave solo si la nueva clave no existe.
*  `TYPE key`: Devuelve el tipo de dato de una clave.
* `SCAN`: Itera sobre las claves de forma incremental (más seguro que `KEYS` en producción).
* `DUMP key`: Serializa una clave
* `RESTORE key ttl serialized-value`: Crea una clave a partir de una serialización.
* `OBJECT`: Inspecciona el estado interno de un objeto. Ej: `OBJECT refcount mykey`
*  `MOVE key db`: Mueve una clave a otra base de datos.

```
SET mi_clave "Hola Mundo"
GET mi_clave
EXPIRE mi_clave 60  # Expira en 60 segundos
TTL mi_clave
DEL mi_clave
```

## 🧮 Tipos de Datos y Comandos Específicos

### 1. Strings (Cadenas)

*   `SET key value`: Establece el valor de una clave (sobrescribe si ya existe).
*   `GET key`: Obtiene el valor de una clave.
*   `GETSET key value`: Establece un nuevo valor y devuelve el valor antiguo.
*   `APPEND key value`: Agrega un valor al final de una cadena.
*   `STRLEN key`: Obtiene la longitud de una cadena.
*   `INCR key`: Incrementa un valor numérico en 1.
*   `INCRBY key increment`: Incrementa un valor numérico en una cantidad específica.
*   `DECR key`: Decrementa un valor numérico en 1.
*   `DECRBY key decrement`: Decrementa un valor numérico en una cantidad específica.
*   `GETRANGE key start end`: Obtiene una subcadena.
*   `SETRANGE key offset value`: Sobrescribe parte de una cadena.
*  `MGET key1 [key2..]`: Obtiene los valores de múltiples claves.
*  `MSET key1 value1 [key2 value2..]`: Establece los valores de múltiples claves.
* `SETNX key value`: Establece el valor solo si la clave no existe.
* `SETEX key seconds value`: Establece el valor y un tiempo de expiración.
* `PSETEX key milliseconds value`: Similar a `SETEX` pero en milisegundos.

```
SET contador 10
INCR contador  # 11
DECRBY contador 3  # 8
```

### 2. Lists (Listas)

*   `LPUSH key value [value ...]`: Inserta uno o más valores al principio de la lista.
*   `RPUSH key value [value ...]`: Inserta uno o más valores al final de la lista.
*   `LPOP key`: Elimina y devuelve el primer elemento de la lista.
*   `RPOP key`: Elimina y devuelve el último elemento de la lista.
*   `LINDEX key index`: Obtiene un elemento por su índice.
*   `LLEN key`: Obtiene la longitud de la lista.
*   `LRANGE key start stop`: Obtiene un rango de elementos de la lista.
*   `LREM key count value`: Elimina elementos de la lista.
*   `LSET key index value`: Establece el valor de un elemento en un índice específico.
*   `LTRIM key start stop`: Recorta la lista a un rango específico.
*   `BLPOP key [key ...] timeout`:  Elimina y devuelve el primer elemento (bloqueante).
*   `BRPOP key [key ...] timeout`:  Elimina y devuelve el último elemento (bloqueante).
*  `RPOPLPUSH source destination`: Rota un elemento de una lista a otra.
*  `BRPOPLPUSH source destination timeout`: Versión bloqueante de `RPOPLPUSH`.

```
LPUSH mi_lista "manzana"
LPUSH mi_lista "plátano"
RPUSH mi_lista "naranja"
LRANGE mi_lista 0 -1  # Obtener todos los elementos
```

### 3. Sets (Conjuntos)

*   `SADD key member [member ...]`: Agrega uno o más miembros a un conjunto.
*   `SREM key member [member ...]`: Elimina uno o más miembros de un conjunto.
*   `SMEMBERS key`: Obtiene todos los miembros de un conjunto.
*   `SISMEMBER key member`: Comprueba si un miembro pertenece a un conjunto.
*   `SCARD key`: Obtiene el número de miembros de un conjunto.
*   `SINTER key [key ...]`: Intersección de conjuntos.
*   `SUNION key [key ...]`: Unión de conjuntos.
*   `SDIFF key [key ...]`: Diferencia de conjuntos.
*   `SPOP key [count]`: Elimina y devuelve uno o más miembros aleatorios.
*   `SRANDMEMBER key [count]`: Devuelve uno o más miembros aleatorios (sin eliminarlos).
* `SMOVE source destination member`: Mueve un miembro de un conjunto a otro.
* `SINTERSTORE destination key [key ...]`: Intersección y guarda el resultado.
* `SUNIONSTORE destination key [key ...]`: Unión y guarda.
* `SDIFFSTORE destination key [key ...]`: Diferencia y guarda.

```
SADD mi_conjunto "rojo"
SADD mi_conjunto "verde"
SADD mi_conjunto "azul"
SMEMBERS mi_conjunto
```

### 4. Sorted Sets (Conjuntos Ordenados)

*   `ZADD key [NX|XX] [GT|LT] [CH] [INCR] score member [score member ...]`: Agrega uno o más miembros a un conjunto ordenado, o actualiza su puntuación (score) si ya existen.
*   `ZREM key member [member ...]`: Elimina uno o más miembros de un conjunto ordenado.
*   `ZRANGE key start stop [WITHSCORES]`: Obtiene un rango de miembros por índice.
*   `ZREVRANGE key start stop [WITHSCORES]`: Obtiene un rango de miembros por índice (orden inverso).
*   `ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]`: Obtiene un rango de miembros por puntuación.
*   `ZREVRANGEBYSCORE key max min [WITHSCORES] [LIMIT offset count]`:  Rango por puntuación (orden inverso).
*   `ZSCORE key member`: Obtiene la puntuación de un miembro.
*   `ZCARD key`: Obtiene el número de miembros de un conjunto ordenado.
*   `ZCOUNT key min max`: Cuenta los miembros con puntuaciones dentro de un rango.
*   `ZRANK key member`: Obtiene el índice (rango) de un miembro.
*   `ZREVRANK key member`: Obtiene el índice (rango) de un miembro (orden inverso).
*   `ZINCRBY key increment member`: Incrementa la puntuación de un miembro.
* `ZINTERSTORE destination numkeys key [key ...] [WEIGHTS weight [weight ...]] [AGGREGATE SUM|MIN|MAX]`: Intersección y guarda.
* `ZUNIONSTORE destination numkeys key [key ...] [WEIGHTS weight [weight ...]] [AGGREGATE SUM|MIN|MAX]`: Unión y guarda.
* `ZREMRANGEBYRANK key start stop`: Elimina por rango
* `ZREMRANGEBYSCORE key min max`: Elimina por score.
* `ZRANGEBYLEX key min max`: Rango por orden lexicográfico.
* `ZLEXCOUNT key min max`: Cuenta por orden lexicográfico.
* `ZREMRANGEBYLEX key min max`: Elimina por orden lexicográfico.

```
ZADD mi_conjunto_ordenado 10 "manzana"
ZADD mi_conjunto_ordenado 20 "plátano"
ZADD mi_conjunto_ordenado 30 "naranja"
ZRANGE mi_conjunto_ordenado 0 -1 WITHSCORES  # Obtener todos los miembros con puntuaciones
```

### 5. Hashes (Diccionarios)

*   `HSET key field value [field value ...]`: Establece uno o más campos y valores en un hash.
*   `HGET key field`: Obtiene el valor de un campo.
*   `HMGET key field [field ...]`: Obtiene los valores de múltiples campos.
*   `HGETALL key`: Obtiene todos los campos y valores de un hash.
*   `HDEL key field [field ...]`: Elimina uno o más campos.
*   `HEXISTS key field`: Comprueba si un campo existe.
*   `HINCRBY key field increment`: Incrementa el valor de un campo numérico.
*   `HKEYS key`: Obtiene todos los campos de un hash.
*   `HLEN key`: Obtiene el número de campos de un hash.
*   `HVALS key`: Obtiene todos los valores de un hash.
* `HSETNX key field value`: Establece el valor si el campo no existe.
* `HSCAN key cursor [MATCH pattern] [COUNT count]`: Itera sobre los campos.

```
HSET mi_hash nombre "Juan" edad 30 ciudad "Madrid"
HGETALL mi_hash
```

### 6. Bitmaps

* `SETBIT key offset value`: Establece un bit.
* `GETBIT key offset`: Obtiene un bit.
* `BITCOUNT key [start end]`: Cuenta bits.
* `BITOP operation destkey key [key ...]`: Operaciones bitwise.
* `BITPOS key bit [start] [end]`: Encuentra el primer bit con valor.

### 7. HyperLogLogs

* `PFADD key element [element ...]`: Añade elementos
* `PFCOUNT key [key ...]`: Cuenta.
* `PFMERGE destkey sourcekey [sourcekey ...]`: Combina.

### 8. Geospatial Indexes

* `GEOADD key longitude latitude member [longitude latitude member ...]`: Añade.
* `GEODIST key member1 member2 [unit]`: Calcula la distancia.
* `GEOHASH key member [member ...]`: Obtiene el geohash.
* `GEOPOS key member [member ...]`: Obtiene la posición.
* `GEORADIUS key longitude latitude radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count] [ASC|DESC] [STORE key] [STOREDIST key]`: Búsqueda por radio.
* `GEORADIUSBYMEMBER key member radius m|km|ft|mi [WITHCOORD] [WITHDIST] [WITHHASH] [COUNT count] [ASC|DESC] [STORE key] [STOREDIST key]`: Búsqueda por radio, tomando un miembro como centro.

### 9. Streams

* `XADD key [NOMKSTREAM] [MAXLEN|MINID [=|~] threshold [LIMIT count]] *|ID field value [field value ...]`: Añade a un stream.
* `XREAD [COUNT count] [BLOCK milliseconds] STREAMS key [key ...] ID [ID ...]`: Lee de uno o varios streams.
* `XREVRANGE key end start [COUNT count]`: Rango inverso.
* `XRANGE key start end [COUNT count]`: Obtiene un rango.
* `XLEN key`: Obtiene la longitud.
* `XDEL key ID [ID ...]`: Elimina.
*`XGROUP CREATE key groupname id-or-$ [MKSTREAM] [ENTRIESREAD entries_read]`: Crea un grupo de consumidores
* `XGROUP CREATECONSUMER key groupname consumername`
* `XGROUP DELCONSUMER key groupname consumername`
* `XGROUP DESTROY key groupname`
* `XGROUP SETID key groupname id-or-$`
* `XINFO [CONSUMERS key groupname] [GROUPS key] [STREAM key] [HELP]`
* `XREADGROUP GROUP group consumer [COUNT count] [BLOCK milliseconds] [NOACK] STREAMS key [key ...] ID [ID ...]`
* `XPENDING key group [IDLE min_idle_time] [start end count] [consumer]`
* `XCLAIM key group consumer min_idle_time ID [ID ...] [IDLE idle_time] [TIME unix_time_ms] [RETRYCOUNT count] [FORCE] [JUSTID] [LASTID last_stream_id]`
* `XAUTOCLAIM key group consumer min_idle_time start [COUNT count] [JUSTID]`
* `XACK key group ID [ID ...]`
* `XTRIM key MAXLEN|MINID [=|~] threshold [LIMIT count]`

## 📢 Pub/Sub (Publicación/Suscripción)

*   `PUBLISH channel message`: Publica un mensaje en un canal.
*   `SUBSCRIBE channel [channel ...]`: Suscribe a uno o más canales.
*   `UNSUBSCRIBE [channel [channel ...]]`: Desuscribe de uno o más canales.
*   `PSUBSCRIBE pattern [pattern ...]`: Suscribe a canales que coinciden con un patrón.
*   `PUNSUBSCRIBE [pattern [pattern ...]]`: Desuscribe de patrones.
*   `PUBSUB subcommand [argument [argument ...]]`:  Comandos de inspección de Pub/Sub (ej. `PUBSUB CHANNELS`, `PUBSUB NUMSUB channel`, `PUBSUB NUMPAT`).

```
# Terminal 1 (suscriptor)
SUBSCRIBE mi_canal

# Terminal 2 (publicador)
PUBLISH mi_canal "Hola Mundo"
```

## 묶음 Transacciones

*   `MULTI`: Inicia una transacción.
*   `EXEC`: Ejecuta todos los comandos en la transacción.
*   `DISCARD`: Descarta la transacción (no ejecuta los comandos).
*   `WATCH key [key ...]`: Observa claves para detección de cambios (optimistic locking).
*   `UNWATCH`: Deshace la observación de claves.

```
MULTI
SET a 10
INCR a
EXEC  # Ejecuta los comandos (o DISCARD para cancelar)
```

## 📜 Scripting Lua

*   `EVAL script numkeys key [key ...] arg [arg ...]`: Ejecuta un script Lua.
*   `EVALSHA sha1 numkeys key [key ...] arg [arg ...]`: Ejecuta un script Lua por su hash SHA1 (después de cargarlo con `SCRIPT LOAD`).
*   `SCRIPT LOAD script`: Carga un script Lua en la caché de scripts.
*   `SCRIPT EXISTS sha1 [sha1 ...]`: Comprueba si un script existe en la caché.
*   `SCRIPT FLUSH`: Limpia la caché de scripts.
*   `SCRIPT KILL`: Detiene la ejecución de un script en ejecución.

```lua
-- Ejemplo de script Lua
local valor = redis.call('GET', KEYS[1])
if valor == ARGV[1] then
    redis.call('SET', KEYS[1], ARGV[2])
    return 1
else
    return 0
end

-- Ejecutar el script desde Redis CLI
EVAL "local valor = redis.call('GET', KEYS[1]) if valor == ARGV[1] then redis.call('SET', KEYS[1], ARGV[2]) return 1 else return 0 end" 1 mi_clave valor_antiguo valor_nuevo
```

## ⚙️ Configuración de Redis

*   **Archivo de configuración:**  `redis.conf` (normalmente en `/etc/redis/`).
*   **Opciones importantes:**
    *   `bind`:  Direcciones IP a las que escuchar.
    *   `port`:  Puerto (6379 por defecto).
    *   `requirepass`:  Contraseña.
    *   `maxmemory`:  Límite de memoria.
    *   `maxmemory-policy`:  Política de evicción cuando se alcanza el límite de memoria (ej. `noeviction`, `allkeys-lru`, `volatile-lru`, `allkeys-random`, `volatile-random`, `volatile-ttl`).
    *   `appendonly`:  Activa el modo AOF (persistencia).
    *   `appendfilename`:  Nombre del archivo AOF.
    *   `appendfsync`:  Frecuencia de sincronización con disco en modo AOF (ej. `always`, `everysec`, `no`).
    *   `save`:  Configuración de snapshots RDB (persistencia).
    *   `dbfilename`: Nombre del archivo RDB.
    * `dir`: Directorio de trabajo.

## 💾 Persistencia

1.  **RDB (Redis Database):**

    *   Snapshot del dataset en un momento dado.
    *   Se guarda en un archivo `.rdb`.
    *   Configuración: `save <segundos> <cambios>` (ej. `save 900 1` - guarda si hay al menos 1 cambio en 900 segundos).
    *   Comandos: `SAVE` (bloqueante), `BGSAVE` (no bloqueante).

2.  **AOF (Append Only File):**

    *   Registra cada operación de escritura en un archivo.
    *   Más durable que RDB, pero puede ser más grande.
    *   Configuración: `appendonly yes`.
    *   Comandos: `BGREWRITEAOF` (reescribe el AOF en segundo plano).

## ✨ Otros Comandos y Características

* **SLOWLOG:** Logs de comandos lentos.
* **DEBUG:** Comandos de depuración
* **Sentinel:** Para alta disponibilidad.
* **Cluster:** Para escalabilidad horizontal.

Este cheatsheet cubre una gran cantidad de comandos y conceptos de Redis, desde lo más básico hasta temas avanzados como transacciones, Pub/Sub, scripting Lua y la configuración. ¡Guárdalo y consúltalo a menudo!