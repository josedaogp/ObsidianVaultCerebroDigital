#  Microsoft Azure Cheatsheet: La Nube de Microsoft ☁️

## 🌟 **Conceptos Fundamentales**

*   **Azure:** La plataforma de computación en la nube de Microsoft. Ofrece una amplia gama de servicios (cómputo, almacenamiento, bases de datos, redes, IA/ML, IoT, etc.).
*   **Suscripciones (Subscriptions):** Un agrupamiento lógico de recursos de Azure.  Los costos se acumulan a nivel de suscripción.  Puedes tener múltiples suscripciones.
*   **Grupos de recursos (Resource Groups):** Contenedores lógicos para *recursos* de Azure (máquinas virtuales, bases de datos, redes, etc.).  Facilitan la gestión, el control de acceso y la facturación.  *Todos* los recursos deben pertenecer a un grupo de recursos.
*   **Regiones (Regions):** Ubicaciones geográficas donde Azure tiene centros de datos.
*   **Zonas de disponibilidad (Availability Zones):** Ubicaciones *físicamente separadas* dentro de una región de Azure.  Proporcionan redundancia y alta disponibilidad.
*   **Azure Resource Manager (ARM):** El servicio de implementación y gestión de Azure.  Proporciona una capa de gestión coherente para crear, actualizar y eliminar recursos.  Las plantillas ARM (ARM templates) son la forma de implementar infraestructura como código (IaC) en Azure.
*   **Azure Portal:** Interfaz web para gestionar los recursos de Azure.
*   **Azure CLI:** Interfaz de línea de comandos.
*   **Azure PowerShell:** Módulo de PowerShell para Azure.
*   **Cloud Shell:** Shell interactiva en el navegador (Bash o PowerShell) con las herramientas de Azure preinstaladas.
*   **RBAC (Role-Based Access Control):** Control de acceso basado en roles (similar a IAM en AWS y GCP).
* **Azure Active Directory (Azure AD)**: Servicio de gestión de identidades y acceso basado en la nube de Microsoft.  Es *diferente* de Active Directory local (on-premises).
* **Modelo de Responsabilidad Compartida**: Similar a AWS y GCP.

## 🔑 **Azure Active Directory (Azure AD) e Identidad**

*   **Azure AD:**  Servicio de identidad y acceso en la nube.  Gestiona usuarios, grupos, aplicaciones y permisos.
    *   **Usuarios (Users):**  Identidades individuales.
    *   **Grupos (Groups):**  Colecciones de usuarios.
    *   **Aplicaciones (Applications):**  Aplicaciones que se integran con Azure AD para la autenticación y autorización.
    *   **Entidades de servicio (Service Principals):**  Identidades para aplicaciones (equivalente a las cuentas de servicio en GCP).
    *   **Identidades administradas (Managed Identities):**  Identidades gestionadas automáticamente por Azure para que los recursos de Azure se autentiquen con otros servicios de Azure (sin necesidad de gestionar credenciales).  *Muy recomendadas* para la seguridad.
        *   **Identidades administradas asignadas por el sistema (System-assigned managed identities):**  Vinculadas al ciclo de vida de un recurso de Azure.
        *   **Identidades administradas asignadas por el usuario (User-assigned managed identities):**  Recursos independientes que se pueden asignar a múltiples recursos de Azure.
*   **RBAC (Role-Based Access Control):**  Control de acceso basado en roles.
    *   **Roles integrados (Built-in roles):**  Roles predefinidos (ej: `Reader`, `Contributor`, `Owner`).
    *   **Roles personalizados (Custom roles):**  Roles que creas tú.
    *  **Definiciones de roles (Role Definitions)**:  Listas de permisos.
    *  **Asignaciones de roles (Role Assignments)**: Asocian un rol a una identidad (usuario, grupo, entidad de servicio) en un ámbito determinado (suscripción, grupo de recursos, recurso).
