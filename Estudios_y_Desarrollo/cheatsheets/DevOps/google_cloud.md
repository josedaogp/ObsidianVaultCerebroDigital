#  Google Cloud Platform (GCP) Cheatsheet: La Nube de Google ☁️

## 🌟 **Conceptos Fundamentales**

*   **Google Cloud Platform (GCP):** Una suite de servicios de computación en la nube que se ejecuta en la misma infraestructura que Google usa internamente para sus productos de usuario final, como la Búsqueda de Google, Gmail, etc.
*   **Proyectos (Projects):**  El contenedor base para todos los recursos de GCP.  Los recursos se crean y gestionan *dentro* de un proyecto.  Un proyecto tiene un ID, un nombre y un número de proyecto.
*   **Regiones (Regions):** Ubicaciones geográficas donde GCP tiene centros de datos.  Elige la región más cercana a tus usuarios.
*   **Zonas (Zones):** Ubicaciones *dentro* de una región.  Despliega tus recursos en múltiples zonas para alta disponibilidad.
*   **Facturación (Billing):**  Los costos se asocian a una cuenta de facturación, que se vincula a uno o más proyectos.
*   **IAM (Identity and Access Management):**  Control de acceso (quién puede hacer qué).  Similar al IAM de AWS.
*   **Console (Google Cloud Console):**  Interfaz web para gestionar los recursos de GCP.
*   **Cloud SDK:**  Herramientas de línea de comandos (`gcloud`, `gsutil`, `bq`).
*   **APIs:**  Interfaces de programación de aplicaciones para interactuar con los servicios de GCP.
*   **Cuotas (Quotas):**  Límites en el uso de recursos (para proteger contra el consumo excesivo accidental).
* **Modelo de responsabilidad compartida**: Similar a AWS.

## 🔑 **IAM (Identity and Access Management)**

*   **Miembros (Members):**  Identidades (usuarios, grupos, cuentas de servicio).  Pueden ser:
    *   **Cuentas de Google (Google Accounts):**  Usuarios individuales.
    *   **Cuentas de servicio (Service Accounts):**  Identidades para aplicaciones (no para usuarios humanos).  *Muy importantes* para la seguridad.
    *   **Grupos de Google (Google Groups):**  Colecciones de cuentas de Google.
    * **Dominios de Google Workspace**: Usuarios de un dominio de Google Workspace.
    *  **Identidades federadas (Federated Identities):**  Usuarios autenticados por un proveedor de identidad externo (ej: Active Directory).
    * **allUsers, allAuthenticatedUsers:** Tipos especiales para acceso público.
*   **Roles:**  Colecciones de *permisos*.  Un permiso permite a un miembro realizar una acción específica en un recurso.
    *   **Roles predefinidos (Predefined roles):**  Roles creados y mantenidos por Google (ej: `roles/viewer`, `roles/editor`, `roles/owner`, `roles/compute.instanceAdmin`, `roles/storage.objectViewer`).
    *   **Roles personalizados (Custom roles):**  Roles que creas tú.
*   **Políticas (Policies):**  Conjunto de *bindings* (enlaces) que asocian miembros a roles.  Las políticas se definen a nivel de recurso (proyecto, carpeta, organización o recurso individual).
* **Principio de mínimo privilegio**: Otorga *solo* los permisos necesarios.

```json
{
  "bindings": [
    {
      "role": "roles/storage.objectViewer",
      "members": [
        "user:ana@example.com",
        "group:admins@example.com",
        "serviceAccount:mi-app@proyecto-id.iam.gserviceaccount.com"
      ]
    },
    {
      "role": "roles/editor",
      "members": [
        "user:juan@example.com"
      ],
      "condition": {
          "title": "Solo_fin_de_semana",
          "expression": "request.time.getDayOfWeek('America/Los_Angeles') >= 6"
      }
    }
  ]
}
```

