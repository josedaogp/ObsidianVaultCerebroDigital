#  Amazon Web Services (AWS) Cheatsheet: La Nube de Amazon ☁️

## 🌟 **Conceptos Fundamentales**

*   **AWS (Amazon Web Services):** Una plataforma de servicios en la nube que ofrece una amplia variedad de servicios de infraestructura (cómputo, almacenamiento, bases de datos, redes, etc.), así como servicios de nivel superior (machine learning, análisis, IoT, etc.).
*   **Regiones (Regions):** Ubicaciones geográficas donde AWS tiene centros de datos.  Elige la región más cercana a tus usuarios o donde necesites cumplir con requisitos de residencia de datos.  Cada región es completamente independiente.
*   **Zonas de Disponibilidad (Availability Zones - AZs):** Ubicaciones *dentro* de una región, diseñadas para ser aisladas de fallos en otras AZs.  Proporcionan redundancia y alta disponibilidad.  Una región tiene *múltiples* AZs.
*   **Modelo de Responsabilidad Compartida (Shared Responsibility Model):** AWS es responsable de la seguridad *de* la nube (infraestructura física, hardware, software de virtualización).  El cliente es responsable de la seguridad *en* la nube (configuración de los servicios, datos, aplicaciones, sistemas operativos, etc.).
*   **IAM (Identity and Access Management):** Servicio para gestionar usuarios, grupos, roles y permisos.  *Fundamental* para la seguridad.
*   **Pago por Uso (Pay-as-you-go):**  Pagas solo por los recursos que consumes, sin contratos a largo plazo.
*  **Consola de AWS (AWS Management Console)**: Interfaz web para gestionar los servicios.
*  **AWS CLI (Command Line Interface)**: Interfaz de línea de comandos.
*  **SDKs (Software Development Kits)**: Bibliotecas para interactuar con AWS desde diferentes lenguajes de programación.
*  **CloudFormation**: Infraestructura como código (IaC).
*  **CloudTrail**: Registro de auditoría de la actividad en tu cuenta de AWS.
*  **CloudWatch**: Monitoreo y observabilidad.

## 🔑 **IAM (Identity and Access Management)**

*   **Usuarios (Users):**  Identidades individuales con credenciales (nombre de usuario y contraseña, claves de acceso).
*   **Grupos (Groups):**  Colecciones de usuarios.  Simplifica la gestión de permisos.
*   **Roles:**  Identidades que se pueden *asumir* por usuarios, aplicaciones o servicios de AWS.  Se usan para delegar permisos temporalmente.  *Muy importante* para la seguridad.
*   **Políticas (Policies):**  Documentos JSON que definen los permisos (qué acciones se permiten o deniegan en qué recursos).
    *   **Políticas administradas por AWS (AWS managed policies):**  Políticas predefinidas por AWS.
    *   **Políticas administradas por el cliente (Customer managed policies):**  Políticas que creas tú.
    *   **Políticas inline (Inline policies):**  Políticas que se adjuntan directamente a un usuario, grupo o rol (menos recomendadas).