*   **MFA (Multi-Factor Authentication):**  Autenticación de múltiples factores.
* **Conditional Access**:  Control de acceso basado en condiciones (ej: ubicación, dispositivo, riesgo de inicio de sesión).
* **Privileged Identity Management (PIM)**:  Gestión de acceso con privilegios elevados (just-in-time access).
* **Azure AD B2C (Business-to-Consumer)**:  Gestión de identidades para clientes (consumidores).
* **Azure AD B2B (Business-to-Business)**:  Colaboración con usuarios externos.

## 💻 **Cómputo (Compute)**

*   **Virtual Machines:**  Máquinas virtuales (similar a EC2 en AWS y Compute Engine en GCP).
    *   **Conjuntos de escalado de máquinas virtuales (Virtual Machine Scale Sets):**  Escalado automático de máquinas virtuales.
    *   **Discos administrados (Managed Disks):**  Almacenamiento en bloques para las máquinas virtuales.
    *  **Imágenes (Images)**: Plantillas para crear máquinas virtuales.
    * **Grupos de seguridad de red (Network Security Groups - NSGs):**  Reglas de firewall a nivel de subred o de interfaz de red.
    * **Azure Bastion**:  Acceso seguro a máquinas virtuales a través del portal de Azure (sin necesidad de exponer puertos SSH/RDP).
*   **Azure App Service:**  Plataforma como servicio (PaaS) para crear y desplegar aplicaciones web, APIs y backends móviles.
*   **Azure Functions:**  Servicio de cómputo *serverless* (similar a AWS Lambda y Cloud Functions en GCP).
*   **Azure Kubernetes Service (AKS):**  Servicio gestionado de Kubernetes.
*   **Azure Container Instances (ACI):**  Ejecuta contenedores individuales sin necesidad de administrar servidores.
*   **Azure Container Apps:**  Servicio *serverless* para ejecutar contenedores (basado en Kubernetes y KEDA).
*   **Azure Batch:**  Ejecución de trabajos por lotes (batch jobs) a gran escala.
* **Azure Logic Apps**:  Plataforma de integración y orquestación de flujos de trabajo *serverless*.
*  **Azure Spring Apps**:  Desarrollo y despliegue de aplicaciones Spring Boot.

## 🗄️ **Almacenamiento (Storage)**

*   **Azure Blob Storage:**  Almacenamiento de *objetos* (similar a S3 en AWS y Cloud Storage en GCP).
    *   **Contenedores (Containers):**  Agrupan blobs.
    *   **Blobs:**  Archivos almacenados en Blob Storage.  Tipos:
        *   **Blobs en bloques (Block blobs):**  Para texto y datos binarios (el tipo más común).
        *   **Blobs en anexos (Append blobs):**  Optimizados para operaciones de anexado (ej: logging).
        *   **Blobs en páginas (Page blobs):**  Para acceso aleatorio (ej: discos de máquinas virtuales).
    *   **Niveles de acceso (Access tiers):**
        *   **Hot:**  Para datos de acceso frecuente.
        *   **Cool:**  Para datos de acceso poco frecuente.
        *   **Archive:**  Para datos de archivado (la opción más económica).
*   **Azure Files:**  Recursos compartidos de archivos totalmente administrados en la nube (SMB).
*   **Azure Queue Storage:**  Colas de mensajes.
*   **Azure Table Storage:**  Almacenamiento NoSQL (clave-valor).
*  **Azure Data Lake Storage Gen2**:  Lago de datos (data lake) basado en Blob Storage.
* **Azure NetApp Files**:  Almacenamiento de archivos de alto rendimiento para cargas de trabajo empresariales (NFS y SMB).
*  **Discos administrados (Managed Disks)**: Ya mencionados.

## 📡 **Redes (Networking)**