* **Organizaciones (Organizations)**: Permiten gestionar de forma centralizada varios proyectos
*  **Carpetas (Folders)**: Agrupan proyectos dentro de una organización.

## 💻 **Cómputo (Compute)**

*   **Compute Engine:**  Máquinas virtuales (instancias).  Similar a EC2 en AWS.
    *   **Tipos de máquina (Machine Types):**  Diferentes configuraciones de CPU y memoria.  Hay tipos predefinidos (de propósito general, optimizados para cómputo, optimizados para memoria, etc.) y puedes crear tipos de máquina personalizados.
    *   **Imágenes (Images):**  Imágenes de disco para las instancias (sistemas operativos, software).  Hay imágenes públicas (de Google y la comunidad) y puedes crear las tus propias.
    *   **Discos persistentes (Persistent Disks):**  Almacenamiento en bloques duradero para las instancias (SSD o HDD).
    *   **Instantáneas (Snapshots):**  Copias de seguridad de los discos persistentes.
    *   **Grupos de instancias (Instance Groups):**  Gestionan conjuntos de instancias idénticas.
        *   **Grupos de instancias administrados (Managed Instance Groups - MIGs):**  Permiten el escalado automático, la alta disponibilidad y la actualización de instancias.
        *   **Grupos de instancias no administrados (Unmanaged Instance Groups):**  Controlas manualmente cada instancia.
    *   **Grupos de seguridad (Firewall Rules):**  Reglas de firewall a nivel de VPC (no de instancia, como en AWS).
    * **GPUs**: Puedes añadir GPUs a las instancias.
    *  **TPUs (Tensor Processing Units)**: Aceleradores de hardware de Google para machine learning.
*   **App Engine:**  Plataforma como servicio (PaaS) para desplegar aplicaciones web.  Escala automáticamente según la demanda.  Dos entornos:
    *   **Standard Environment:**  Entorno de ejecución preconfigurado (con limitaciones).  Más fácil de usar.
    *   **Flexible Environment:**  Usa contenedores Docker.  Más flexible.
*   **Cloud Functions:**  Servicio de cómputo *serverless* (similar a AWS Lambda).  Ejecuta funciones en respuesta a eventos.
*   **Cloud Run:**  Ejecuta contenedores *serverless*.  Se basa en Knative.
*   **Kubernetes Engine (GKE):**  Servicio gestionado de Kubernetes.
*  **Bare Metal Solution**: Servidores físicos dedicados.

## 🗄️ **Almacenamiento (Storage)**

*   **Cloud Storage:**  Almacenamiento de *objetos* (similar a S3 en AWS).
    *   **Buckets:**  Contenedores para objetos.  Los nombres de los buckets son *globalmente únicos*.
    *   **Objetos:**  Archivos almacenados en Cloud Storage.
    *   **Clases de almacenamiento:**
        *   **Standard:**  Para datos de acceso frecuente.
        *   **Nearline:**  Para datos de acceso poco frecuente (al menos una vez al mes).
        *   **Coldline:**  Para datos de acceso muy poco frecuente (al menos una vez al trimestre).
        *   **Archive:**  Para archivado de datos a largo plazo (al menos una vez al año).  La opción más económica.
    *   **Ciclo de vida (Lifecycle Management):**  Reglas para mover automáticamente los objetos entre clases de almacenamiento o eliminarlos.
*   **Persistent Disk:**  Ya mencionado (almacenamiento en bloques para Compute Engine).
*   **Filestore:**  Almacenamiento de archivos compartido (NFS) para instancias de Compute Engine.
* **Cloud Storage Nearline y Coldline**: Para backups y archivado.

## 📡 **Redes (Networking)**

