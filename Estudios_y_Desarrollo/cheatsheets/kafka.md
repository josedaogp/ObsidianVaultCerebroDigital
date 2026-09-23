#  Apache Kafka Cheatsheet

Este cheatsheet proporciona una referencia rápida a los conceptos y comandos más importantes de Apache Kafka.

## 🔍 Conceptos Básicos

*   **Kafka:** Plataforma distribuida de streaming de eventos (event streaming platform).
*   **Topic:**  Categoría o *feed* al que se publican los mensajes.  Esencialmente, un *topic* es como una tabla en una base de datos (sin las restricciones de un esquema fijo).
*   **Partition:**  Subdivisión de un *topic*.  Cada partición es un log ordenado, inmutable y *append-only*.  El paralelismo se logra a través de las particiones.
*   **Offset:**  Identificador único y secuencial de un mensaje dentro de una partición.
*   **Broker:**  Un servidor Kafka.  Un cluster de Kafka está formado por múltiples brokers.
*   **Producer:**  Aplicación que publica (escribe) mensajes en un *topic* de Kafka.
*   **Consumer:**  Aplicación que se suscribe (lee) a uno o más *topics* y procesa los mensajes.
*   **Consumer Group:**  Grupo de *consumers* que cooperan para consumir mensajes de uno o más *topics*.  Cada partición es consumida por exactamente un *consumer* dentro de cada grupo.
*   **Zookeeper:**  (Obsoleto a partir de Kafka 3.x con KRaft) Gestiona la metainformación del cluster (brokers, topics, consumers, etc.).  Kafka usaba ZooKeeper para el *cluster management*. A partir de Kafka 2.8 se puede correr sin ZK.
* **KRaft:** (Kafka Raft) Es el protocolo de consenso, implementa un *quorum service* basado en Raft, que remplaza a Zookeeper.
*   **Replication Factor:**  Número de copias de cada partición.  Proporciona alta disponibilidad y tolerancia a fallos.
*   **ISR (In-Sync Replicas):**  Conjunto de réplicas que están "al día" con el líder de la partición.
*   **Leader:**  El broker responsable de todas las lecturas y escrituras de una partición dada.
*   **Follower:**  Réplica pasiva que copia los datos del *leader*.
* **Controller:** Uno de los brokers se elige como controller y es responsable de asignar particiones a los brokers, y monitorizar fallos en los brokers.
*   **Segment:**  Un *topic partition* se divide en segmentos.  Un segmento es un archivo en disco.
*   **Retention:**  Política de retención de mensajes (por tiempo o tamaño).
*  **Message Key:** Clave opcional asociada a un mensaje.  Garantiza que los mensajes con la misma clave se escriban en la misma partición (y por lo tanto sean procesados en orden).
* **Schema Registry:** (No es parte intrínseca de Kafka, pero es muy común) Almacena y gestiona esquemas (ej. Avro, Protobuf, JSON Schema) para los mensajes.  Ayuda a garantizar la compatibilidad entre *producers* y *consumers*.

## 🚀 Comandos CLI (Línea de Comandos)

Estos comandos se ejecutan típicamente desde el directorio `bin/` de la instalación de Kafka.  A menudo se usa `kafka-topics.sh`, `kafka-console-producer.sh`, `kafka-console-consumer.sh`, etc.  Recuerda que los nombres de los scripts pueden variar ligeramente dependiendo de la versión de Kafka y del sistema operativo.

### 1.  `kafka-topics.sh` (Gestión de Topics)

*   **Crear un topic:**

    ```bash
    # Kafka con Zookeeper
    kafka-topics.sh --bootstrap-server localhost:9092 --create --topic mi-topic --partitions 3 --replication-factor 2

    # Kafka con KRaft
    kafka-topics.sh --bootstrap-server localhost:9092 --create --topic mi-topic --partitions 3 --replication-factor 2
    ```

    *   `--bootstrap-server`:  Lista de brokers (host:puerto).
    *   `--create`:  Indica la acción de crear.
    *   `--topic`:  Nombre del topic.
    *   `--partitions`:  Número de particiones.
    *   `--replication-factor`:  Factor de replicación.
    *   `--config <key>=<value>`: Configuración adicional (ej. `retention.ms`, `cleanup.policy`).
    *  `--if-not-exists`: El topic solo se creará si no existe.