*   **Virtual Network (VNet):**  Red virtual aislada (similar a VPC en AWS y GCP).
    *   **Subredes (Subnets):**  Rangos de direcciones IP dentro de una VNet.
    *   **Grupos de seguridad de red (Network Security Groups - NSGs):**  Reglas de firewall a nivel de subred o de interfaz de red.
    *   **Tablas de rutas (Route Tables):**  Definen cómo se enruta el tráfico.
    *   **Emparejamiento de redes virtuales (VNet Peering):**  Conexión entre dos VNets.
    *   **Azure VPN Gateway:**  Conexión VPN entre tu red local y tu VNet.
    *   **ExpressRoute:**  Conexión de red dedicada (privada) entre tu red local y Azure.
*   **Azure Load Balancer:**  Balanceo de carga (varios tipos).
    *   **Azure Application Gateway:**  Controlador de entrega de aplicaciones (ADC) como servicio (capa 7, HTTP/HTTPS).  Incluye Web Application Firewall (WAF).
    * **Azure Front Door**:  CDN y balanceador de carga global (capa 7).
    * **Traffic Manager**: Enrutamiento de tráfico basado en DNS.
*   **Azure DNS:**  Servicio DNS.
* **Azure CDN**: Red de entrega de contenido.

## 🗃️ **Bases de Datos (Databases)**

*   **Azure SQL Database:**  Servicio de base de datos relacional gestionado (SQL Server).
    *   **Single Database:**  Una sola base de datos.
    *   **Elastic Pool:**  Grupo de bases de datos con recursos compartidos.
    *   **Managed Instance:**  Instancia administrada de SQL Server (casi 100% compatible con SQL Server on-premises).
*   **Azure Cosmos DB:**  Base de datos NoSQL *multimodelo* (documental, clave-valor, grafos, wide-column) y *globalmente distribuida*.
*   **Azure Database for PostgreSQL:**  Servicio gestionado de PostgreSQL.
*   **Azure Database for MySQL:**  Servicio gestionado de MySQL.
*   **Azure Database for MariaDB:**  Servicio gestionado de MariaDB.
*  **Azure Cache for Redis:**  Servicio de caché en memoria (Redis).
* **Azure Synapse Analytics**:  Servicio de análisis empresarial (anteriormente Azure SQL Data Warehouse).

## 🤖 **Inteligencia Artificial y Machine Learning (AI + Machine Learning)**

*   **Azure Machine Learning:**  Plataforma completa para machine learning (construcción, entrenamiento y despliegue de modelos).
*   **Cognitive Services:**  APIs pre-entrenadas para tareas de IA.
    *   **Vision:**  Análisis de imágenes y videos.
    *   **Speech:**  Voz a texto y texto a voz.
    *   **Language:**  Procesamiento de lenguaje natural (NLP).
    *   **Decision:**  Anomalías, moderación de contenido, personalización.
    *  **Search**: Búsqueda (Bing).
*  **Azure Bot Service**:  Creación de chatbots.
* **Azure Databricks**:  Plataforma de análisis basada en Apache Spark.
* **Azure OpenAI Service**: Acceso a los modelos de OpenAI (GPT, etc.).

## 📊 **Análisis (Analytics)**

*   **Azure Synapse Analytics:**  Ya mencionado (anteriormente Azure SQL Data Warehouse).
*   **HDInsight:**  Servicio gestionado de Hadoop, Spark, Hive, etc.
*   **Azure Stream Analytics:**  Análisis de datos en streaming en tiempo real.
*   **Azure Data Factory:**  Servicio de integración de datos (ETL).
*   **Azure Databricks:**  Ya mencionado.
*  **Power BI Embedded**:  Integración de informes y paneles de Power BI en tus aplicaciones.
* **Azure Data Explorer**:  Análisis de datos de telemetría y logs.
*  **Azure Analysis Services**:  Motor de análisis OLAP.

## ⚙️ **DevOps y Herramientas para Desarrolladores**