*   **Virtual Private Cloud (VPC):**  Red virtual aislada.  Similar a VPC en AWS.
    *   **Subredes (Subnetworks):**  Rangos de direcciones IP dentro de una VPC.
    *   **Reglas de firewall (Firewall Rules):**  Controlan el tráfico entrante y saliente *a nivel de VPC*.
    *   **Rutas (Routes):**  Definen cómo se enruta el tráfico.
    *   **Cloud VPN:**  Conexión VPN entre tu red local y tu VPC.
    *   **Cloud Interconnect:**  Conexión de red dedicada (física) entre tu red local y Google Cloud.
        * **Dedicated Interconnect**: Conexión directa.
        * **Partner Interconnect**: A través de un proveedor de servicios.
    *   **VPC Network Peering:**  Conexión entre dos VPCs.
*   **Cloud Load Balancing:**  Balanceo de carga (varios tipos).
    *   **HTTP(S) Load Balancing:**  Balanceo de carga a nivel de aplicación (capa 7).
    *   **TCP Load Balancing:**  Balanceo de carga a nivel de transporte (capa 4).
    *   **UDP Load Balancing:** Balanceo de carga a nivel de transporte (capa 4).
    *   **Internal Load Balancing:**  Balanceo de carga para tráfico interno (dentro de tu VPC).
*   **Cloud DNS:**  Servicio DNS.
*   **Cloud CDN:**  Red de distribución de contenido (CDN).
* **Network Service Tiers**: Niveles de rendimiento de red (Premium y Standard).
* **Traffic Director**:  Plano de control de tráfico para mallas de servicios (service meshes).

## 🗃️ **Bases de Datos (Databases)**

*   **Cloud SQL:**  Servicio de bases de datos relacionales gestionado (MySQL, PostgreSQL, SQL Server).
*   **Cloud Spanner:**  Base de datos relacional *globalmente distribuida* y *fuertemente consistente*.
*   **Cloud Datastore:**  Base de datos NoSQL (documental) altamente escalable. Ahora es **Firestore en modo Datastore**.
*   **Firestore:**  Base de datos NoSQL (documental) para aplicaciones web y móviles.
*   **Bigtable:**  Base de datos NoSQL (wide-column) de alto rendimiento para grandes cargas de trabajo analíticas y operacionales.
*   **Memorystore:**  Servicio de caché en memoria (Redis y Memcached).
*  **AlloyDB**: Base de datos compatible con PostgreSQL.

## 📊 **Big Data y Análisis (Analytics)**

*   **BigQuery:**  Data warehouse (almacén de datos) *serverless* y altamente escalable para análisis.  Permite ejecutar consultas SQL sobre grandes conjuntos de datos.
*   **Dataflow:**  Servicio para procesamiento de datos en *streaming* y por lotes (batch).  Se basa en Apache Beam.
*   **Dataproc:**  Servicio gestionado de Hadoop y Spark.
*   **Pub/Sub:**  Servicio de mensajería y *streaming* de eventos en tiempo real.
*   **Data Fusion:**  Servicio ETL (Extract, Transform, Load) basado en una interfaz gráfica.
*   **Data Catalog:**  Servicio de gestión de metadatos.
*   **Composer:**  Servicio de orquestación de flujos de trabajo (basado en Apache Airflow).
*  **Looker**: Plataforma de business intelligence (BI).
*  **Dataprep**:  Preparación de datos visual.

## 🤖 **Machine Learning e Inteligencia Artificial (AI)**

*   **Vertex AI:**  Plataforma unificada para machine learning (construcción, entrenamiento y despliegue de modelos).  Sustituye a AI Platform.
*   **Cloud AutoML:**  Permite entrenar modelos de ML personalizados *sin necesidad de escribir código* (o con poco código).
    *   **AutoML Vision:**  Para imágenes.
    *   **AutoML Video Intelligence**: Para videos.
    *   **AutoML Natural Language:**  Para texto.
    *   **AutoML Translation:**  Para traducción.
    *   **AutoML Tables:**  Para datos tabulares.