*   **Listar topics:**

    ```bash
    kafka-topics.sh --bootstrap-server localhost:9092 --list
    ```

*   **Describir un topic:**

    ```bash
    kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic mi-topic
    ```

*   **Modificar un topic:**

    ```bash
    # Incrementar el número de particiones (solo se puede incrementar)
    kafka-topics.sh --bootstrap-server localhost:9092 --alter --topic mi-topic --partitions 5
    
    # Cambiar la configuración
     kafka-topics.sh --bootstrap-server localhost:9092 --alter --topic mi-topic --config retention.ms=604800000
    ```
     *  `--if-exists`: Modifica el topic solo si ya existe.

*   **Eliminar un topic:**

    ```bash
    kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic mi-topic
    ```
    *   `--if-exists`: Elimina el topic solo si existe.

### 2.  `kafka-console-producer.sh` (Productor de Consola)

*   **Enviar mensajes desde la consola:**

    ```bash
    kafka-console-producer.sh --bootstrap-server localhost:9092 --topic mi-topic
    # Escribe mensajes (cada línea es un mensaje) y presiona Ctrl+D para terminar.
    ```

    *   `--property <key>=<value>`:  Configuración adicional del producer (ej. `parse.key=true`, `key.separator=:`, `acks=all`).
    * `--key <key>`: Enviar mensaje con clave.

### 3.  `kafka-console-consumer.sh` (Consumidor de Consola)

*   **Leer mensajes desde la consola:**

    ```bash
    kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic mi-topic --from-beginning
    ```

    *   `--from-beginning`:  Lee todos los mensajes desde el inicio del topic.
    *   `--group <group-id>`:  Especifica el *consumer group*.
    *   `--property <key>=<value>`: Configuración adicional del consumer (ej. `print.key=true`, `key.separator=:`).
    *   `--offset <offset>`:  Comienza a consumir desde un *offset* específico.
    * `--partition <partition>`: Consume de una partición en específico.
    *   `--max-messages <n>`:  Consume un máximo de *n* mensajes.

### 4. `kafka-consumer-groups.sh` (Gestión de Consumer Groups)

* **Listar consumer groups:**

    ```bash
    kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
    ```
* **Describir consumer group:**
    ```bash
      kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group mi-grupo
    ```
* **Eliminar un consumer group:**

    ```bash
    kafka-consumer-groups.sh --bootstrap-server localhost:9092 --delete --group mi-grupo
    ```
    * `--all-groups`: Eliminar todos los grupos de consumidores.
* **Resetear offsets de un consumer group:**

   ```bash
   # Resetear al offset más temprano
    kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group mi-grupo --reset-offsets --to-earliest --topic mi-topic --execute

    # Resetear a un offset específico
    kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group mi-grupo --reset-offsets --to-offset 100 --topic mi-topic:0 --execute

    # Resetear por tiempo
    kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group mi-grupo --reset-offsets --to-datetime 2023-10-26T10:00:00.000 --topic mi-topic --execute

    #Resetear mediante un desplazamiento (hacia adelante o atrás)
     kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group mi-grupo --reset-offsets --by-duration PT1H --topic mi-topic --execute # Retrocede 1 hora.
   ```
    * `--topic <topic:partition>`: Especifica el topic y, opcionalmente, la partición.  Si no se especifica la partición, se aplica a todas las particiones del topic.
    * `--execute`: Realiza la operación (sin esto, solo se simula).
    * `--dry-run`: Solo simula la operación, no la ejecuta.
    * `--to-latest`: Mueve el offset al más reciente.
    * `--shift-by N`: Mueve el offset N posiciones (positivo o negativo).

### 5. `kafka-configs.sh`

* **Modificar la configuración de brokers, o topics a nivel dinámico:**

    ```bash
    # Alterar configuración de un topic.
    kafka-configs.sh --bootstrap-server localhost:9092 --alter --entity-type topics --entity-name mi-topic --add-config retention.ms=86400000

    # Alterar configuración de un broker
    kafka-configs.sh  --bootstrap-server localhost:9092 --alter --entity-type brokers --entity-name 0 --add-config log.retention.hours=168
    ```
    * `--entity-type`: brokers, topics, clients, users, etc.
    * `--entity-name`: El id del broker, nombre del topic, etc.
    * `--add-config`: Añadir o modificar una configuración.
    * `--delete-config`: Eliminar una configuración.