*   **MFA (Multi-Factor Authentication):**  Autenticación de múltiples factores (recomendado para todos los usuarios).
* **Principio de mínimo privilegio**: Otorga solo los permisos *estrictamente necesarios*.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::mi-bucket",
        "arn:aws:s3:::mi-bucket/*"
      ]
    },
    {
        "Effect": "Deny",
        "Action": "s3:*",
        "NotResource": [
            "arn:aws:s3:::mi-bucket",
            "arn:aws:s3:::mi-bucket/*"
        ]
    }
  ]
}
```

*   **`Effect`:**  `Allow` o `Deny`.
*   **`Action`:**  Lista de acciones permitidas o denegadas (ej: `s3:GetObject`, `ec2:RunInstances`, `iam:CreateUser`).  Usa `*` para todas las acciones.
*   **`Resource`:**  Lista de recursos a los que se aplica la política (se usan ARNs - Amazon Resource Names).  Usa `*` para todos los recursos.
* **`NotAction`**: Especifica acciones *excepto* las listadas.
* **`NotResource`**: Especifica recursos *excepto* los listados.
*  **`Condition`**:  Condiciones opcionales para aplicar la política (ej: según la dirección IP, la fecha, etc.).

## 💻 **Cómputo (Compute)**

*   **EC2 (Elastic Compute Cloud):**  Máquinas virtuales (instancias) en la nube.
    *   **Tipos de instancia:**  Diferentes combinaciones de CPU, memoria, almacenamiento y red.  Hay instancias optimizadas para diferentes cargas de trabajo (cómputo general, memoria intensiva, cómputo intensivo, almacenamiento intensivo, GPU, etc.).
    *   **AMI (Amazon Machine Image):**  Plantillas para lanzar instancias (incluyen el sistema operativo, software instalado, etc.).  Hay AMIs públicas (de AWS y la comunidad) y puedes crear las tuyas propias.
    *   **Grupos de seguridad (Security Groups):**  Firewalls virtuales a nivel de instancia.  Controlan el tráfico entrante y saliente.
    *   **Pares de claves (Key Pairs):**  Para acceder a las instancias a través de SSH.
    *   **Elastic IP Addresses:**  Direcciones IP públicas estáticas.
    *   **EBS (Elastic Block Storage):**  Almacenamiento en bloques (discos) persistente para las instancias EC2.  Diferentes tipos de volúmenes EBS (SSD, HDD, optimizados para rendimiento, etc.).
    *   **EC2 Instance Store:**  Almacenamiento en bloques *temporal* (no persistente) que está físicamente conectado a la instancia.  Alto rendimiento, pero los datos se pierden si la instancia se detiene o falla.
    *   **Auto Scaling:**  Escala automáticamente el número de instancias EC2 según la demanda.
    *   **Elastic Load Balancing (ELB):**  Distribuye el tráfico entre múltiples instancias EC2.
    * **Spot Instances**: Instancias EC2 a un precio *mucho* más bajo (hasta un 90% de descuento), pero que pueden ser interrumpidas por AWS con un aviso de 2 minutos.  Ideal para cargas de trabajo tolerantes a fallos y flexibles en el tiempo.
    * **Reserved Instances**:  Descuentos significativos (hasta un 75%) a cambio de un compromiso de uso de 1 o 3 años.
    * **Savings Plans**:  Modelo de precios flexible que ofrece descuentos a cambio de un compromiso de uso (medido en $/hora).
*   **Lambda:**  Servicio de cómputo *serverless* (sin servidor).  Ejecuta código en respuesta a eventos (ej: peticiones HTTP, cambios en S3, mensajes en una cola, etc.).  No necesitas administrar servidores.  Pagas solo por el tiempo de cómputo que consumes.
*   **ECS (Elastic Container Service):**  Servicio para ejecutar y administrar contenedores Docker.
*   **EKS (Elastic Kubernetes Service):**  Servicio gestionado de Kubernetes.
*   **Fargate:**  Motor de cómputo *serverless* para contenedores (ECS y EKS).  No necesitas administrar servidores ni instancias EC2.
*  **Elastic Beanstalk**:  Plataforma como servicio (PaaS) para desplegar y escalar aplicaciones web.
* **Batch**: Ejecuta trabajos por lotes (batch jobs).

## 🗄️ **Almacenamiento (Storage)**

*   **S3 (Simple Storage Service):**  Almacenamiento de *objetos* altamente escalable, duradero y disponible.  Ideal para almacenar archivos, imágenes, videos, backups, etc.
    *   **Buckets:**  Contenedores para objetos.  Los nombres de los buckets son *globalmente únicos* (en todas las cuentas de AWS).
    *   **Objetos:**  Archivos almacenados en S3.  Cada objeto tiene una clave (nombre) y datos.
    *   **Clases de almacenamiento:**
        *   **S3 Standard:**  Para datos de acceso frecuente.
        *   **S3 Intelligent-Tiering:**  Mueve automáticamente los objetos entre diferentes clases de almacenamiento según los patrones de acceso.
        *   **S3 Standard-IA (Infrequent Access):**  Para datos de acceso poco frecuente, pero que requieren acceso rápido cuando se necesitan.
        *   **S3 One Zone-IA:**  Similar a S3 Standard-IA, pero los datos se almacenan en una sola zona de disponibilidad (menor costo, menor redundancia).
        *   **S3 Glacier Instant Retrieval**: Para archivado de datos a largo plazo, con acceso en milisegundos.
        *   **S3 Glacier Flexible Retrieval**: Para archivado de datos a largo plazo, con acceso en minutos u horas.
        *  **S3 Glacier Deep Archive:**  Para archivado de datos a largo plazo, con acceso en horas (la opción más económica).
    *   **Versionado:**  Permite mantener múltiples versiones de un objeto.
    *   **Ciclo de vida (Lifecycle Policies):**  Define reglas para mover automáticamente los objetos entre diferentes clases de almacenamiento o eliminarlos después de un cierto tiempo.
    *   **Replicación (Replication):**  Replica automáticamente los objetos a otro bucket (en la misma región o en otra región).
    *   **Cifrado (Encryption):**  Cifra los datos en reposo y en tránsito.
    * **S3 Transfer Acceleration**: Acelera las transferencias de datos a S3 desde ubicaciones distantes.
*   **EBS (Elastic Block Storage):**  Ya mencionado (almacenamiento en bloques para EC2).
*   **EFS (Elastic File System):**  Sistema de archivos *compartido* y escalable para instancias EC2 (NFS).  Varias instancias EC2 pueden acceder al mismo sistema de archivos EFS simultáneamente.
*   **FSx:**  Sistemas de archivos totalmente gestionados para Windows File Server y Lustre.
*  **Storage Gateway**:  Conecta almacenamiento local (on-premises) con almacenamiento en la nube de AWS.

## 📡 **Redes (Networking)**

*   **VPC (Virtual Private Cloud):**  Red virtual aislada en la nube de AWS.  Controlas tu entorno de red (rango de IPs, subredes, tablas de rutas, gateways, etc.).
    *   **Subredes (Subnets):**  Rangos de direcciones IP dentro de una VPC.  Pueden ser públicas (con acceso a Internet) o privadas (sin acceso directo a Internet).
    *   **Tablas de rutas (Route Tables):**  Definen reglas para dirigir el tráfico de red.
    *   **Gateways de Internet (Internet Gateways):**  Permiten la comunicación entre tu VPC e Internet.
    *   **NAT Gateways:**  Permiten que las instancias en subredes privadas accedan a Internet (para actualizaciones, etc.) sin tener direcciones IP públicas.
    *   **Grupos de seguridad (Security Groups):**  Firewalls virtuales a nivel de *instancia*.
    *   **ACLs de red (Network ACLs):**  Firewalls virtuales a nivel de *subred*.
    *   **VPC Peering:**  Conexión entre dos VPCs (en la misma región o en diferentes regiones).
    *   **VPN (Virtual Private Network):**  Conexión segura entre tu red local y tu VPC.
    *   **Direct Connect:**  Conexión de red *dedicada* (física) entre tu red local y AWS.
*   **Route 53:**  Servicio DNS escalable y altamente disponible.
*   **CloudFront:**  Red de distribución de contenido (CDN).  Distribuye contenido estático y dinámico a usuarios de todo el mundo con baja latencia.
*   **ELB (Elastic Load Balancing):**  Distribuye el tráfico entre múltiples instancias EC2 (o contenedores, funciones Lambda, etc.).
    *   **Application Load Balancer (ALB):**  Balanceo de carga a nivel de aplicación (capa 7, HTTP/HTTPS).  Enrutamiento basado en el contenido de la solicitud (host, path, headers, etc.).
    *   **Network Load Balancer (NLB):**  Balanceo de carga a nivel de transporte (capa 4, TCP/UDP/TLS).  Alto rendimiento, baja latencia.
    *   **Classic Load Balancer (CLB):**  Balanceador de carga de generación anterior (no recomendado para nuevas aplicaciones).
*  **API Gateway**: Crea y gestiona APIs RESTful y WebSocket.
* **Global Accelerator**: Mejora la disponibilidad y el rendimiento de las aplicaciones dirigiendo el tráfico a través de la red global de AWS.
* **Transit Gateway**: Conecta VPCs y redes locales.

## 🗃️ **Bases de Datos (Databases)**

*   **RDS (Relational Database Service):**  Servicio de bases de datos relacionales gestionado.  Soporta varios motores de bases de datos (MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, Aurora).
    *   **Aurora:**  Base de datos relacional compatible con MySQL y PostgreSQL, diseñada para la nube.  Alto rendimiento y disponibilidad.
*   **DynamoDB:**  Base de datos NoSQL (clave-valor y documental) rápida y flexible.
*   **ElastiCache:**  Servicio de caché en memoria (Redis y Memcached).
*   **Redshift:**  Data warehouse (almacén de datos) para análisis a gran escala.
*   **Neptune:**  Base de datos de grafos.
*  **DocumentDB**: Base de datos compatible con MongoDB.
*  **Keyspaces**: Base de datos compatible con Apache Cassandra.
* **Timestream**: Base de datos de series temporales.
* **QLDB (Quantum Ledger Database)**: Base de datos de libro mayor (ledger) inmutable y verificable criptográficamente.

## 🛠️ **Herramientas para Desarrolladores (Developer Tools)**

*   **Cloud9:**  IDE basado en la nube.
*   **CodeCommit:**  Servicio de control de versiones (Git) privado.
*   **CodeBuild:**  Servicio de compilación (build).
*   **CodeDeploy:**  Servicio de despliegue (deployment).
*   **CodePipeline:**  Servicio de integración continua y entrega continua (CI/CD).
*  **X-Ray**:  Análisis y depuración de aplicaciones distribuidas.
* **CloudShell**:  Shell basada en el navegador para interactuar con AWS.

## 🤖 **Machine Learning**

*   **SageMaker:**  Plataforma completa para machine learning (construcción, entrenamiento y despliegue de modelos).
*   **Rekognition:**  Análisis de imágenes y videos.
*   **Comprehend:**  Procesamiento de lenguaje natural (NLP).
*   **Translate:**  Traducción automática.
*   **Polly:**  Texto a voz.
*   **Transcribe:**  Voz a texto.
*   **Lex:**  Creación de chatbots.
* **Forecast**: Previsión de series temporales.
*  **Personalize**:  Recomendaciones personalizadas.
*  **Fraud Detector**: Detección de fraude.

## 📊 **Análisis (Analytics)**

*   **Athena:**  Consulta datos en S3 usando SQL.
*   **EMR (Elastic MapReduce):**  Procesamiento de big data (Hadoop, Spark, etc.).
*   **Kinesis:**  Procesamiento de datos en streaming en tiempo real.
*   **Glue:**  Servicio ETL (Extract, Transform, Load).
*   **QuickSight:**  Servicio de বিজনেস intelligence (BI).
* **Data Pipeline**:  Orquestación de flujos de trabajo de datos.
* **Lake Formation**:  Construcción de data lakes.

## ✉️ **Mensajería (Messaging)**

*   **SQS (Simple Queue Service):**  Colas de mensajes.
*   **SNS (Simple Notification Service):**  Notificaciones push (pub/sub).
* **SES (Simple Email Service):**  Envío de correos electrónicos.
*  **EventBridge**: Bus de eventos *serverless*.

## 📱 **Aplicaciones Móviles (Mobile)**

*   **Amplify:**  Plataforma para construir aplicaciones web y móviles (frontend y backend).
*   **Mobile Hub:**  Consola para configurar servicios de AWS para aplicaciones móviles.
*   **AppSync:**  Servicio GraphQL gestionado.
*   **Device Farm:**  Pruebas de aplicaciones móviles en dispositivos reales.
* **Cognito**: Autenticación, autorización y gestión de usuarios.

## 🏢 **Otros Servicios Importantes**

*   **CloudFormation:**  Infraestructura como código (IaC).  Define tu infraestructura en plantillas JSON o YAML.
*   **CloudTrail:**  Registro de auditoría de la actividad en tu cuenta de AWS (quién hizo qué, cuándo y desde dónde).  *Fundamental* para la seguridad y el cumplimiento normativo.
*   **CloudWatch:**  Monitoreo y observabilidad.  Recopila métricas, logs y eventos de tus recursos de AWS y aplicaciones.
    *   **Métricas (Metrics):**  Datos numéricos sobre el rendimiento de tus recursos (ej: CPU utilization, network traffic, etc.).
    *   **Logs:**  Registros de eventos de tus aplicaciones y servicios.
    *   **Alarmas (Alarms):**  Notificaciones basadas en métricas.
    *   **Eventos (Events):**  Cambios de estado en tus recursos de AWS.
    *   **Dashboards:**  Paneles personalizados para visualizar métricas y logs.
*  **Organizations**:  Gestión centralizada de múltiples cuentas de AWS.
* **Config**:  Evaluación, auditoría y configuración de recursos de AWS.
* **Systems Manager**:  Gestión de operaciones para recursos en la nube y locales.
* **Trusted Advisor**:  Recomendaciones para optimizar costos, seguridad, rendimiento, etc.
* **Well-Architected Tool**:  Revisión de arquitecturas según las mejores prácticas de AWS.
* **Control Tower**: Configura y gestiona un entorno multi-cuenta de AWS.
* **Service Catalog**:  Crea y gestiona catálogos de servicios de TI aprobados.
* **Secrets Manager**:  Gestión de secretos (contraseñas, claves de API, etc.).
* **KMS (Key Management Service)**:  Gestión de claves criptográficas.
* **WAF (Web Application Firewall)**:  Protección contra ataques web comunes.
* **Shield**:  Protección contra ataques DDoS.
* **GuardDuty**:  Detección inteligente de amenazas.
* **Inspector**:  Evaluación de seguridad automatizada.
* **Macie**:  Descubrimiento y protección de datos sensibles.
* **Artifact**:  Acceso a informes de cumplimiento de AWS.
* **Certificate Manager**:  Gestión de certificados SSL/TLS.
* **Directory Service**:  Integración con Microsoft Active Directory.
* **CloudHSM**:  Módulos de seguridad de hardware (HSM) en la nube.
* **Cost Explorer**:  Análisis y gestión de costos.
* **Budgets**:  Presupuestos y alertas.