*   **Pre-trained APIs:**  APIs pre-entrenadas para tareas comunes de ML.
    *   **Vision API:**  Análisis de imágenes.
    *   **Video Intelligence API**: Análisis de videos.
    *   **Natural Language API:**  Procesamiento de lenguaje natural (NLP).
    *   **Translation API:**  Traducción automática.
    *   **Speech-to-Text:**  Voz a texto.
    *   **Text-to-Speech:**  Texto a voz.
    *  **Document AI**:  Procesamiento de documentos.
    * **Recommendations AI**: Recomendaciones.
    * **Dialogflow**:  Creación de chatbots.

## 🛠️ **Herramientas para Desarrolladores (Developer Tools)**

*   **Cloud SDK:**  Herramientas de línea de comandos (`gcloud`, `gsutil`, `bq`).
    *   **`gcloud`:**  Herramienta principal para interactuar con GCP desde la línea de comandos.
    *   **`gsutil`:**  Herramienta para interactuar con Cloud Storage.
    *   **`bq`:**  Herramienta para interactuar con BigQuery.
*   **Cloud Shell:**  Shell interactiva en el navegador (con el Cloud SDK preinstalado).
*   **Cloud Code:**  Extensiones para IDEs (VS Code, IntelliJ) para facilitar el desarrollo en GCP.
*   **Cloud Build:**  Servicio de integración continua y entrega continua (CI/CD).
*   **Cloud Source Repositories:**  Repositorios Git privados.
*   **Artifact Registry:**  Gestión de artefactos (imágenes de contenedores, paquetes, etc.).
*   **Container Registry:** Registro privado de imágenes de contenedores Docker (obsoleto, usar Artifact Registry)
*  **Cloud Deployment Manager:** Infraestructura como código (IaC).  Alternativa a Terraform.
* **Cloud Scheduler**:  Programador de tareas (cron jobs).

## 🔒 **Seguridad (Security)**

*   **Cloud IAM:**  Ya mencionado (control de acceso).
*   **Cloud Security Command Center:**  Gestión centralizada de la seguridad y el riesgo de datos.
*   **Cloud Key Management Service (KMS):**  Gestión de claves criptográficas.
*   **Secret Manager:**  Gestión de secretos (contraseñas, claves de API, etc.).
*   **Cloud Armor:**  Protección contra ataques DDoS y WAF (Web Application Firewall).
*   **VPC Service Controls:**  Perímetro de seguridad para los servicios de GCP.
*  **Security Scanner**:  Escaneo de vulnerabilidades para App Engine, GKE y Compute Engine.
* **Binary Authorization**: Asegura que solo se ejecuten imágenes de contenedores confiables.
*  **Event Threat Detection**:  Detección de amenazas.
* **Assured Workloads**:  Control de cumplimiento normativo.
* **Confidential Computing**:  Mantiene los datos cifrados *incluso durante el procesamiento*.

## 📈 **Monitoreo y Logging (Monitoring and Logging)**

*   **Cloud Monitoring (antes Stackdriver Monitoring):**  Monitoreo de métricas, dashboards y alertas.
*   **Cloud Logging (antes Stackdriver Logging):**  Gestión de logs.
*   **Cloud Trace (antes Stackdriver Trace):**  Análisis de rendimiento y latencia de aplicaciones distribuidas.
*   **Cloud Debugger (antes Stackdriver Debugger):**  Depuración de aplicaciones en producción.
*   **Cloud Profiler (antes Stackdriver Profiler):**  Perfilado de rendimiento (CPU y memoria).
* **Error Reporting**:  Agrega y muestra errores producidos en las aplicaciones.

## ✉️ **Mensajería (Messaging)**
* **Pub/Sub**: Ya mencionado

## ➕ **Otros Servicios Importantes**

*   **Apigee:**  Plataforma para la gestión de APIs.
*  **Anthos**:  Plataforma para modernizar aplicaciones existentes y construir nuevas aplicaciones nativas de la nube (híbrida y multi-cloud).
* **Game Servers**:  Infraestructura para juegos.