### 6.  `kafka-run-class.sh` (Herramienta genérica)

*   Ejecuta cualquier clase de Kafka que tenga un método `main`.  Se usa para herramientas menos comunes.

### 7.  Otros comandos útiles (menos frecuentes, pero importantes)

*   `kafka-replica-verification.sh`:  Verifica la consistencia de las réplicas.
*   `kafka-preferred-replica-election.sh`:  Realiza una elección de réplica preferida.
*   `kafka-log-dirs.sh`:  Describe o modifica los directorios de logs.
*   `kafka-server-start.sh`, `kafka-server-stop.sh`: Iniciar y detener un broker (usado normalmente por scripts de systemd, etc., no directamente por el usuario).
* `kafka-cluster.sh`: Utilidades para KRaft.
* `kafka-metadata-shell.sh`: Para interactuar con el cluster en modo KRaft.

## 💻 APIs de Programación (Java, Python, etc.)

Kafka ofrece APIs para varios lenguajes de programación.  Los más comunes son:

*   **Java:**  La API nativa de Kafka.  Ofrece el mayor rendimiento y funcionalidad.
    *   `org.apache.kafka.clients.producer`:  API de productor.
    *   `org.apache.kafka.clients.consumer`:  API de consumidor.
    *   `org.apache.kafka.streams`:  API de Kafka Streams (procesamiento de streams).
*   **Python:**  `confluent-kafka-python` (recomendado, basado en `librdkafka`), `kafka-python`.
*   **Go:**  `github.com/confluentinc/confluent-kafka-go` (basado en `librdkafka`).
*   **C/C++:**  `librdkafka` (biblioteca de alto rendimiento).
*   **Node.js:**  `node-rdkafka` (basado en `librdkafka`), `kafkajs`.
*   **.NET:** `Confluent.Kafka` (basado en `librdkafka`).

## 💡 Ejemplos de Código (Python - `confluent-kafka-python`)

### Productor

```python
from confluent_kafka import Producer

conf = {
    'bootstrap.servers': 'localhost:9092',  # Brokers
    'client.id': 'mi-productor'
}

producer = Producer(conf)

def delivery_report(err, msg):
    if err is not None:
        print(f'Error al entregar el mensaje: {err}')
    else:
        print(f'Mensaje entregado a {msg.topic()} [{msg.partition()}] at offset {msg.offset()}')

# Enviar mensajes
producer.produce('mi-topic', key='clave1', value='valor1', callback=delivery_report)
producer.produce('mi-topic', value='valor2', callback=delivery_report)

# Esperar a que se envíen todos los mensajes (importante)
producer.flush()
```

### Consumidor

```python
from confluent_kafka import Consumer, KafkaError

conf = {
    'bootstrap.servers': 'localhost:9092',
    'group.id': 'mi-grupo',
    'auto.offset.reset': 'earliest',  # 'latest', 'none'
    # 'enable.auto.commit': False # Para commits manuales
}

consumer = Consumer(conf)

consumer.subscribe(['mi-topic'])

try:
    while True:
        msg = consumer.poll(1.0)  # Timeout en segundos

        if msg is None:
            continue
        if msg.error():
            if msg.error().code() == KafkaError._PARTITION_EOF:
                print('Fin de la partición')
            else:
                print(f'Error al consumir: {msg.error()}')
        else:
            print(f'Recibido: {msg.topic()} [{msg.partition()}] at offset {msg.offset()}: key={msg.key()}, value={msg.value()}')
            # consumer.commit(msg)  # Commit manual (si enable.auto.commit=False)
except KeyboardInterrupt:
    pass
finally:
    consumer.close()
```

## 📚 Buenas Prácticas y Consideraciones

