### 📚 Índice Detallado del Curso

Este es el roadmap que seguiremos para construir tu portafolio:

#### **Módulo 1: Arquitectura de Grado Militar y Setup**

- **Teoría:** Estrategias de Monorepo con Nx y Diseño Guiado por el Dominio (DDD) aplicado a Front/Back.
    
- **Práctica:** Inicialización del Workspace Nx, configuración estricta de **ESLint** (reglas de arquitectura), definición del modelo de datos (Entidades: `Asset`, `Mission`, `Telemetry`) y Docker Compose para SQL Server y Redis.
    

#### **Módulo 2: El Núcleo Backend (.NET 8)**

- **Teoría:** Minimal APIs vs Controllers en Enterprise, DTOs inmutables (Records) y mapeo eficiente.
    
- **Práctica:** Construcción de la API REST. Implementación de **EF Core** con Code First. Creación de un patrón **Service/Repository** genérico pero tipado. Middleware de manejo de excepciones global (RFC 7807).
    

#### **Módulo 3: Fundamentos Angular 18 y Standalone**

- **Teoría:** El adiós a `NgModule`. Mentalidad "Standalone". Nueva sintaxis de Control Flow (`@if`, `@for`) y su impacto en el rendimiento.
    
- **Práctica:** Creación de la estructura de carpetas Angular (Barrelling). Desarrollo de componentes UI reutilizables (Botones tácticos, Tarjetas de unidad) usando inputs requeridos y estilos encapsulados.
    

#### **Módulo 4: Gestión de Estado Reactiva (Signals Store)**

- **Teoría:** ¿Por qué Signals mató a los `BehaviorSubjects`? Gestión de estado local vs. Global.
    
- **Práctica:** Implementación de un **"Lightweight Store"** propio basado puramente en `signal`, `computed` y `effect`. Conexión con la API mediante `HttpClient` y transformación de datos reactiva. Manejo de condiciones de carrera.
    

#### **Módulo 5: Real-Time Intelligence (SignalR)**

- **Teoría:** WebSockets vs. Polling. Arquitectura de Hubs en .NET.
    
- **Práctica:** Configuración del Hub de Telemetría en .NET. Creación de un **Simulador de Batalla** (Background Service) que mueva unidades aleatoriamente. Consumo en Angular y actualización del Store basada en eventos push sin refrescar la pantalla.
    

#### **Módulo 6: Visualización Geoespacial (OpenLayers)**

- **Teoría:** Capas (Layers), Proyecciones y Renderizado Canvas.
    
- **Práctica:** Integración de OpenLayers. Creación de un mapa interactivo. Renderizado de miles de puntos (unidades) con alto rendimiento. Sincronización del mapa con los Signals del Store (si una unidad se mueve en el store, se mueve en el mapa).
    

#### **Módulo 7: Seguridad y Control de Acceso (RBAC)**

- **Teoría:** Autenticación Stateless (JWT) y seguridad en profundidad.
    
- **Práctica:** Endpoint de Login en .NET. Interceptores HTTP en Angular para inyectar el Token. **Functional Guards** (la forma moderna de proteger rutas) para asegurar que solo el rol `COMMANDER` pueda editar misiones, mientras `VIEWER` solo ve.
    

#### **Módulo 8: Calidad de Misión Crítica (Testing)**

- **Teoría:** La pirámide de pruebas moderna.
    
- **Práctica:** Unit Testing de Servicios y Stores con **Jest** (mocking de dependencias). Testing E2E con **Cypress** simulando un flujo completo de creación de misión y verificación visual en el mapa.