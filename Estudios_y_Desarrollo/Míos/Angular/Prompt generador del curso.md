```
Actúa como un Arquitecto de Software Senior y Instructor Experto especializado en ecosistemas Enterprise (Microsoft .NET y Angular). Tu objetivo es generar un **Curso de Maestría Técnica Intensivo** para prepararme para un puesto de Tech Lead en un proyecto del sector Defensa/OTAN.

**Contexto del Estudiante:**
Soy un Tech Lead con experiencia en Backend (.NET), pero necesito actualizarme profundamente en Angular Moderno (v17/18+) y su integración con arquitecturas de Microservicios en Azure.

**Objetivo del Curso:**
Crear desde cero un **Proyecto Final Integrado** llamado **"Tactical Command Dashboard"**.
Este proyecto debe ser una aplicación de "Mando y Control" que visualice activos (vehículos/unidades) en un mapa en tiempo real, gestione misiones (CRUD complejo) y tenga roles de seguridad. Este proyecto será mi portafolio para la entrevista técnica.

**Requisitos Técnicos Estrictos:**

1.  **Frontend (Angular 18+):**
    * Arquitectura **Standalone Components** (Sin NgModules).
    * Uso estricto de **Signals** para la reactividad (sustituyendo o conviviendo con RxJS donde proceda).
    * Estrategia de detección de cambios **OnPush** obligatoria.
    * Arquitectura basada en Dominios (DDD) y patrón **Smart/Dumb Components**.
    * Integración de mapas (OpenLayers o Leaflet) con renderizado eficiente.
    * Optimización (Deferrable Views, Lazy Loading, Virtual Scrolling).

2.  **Backend (.NET 8):**
    * API REST con **.NET 8** (Controllers y Minimal APIs).
    * Uso de **SignalR** para actualizaciones en tiempo real (posicionamiento de unidades).
    * Patrón **BFF (Backend For Frontend)** para servir datos optimizados a la vista.
    * **Entity Framework Core** con DTOs (Records) limpios (sin exponer entidades).
    * Implementación de Resiliencia (Polly).

3.  **Infraestructura y Seguridad (Azure Context):**
    * Simulación de despliegue en **Azure App Service** y **AKS**.
    * Seguridad con **JWT**, **Interceptors** y **Guards** (RBAC).
    * Manejo de errores global y Logging.

**Estructura del Curso (Debes desarrollar esto):**

El curso debe ser **EXTENSO, DETALLADO y PRÁCTICO**. No resumas código. Quiero ver la implementación real.

Divide el curso en los siguientes Módulos:

* **Módulo 1: Arquitectura y Configuración:** Setup de Nx Workspace, estructura de carpetas DDD, configuración de ESLint/Prettier y diseño de la BBDD del proyecto.
* **Módulo 2: El Backend (.NET Core):** Creación de la API, DTOs, Servicios, EF Core y el patrón Repository/Unit of Work si aplica.
* **Módulo 3: Fundamentos Angular Moderno:** Standalone components, nueva sintaxis de control flow (@if, @for), y creación de componentes UI reutilizables (Dumb).
* **Módulo 4: Gestión de Estado Avanzada:** Implementación de **Signals** para el estado local y un servicio global (Store ligero) para el estado de la aplicación. Conexión con la API usando HttpClient e Interceptors.
* **Módulo 5: Tiempo Real con SignalR:** Configuración del Hub en .NET y el servicio cliente en Angular. Visualización de movimientos en vivo.
* **Módulo 6: Visualización GIS (Mapas):** Integración de OpenLayers, optimización de renderizado de marcadores y manejo de eventos del mapa.
* **Módulo 7: Seguridad y Guards:** Implementación de Login, JWT Interceptor y protección de rutas por roles.
* **Módulo 8: Calidad y Testing:** Unit testing con Jest y E2E con Cypress para los flujos críticos.

**Instrucciones de Salida:**

NO generes todo el curso ahora mismo porque se cortará.
1.  Primero, preséntame el **Índice Detallado** de lo que vamos a construir.
2.  Describe brevemente el **Stack Tecnológico** y la **Arquitectura del Proyecto Final**.
3.  Espera a que yo te diga "Empieza el Módulo 1" para generar el contenido.

Cuando generes el contenido de cada módulo, quiero:
* Explicación teórica "Senior" (el *por qué* de las decisiones).
* **Bloques de código COMPLETOS** (nada de `//... rest of code`).
* Instrucciones paso a paso.

Empieza presentando el Índice y el Proyecto.
```