*   **Elige el número correcto de particiones:**  Determina el paralelismo de consumo.  Piensa en el *throughput* deseado y el número de *consumers*.  No se puede reducir el número de particiones, solo aumentar.
*   **Usa un factor de replicación adecuado:**  Normalmente 3 para producción.
*   **Monitoriza el cluster:**  *Consumer lag*, uso de disco, uso de CPU/memoria/red de los brokers, etc.  Usa herramientas como Kafka Manager, Burrow, Prometheus + Grafana, o soluciones comerciales.
*   **Gestiona los *offsets* correctamente:**  Decide entre *at-least-once*, *at-most-once* o *exactly-once* delivery semantics.
*   **Usa *consumer groups* para escalabilidad y tolerancia a fallos.**
*   **Considera el uso de un Schema Registry.**
*   **Ajusta la configuración de *producers* y *consumers*:**  `batch.size`, `linger.ms`, `acks` (producer); `fetch.min.bytes`, `fetch.max.wait.ms` (consumer).
* **Comprende el modelo de consistencia de Kafka.**
* **Seguridad:** Habilitar la autenticación (SASL, Kerberos), autorización (ACLs) y encriptación (TLS/SSL).
* **Kafka Streams:** Si necesitas procesamiento de *streams* (transformaciones, agregaciones, joins, etc.), considera usar Kafka Streams en lugar de *consumers* y *producers* básicos.

Este cheatsheet proporciona una visión general de los aspectos más importantes de Apache Kafka.  ¡Recuerda consultar la documentación oficial para obtener información más detallada!


Sí, hay varias alternativas y tecnologías complementarias a Kafka que son importantes considerar en el desarrollo de sistemas de streaming y mensajería, dependiendo de los requisitos específicos del proyecto. Aquí te presento algunas de las más relevantes, divididas en categorías:

**1. Alternativas Directas (Mensajería y Streaming):**

*   **Apache Pulsar:**
    *   Similar a Kafka, pero con algunas diferencias arquitectónicas clave.
    *   Modelo de mensajería unificado: Combina colas de mensajes (queues) y streaming (topics) en un solo sistema.  Kafka, tradicionalmente, se ha centrado más en streaming.
    *   Almacenamiento en capas (Tiered Storage): Puede descargar datos más antiguos a almacenamiento más barato (ej. S3, GCS) para retención a largo plazo. Kafka requiere plugins (como el de Confluent) para esto.
    *   Funciones (Pulsar Functions): Framework ligero para procesamiento de eventos (similar a Kafka Streams, pero más simple y con soporte para múltiples lenguajes).
    *   Multi-tenancy: Soporte nativo para múltiples *tenants* (aislamiento de recursos).
    *   Georeplicación: Replicación síncrona y asíncrona entre clusters en diferentes regiones.
*   **RabbitMQ:**
    *   Un broker de mensajes tradicional (implementa AMQP - Advanced Message Queuing Protocol).
    *   Muy maduro y ampliamente utilizado.
    *   Enfoque en mensajería, no tanto en streaming de alta velocidad.
    *   Soporte para varios patrones de mensajería (publish-subscribe, point-to-point, request-reply).
    *   No es tan escalable horizontalmente como Kafka o Pulsar para casos de uso de streaming de alto volumen.
*   **NATS:**
    *   Sistema de mensajería ligero y de alto rendimiento.
    *   Muy simple de configurar y usar.
    *   Bueno para casos de uso de baja latencia y alta disponibilidad.
    *   Menos funcionalidades que Kafka o Pulsar (no tiene persistencia, por ejemplo, en su configuración básica; NATS Streaming/JetStream añaden algunas capacidades).
    *   Orientado a mensajería efímera (no persistente) en su configuración por defecto (aunque NATS JetStream añade persistencia).
* **Redis Streams:**
    * Redis, conocido por su base de datos en memoria, también tiene una funcionalidad de streaming llamada Redis Streams.
    * Muy rápido, ideal para casos que requieren muy baja latencia.
    * Más simple que Kafka, pero menos funcionalidades y escalabilidad.
    * No está diseñado para ser un "data lake" como Kafka, sino para flujos de datos más efímeros y de menor volumen.
* **Amazon Kinesis Data Streams:**
    * Servicio de streaming de datos en tiempo real de AWS.
    * Similar a Kafka en concepto, pero como servicio gestionado.
    * Integración nativa con otros servicios de AWS (ej. Lambda, S3, Redshift).
*   **Azure Event Hubs:**
    *   Servicio de ingesta de eventos de Azure.
    *   Similar a Kafka y Kinesis, pero integrado con el ecosistema de Azure.
* **Google Cloud Pub/Sub:**
     * Servicio de mensajería asíncrona de Google Cloud.
     * Modelo *publish-subscribe*.
     * Escalable y gestionado.
     * No tiene el concepto de *partitions* ordenadas como Kafka, lo que lo hace más simple, pero menos adecuado para casos donde el orden estricto es crucial.

**2. Tecnologías Complementarias (Procesamiento de Streams):**