*   **Azure DevOps:**  Conjunto de herramientas para DevOps (control de versiones, CI/CD, gestión de proyectos, etc.).
    *   **Azure Boards:**  Gestión de proyectos (Kanban, Scrum).
    *   **Azure Repos:**  Repositorios Git privados.
    *   **Azure Pipelines:**  CI/CD.
    *   **Azure Test Plans:**  Pruebas manuales y automatizadas.
    *   **Azure Artifacts:**  Gestión de paquetes (NuGet, npm, Maven, etc.).
*   **GitHub Actions:**  CI/CD (integrado con GitHub).
*   **Azure CLI:**  Interfaz de línea de comandos.
*   **Azure PowerShell:**  Módulo de PowerShell.
*   **Cloud Shell:**  Shell interactiva en el navegador.
*  **Visual Studio Code**:  Editor de código (con extensiones para Azure).
* **Azure SDKs**: Bibliotecas para interactuar con Azure desde diferentes lenguajes de programación.

## ✉️ **Mensajería (Messaging)**

*   **Azure Service Bus:**  Colas de mensajes y temas (pub/sub).
*   **Azure Event Hubs:**  Ingesta de datos en streaming a gran escala.
*   **Azure Event Grid:**  Enrutamiento de eventos basado en un modelo pub/sub.
*  **Azure Notification Hubs**:  Notificaciones push.

## 🔒 **Seguridad (Security)**

*   **Azure Active Directory (Azure AD):**  Ya mencionado.
*   **Azure Security Center:**  Gestión centralizada de la seguridad y protección contra amenazas.
*   **Azure Key Vault:**  Gestión de secretos (claves, contraseñas, certificados).
*   **Azure Sentinel:**  SIEM (Security Information and Event Management) y SOAR (Security Orchestration, Automation, and Response).
*   **Azure Defender:** Protección contra amenazas para varios servicios de Azure.
*  **Azure Firewall**:  Firewall de red gestionado.
* **Azure DDoS Protection**:  Protección contra ataques DDoS.
*  **Azure Policy**:  Gobierno y cumplimiento normativo.
* **Microsoft Defender for Cloud**: Nombre unificado para varias herramientas de seguridad (Azure Security Center, Azure Defender, etc.).

## 📈 **Monitoreo y Administración (Monitoring and Management)**

*   **Azure Monitor:**  Monitoreo de métricas, logs y alertas.
    *   **Application Insights:**  Monitoreo de aplicaciones web (rendimiento, errores, etc.).
    *   **Log Analytics:**  Análisis de logs.
*   **Azure Advisor:**  Recomendaciones personalizadas para optimizar costos, seguridad, rendimiento, etc.
*   **Azure Resource Manager (ARM):** Ya mencionado
* **Azure Automation**:  Automatización de tareas.
* **Azure Backup**:  Copias de seguridad.
*  **Azure Site Recovery**:  Recuperación ante desastres.
* **Azure Cost Management + Billing**:  Gestión de costos y facturación.
* **Azure Policy**:  Gobierno y cumplimiento.
*  **Azure Blueprints**:  Define y despliega entornos de Azure de forma repetible y consistente.
*  **Azure Lighthouse**:  Gestión delegada de recursos entre diferentes tenants de Azure AD.

## 🌐 **Otros Servicios Importantes**

*   **Azure IoT Hub:**  Servicio para conectar, monitorear y administrar dispositivos IoT.
*   **Azure IoT Central:**  Plataforma de aplicaciones IoT.
* **Azure Sphere**: Plataforma segura para dispositivos IoT.
*  **Azure Digital Twins**:  Representación digital de entornos físicos.
* **Azure Maps**:  Servicios de mapas y geolocalización.
* **Azure Cognitive Search**: Búsqueda como servicio (anteriormente Azure Search).
* **Azure Media Services**:  Servicios para codificación, streaming y protección de contenido multimedia.
* **Azure SignalR Service**:  Comunicación en tiempo real (WebSockets).
* **Azure Web PubSub**:  Comunicación en tiempo real (WebSockets) serverless.