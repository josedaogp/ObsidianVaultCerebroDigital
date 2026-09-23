### 1. ARQUITECTURA FRONTEND & ESTRATEGIA (El esqueleto)

_Cómo organizar el código para que escale._

- **Nx Workspaces (Monorepos):** Herramienta para gestionar múltiples aplicaciones y librerías en un solo repo. Permite compartir código entre proyectos.
    
- **Domain Driven Design (DDD) en Front:** Organización de carpetas por contexto de negocio (`User`, `Mission`, `Logistics`) y no por tipo técnico.
    
- **Arquitectura Hexagonal / Clean:** Separación estricta en capas: `Domain` (Modelos), `Data` (Infraestructura/API) y `UI` (Componentes).
    
- **Smart vs Dumb Components (Container/Presenter):** Patrón de diseño donde los componentes "Smart" gestionan datos y los "Dumb" solo visualizan.
    
- **Standalone Components:** Arquitectura moderna (Angular 15+) sin `NgModules`. Reduce boilerplate y facilita el _Lazy Loading_.
    
- **Micro-Frontends (Module Federation):** (Opcional pero posible) Dividir una app gigante en sub-aplicaciones desplegables independientemente.
    

### 2. ANGULAR PROFUNDO (El núcleo)

_Herramientas diarias del equipo que debes supervisar._

- **ChangeDetectionStrategy.OnPush:** **CRÍTICO.** Configuración para que Angular solo actualice la vista cuando cambian las referencias de entrada. Clave para el rendimiento.
    
- **RxJS (Programación Reactiva):**
    
    - **Observables vs Subjects vs BehaviorSubjects:** Gestión de flujos de datos.
        
    - **High Order Maps:** `switchMap` (cancelar), `mergeMap` (paralelo), `concatMap` (cola).
        
    - **Memory Leaks:** Uso de `takeUntil`, `AsyncPipe` o `DestroyRef`.
        
- **Angular Signals (Moderno):** Nuevo sistema de reactividad granular (`WritableSignal`, `Computed`, `Effect`). El futuro de Angular.
    
- **Inyección de Dependencias (DI):**
    
    - **Providers:** `useClass`, `useValue`, `useFactory`.
        
    - **Resolution Modifiers:** `@Optional`, `@SkipSelf`, `@Host`.
        
    - **InjectionTokens:** Para inyectar configuración o primitivos.
        
- **Directivas Avanzadas:**
    
    - **Estructurales:** Manipulación del DOM (`*ngIf`, `*ngFor` o la nueva sintaxis `@if`, `@for`).
        
    - **Atributo:** Modificar comportamiento/apariencia.
        
- **Pipes:** Diferencia entre **Puros** (solo ejecutan al cambiar referencia) e **Impuros** (ejecutan en cada ciclo).
    
- **Content Projection (ng-content):** Crear componentes reutilizables ("Slots") donde el padre inyecta HTML en el hijo.
    

### 3. RENDIMIENTO & UX (Critical Mission)

_Vital para dashboards de defensa._

- **Virtual Scrolling (CDK):** Renderizar solo lo visible en listas de miles de datos.
    
- **Lazy Loading:** Carga diferida de rutas.
    
- **Deferrable Views (@defer):** Cargar bloques de plantilla solo cuando son visibles en pantalla.
    
- **Web Workers:** Mover cálculos pesados (ej: trigonometría de mapas) a un hilo secundario para no congelar la UI.
    
- **Bundle Size Optimization:** Uso de `source-map-explorer` para ver qué librerías pesan demasiado.
    

### 4. INTEGRACIÓN CON BACKEND .NET (El puente)

_Tu valor diferencial._

- **Patrón BFF (Backend For Frontend):** API intermedia en .NET que prepara los datos específicos para la vista.
    
- **SignalR (WebSockets):**
    
    - **Hubs (.NET):** Puntos de emisión de eventos.
        
    - **Client (Angular):** Suscripción a eventos y reconexión automática (Backoff).
        
- **DTOs (Data Transfer Objects):** Uso de `Records` en C# y `Interfaces` en TS. Nunca exponer entidades de EF Core directas.
    
- **Interceptors HTTP:** Middleware en Angular para Token JWT, manejo de errores globales (401, 500) y Logging.
    
- **Resiliencia (Polly):** Librería .NET para Reintentos (`Retry`) y `Circuit Breaker` en llamadas entre microservicios.
    

### 5. INFRAESTRUCTURA AZURE (El entorno)

_Dónde vive todo._

- **Azure App Service:** PaaS para alojar la web y la API. Escalado horizontal y vertical.
    
- **Azure Kubernetes Service (AKS):** Orquestación de contenedores Docker para los microservicios.
    
- **Azure SignalR Service:** Servicio gestionado para escalar WebSockets a miles de conexiones.
    
- **Azure SQL Database:** BBDD Relacional.
    
- **Application Insights:** Monitorización, trazas distribuidas y logs de errores frontend/backend unificados.
    
- **VNETs & Private Endpoints:** Redes virtuales aisladas para que la BBDD y APIs no sean accesibles desde internet público.
    

### 6. SEGURIDAD (Contexto OTAN)

_Requisitos no funcionales obligatorios._

- **OIDC / OAuth2 (PKCE):** Flujo de autenticación estándar.
    
- **JWT (JSON Web Token):** Manejo de Access Token y Refresh Token.
    
- **Guards:** Protección de rutas (`CanActivate`, `CanMatch`) basada en Roles (RBAC).
    
- **Content Security Policy (CSP):** Cabeceras HTTP para evitar XSS.
    
- **Sanitización:** Prevención de inyección de código en inputs.
    

### 7. VISUALIZACIÓN GIS (Mapas)

_Específico del dominio defensa._

- **Librerías:** OpenLayers o Leaflet.
    
- **Canvas / WebGL:** Renderizado de alto rendimiento para miles de marcadores/unidades.
    

### 8. CALIDAD & DEVOPS (El proceso)

_Liderazgo técnico._

- **Testing Strategy:**
    
    - **Unit:** Jest (Lógica de negocio/Servicios).
        
    - **E2E:** Cypress o Playwright (Flujos completos).
        
- **Azure DevOps:**
    
    - **Pipelines (YAML):** CI/CD (Build, Test, Deploy).
        
    - **Repos:** Políticas de ramas (Pull Requests obligatorias).
        
- **Code Quality:** ESLint, Prettier, Husky (Git Hooks), SonarQube (Análisis estático).
    

Esta lista contiene **todo**. Si te preguntan algo que no está aquí, es un detalle trivial. Si dominas el concepto de cada punto de esta lista, tienes el puesto.