*   **Apache Flink:**
    *   Motor de procesamiento de *streams* y *batch* de alto rendimiento.
    *   Puede consumir datos de Kafka (y otras fuentes).
    *   Ofrece procesamiento *stateful* (con estado) y *exactly-once* semantics.
    *   Soporte para SQL y APIs de alto nivel (DataStream API, Table API).
    *   Muy utilizado para análisis en tiempo real, detección de anomalías, etc.
*   **Apache Spark Streaming / Structured Streaming:**
    *   Spark Streaming es una extensión de Spark para procesamiento de *streams* (micro-batches).
    *   Structured Streaming es una API de más alto nivel construida sobre Spark SQL que proporciona procesamiento *stateful* y *exactly-once*.
    *   Integración con Kafka.
*   **Apache Beam:**
    *   Modelo de programación unificado para definir pipelines de procesamiento de datos *batch* y *streaming*.
    *   Puede ejecutar pipelines en diferentes motores (Flink, Spark, Google Cloud Dataflow, etc.).
    *   Portabilidad entre diferentes *runners*.
* **ksqlDB:**
     * Base de datos de streaming construida sobre Kafka Streams.
     * Permite consultar y procesar streams de Kafka usando SQL.
     * Muy útil para análisis en tiempo real y creación de aplicaciones basadas en eventos.

**3. Otras Consideraciones:**

*   **Schema Registry (Confluent Schema Registry, Apicurio Registry):** Fundamental para la gestión de esquemas en un entorno de streaming, especialmente cuando se usan formatos como Avro, Protobuf o JSON Schema.  Ayuda a mantener la compatibilidad entre productores y consumidores.
*   **Herramientas de Monitoreo (Prometheus, Grafana, Datadog, etc.):**  Esenciales para observar el estado del cluster de Kafka (o de cualquier sistema de streaming), el *consumer lag*, el uso de recursos, etc.
*  **Conectores (Kafka Connect):** Ecosistema de conectores para integrar Kafka con diversas fuentes y destinos de datos (bases de datos, sistemas de archivos, etc.).

**Cómo Elegir:**

La elección de la tecnología adecuada depende de varios factores:

*   **Volumen y velocidad de los datos:**  ¿Necesitas alta velocidad y volumen (terabytes/día)? Kafka y Pulsar son buenas opciones.  ¿Volúmenes más moderados? RabbitMQ o Redis Streams podrían ser suficientes.
*   **Requisitos de latencia:**  ¿Necesitas latencia muy baja (milisegundos)? Redis Streams o NATS podrían ser adecuados.
*   **Orden de los mensajes:**  ¿Es crucial el orden estricto de los mensajes? Kafka lo garantiza dentro de una partición.  Pub/Sub, por ejemplo, no lo garantiza.
*   **Modelo de mensajería:**  ¿Necesitas colas de mensajes tradicionales (point-to-point) o streaming (publish-subscribe)? RabbitMQ se centra en colas, Kafka en streaming, Pulsar combina ambos.
*   **Persistencia:**  ¿Necesitas almacenar los datos durante mucho tiempo? Kafka y Pulsar están diseñados para esto.
*   **Tolerancia a fallos y alta disponibilidad:**  Kafka, Pulsar y RabbitMQ ofrecen alta disponibilidad.
*   **Facilidad de uso y gestión:**  Servicios gestionados (Kinesis, Event Hubs, Pub/Sub) son más fáciles de gestionar que desplegar y mantener tu propio cluster de Kafka.
*   **Ecosistema y herramientas:**  ¿Necesitas integrarte con otros servicios (ej. AWS, Azure, GCP)?  ¿Necesitas procesamiento de *streams* avanzado (Flink, Spark)?
* **Madurez y comunidad:** Kafka es muy maduro y tiene una gran comunidad.  Pulsar es más nuevo, pero está ganando mucha tracción.
* **Costos:** Considera los costos de los servicios gestionados frente a los costos de infraestructura y operación de desplegar tus propias soluciones.

En resumen, no hay una respuesta única.  Es importante evaluar cuidadosamente los requisitos de tu proyecto y elegir la combinación de tecnologías que mejor se adapte a tus necesidades.  A menudo, se utiliza Kafka junto con otras herramientas de procesamiento de *streams* como Flink o Spark.  Y, en muchos casos, Kafka se complementa muy bien con un Schema Registry.
