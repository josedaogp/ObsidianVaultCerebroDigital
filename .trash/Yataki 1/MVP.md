# 📝 Especificación Detallada: MVP (Fase 1) - Plataforma de Rural Delivery

Este documento define el alcance completo y detallado del Producto Mínimo Viable (MVP) para la plataforma de Delivery Rural, enfocado exclusivamente en la optimización logística entre Restaurantes, Repartidores y el Administrador.

## 1. Introducción y Propósito del MVP

|   |   |
|---|---|
|**Concepto**|**Detalle**|
|**Objetivo Único del MVP**|Validar la eficiencia en la gestión de pedidos y la optimización de rutas en entornos rurales, antes de abrir el sistema al cliente final.|
|**Lógica Central**|Minimizar el tiempo total de pedido (`TTP`) calculando la secuencia de recogida y entrega más corta para el Repartidor.|
|**Alcance de los Actores**|**Clientes NO tienen acceso** en esta fase. Los pedidos se registran manualmente por el Restaurante o el Administrador.|
|**Tecnología Frontend**|Single Progressive Web App (PWA) construida con **React**.|
|**Tecnología Backend**|**Supabase (PostgreSQL)** para almacenamiento de datos y **APIs Externas** para geocodificación y ruteo.|

### 1.1. Flujo de Autenticación y Seguridad (Supabase Auth)

|   |   |
|---|---|
|**Concepto**|**Detalle**|
|**Inicio de Sesión Primario**|La aplicación intentará autenticarse primero usando el token personalizado inyectado (`__initial_auth_token`).|
|**Flujo Secundario (Manual)**|Si el token no está disponible o falla, se presentarán las vistas de Login/Registro/Recuperación.|
|**Asignación de Rol**|Una vez autenticado, la aplicación consultará la tabla `users` para determinar el `rol` (admin, restaurante, repartidor) y redirigir a la vista correcta.|
|**Protección**|Se utilizará **Row Level Security (RLS)** en Supabase para asegurar que un usuario solo pueda ver los datos correspondientes a su rol (ej: el Restaurante A solo ve sus pedidos).|

## 2. Arquitectura y Tecnología

### 2.1. Stack Tecnológico

- **Frontend:** React (Componentes Funcionales y Hooks).
    
- **Diseño:** Tailwind CSS (para asegurar la responsividad en móvil).
    
- **BBDD/Persistencia:** **Supabase (PostgreSQL)**. Utilizaremos las funciones de autenticación y la base de datos relacional de Supabase.
    
- **Mapeo:** Librería de mapas (Ej: Leaflet o Mapbox GL JS) y API externa para cálculos de rutas (VRP).
    

### 2.2. Modelo de Autenticación y Datos (Supabase/PostgreSQL)

La aplicación debe inicializar Supabase y autenticar al usuario al cargar, determinando su rol (`repartidor`, `restaurante`, `admin`) para mostrar la vista correcta.

|   |   |
|---|---|
|**Tabla SQL**|**Propósito y Tipo de Data**|
|`users`|Almacena perfiles de usuarios con sus roles (`id` (UUID de Auth), `rol`, `email`, `nombre`).|
|`restaurants`|Datos de los restaurantes (`id`, `nombre`, `teléfono`, `ubicación_gps` (JSON/Geography), `user_id` de propietario).|
|`deliverers`|Datos de los repartidores (`id`, `nombre`, `teléfono`, `estado_actual`, `user_id`).|
|`orders`|**Tabla Crítica.** Almacena cada pedido.|

### 2.3. Estructura de la Tabla `orders` (Columnas)

|   |   |   |
|---|---|---|
|**Campo**|**Tipo**|**Propósito**|
|`id`|UUID/Serial|ID único (automático de PostgreSQL).|
|`restaurant_id`|UUID/Integer|ID del restaurante de origen (Foreign Key).|
|`customer_name`|String|Nombre del cliente final.|
|`customer_phone`|String|Teléfono del cliente.|
|**`delivery_gps`**|JSONB/Geography|**Coordenadas de entrega (CRÍTICO: {lat, lng}). REQUERIDO para el VRP. Es el punto exacto de entrega.**|
|`status`|Integer|Estado del pedido (ver 3.1).|
|`assigned_deliverer_id`|UUID/Integer|ID del repartidor asignado (Foreign Key).|
|`total_amount`|Number|Monto total del pedido.|
|`items`|JSONB|Productos pedidos (ej: `[{name: 'Pizza', qty: 1}]`).|
|`pickup_time_estimate`|Timestamp|Hora estimada de recogida (para optimización).|

## 3. Lógica Operacional Crítica

### 3.1. Flujo de Estados de Pedido (Status)

El `status` es el campo central para la sincronización entre actores.

|   |   |   |   |
|---|---|---|---|
|**Estado (Código)**|**Nombre**|**Actor que lo Cambia**|**Efecto Clave**|
|**1**|Registrado|Restaurante / Admin|Pedido creado, visible en el tablero del Restaurante.|
|**2**|En Preparación|Restaurante|Cocina empieza a trabajar (Visualización en tablero Kanban).|
|**3 (CRÍTICO)**|**Listo para Recoger**|**Restaurante**|**DISPARA** la lógica de asignación y ruteo para el Admin.|
|**4**|Asignado|Admin / App|Pedido bloqueado al Repartidor. Visible en su Vista "Por Recoger".|
|**5**|En Ruta (Recogido)|Repartidor|Repartidor confirma que tiene la comida y va de camino al cliente.|
|**6**|Finalizado|Repartidor|Repartidor confirma la entrega. Desaparece de todas las vistas operacionales.|

### 3.2. Motor de Optimización de Rutas (VRP)

1. **Input:** La lógica VRP se dispara cuando el Admin asigna un pedido en estado **"Listo para Recoger" (3)** a un Repartidor.
    
2. **Proceso:**
    
    - Se recogen todas las ubicaciones GPS de los pedidos asignados al Repartidor (incluyendo la ubicación inicial del Repartidor).
        
    - Se llama a la **API Externa de Rutas** (Ej: OSRM/Mapbox) para obtener la matriz de distancias/tiempos entre todos los puntos.
        
    - Se ejecuta el algoritmo VRP (en backend/servidor) para determinar la secuencia óptima de N puntos.
        
3. **Output:** La aplicación del Repartidor recibe la secuencia ordenada de coordenadas y la muestra en la **Vista "Ruta Activa"**.
    

## 4. Especificación Detallada de Vistas por Rol

La aplicación es una única PWA que muestra una de las siguientes interfaces después del login, basada en el rol.

### 4.A. Vista Repartidor (Role: `repartidor`)

**Foco:** Eficiencia, Mapa, Botones Grandes.

|   |   |
|---|---|
|**Vista**|**Componentes y Acciones**|
|**Vista Principal**|**"Mi Ruta"** (Pestaña por defecto).|
|**Header**|Muestra Nombre del Repartidor, ID de Usuario (CRÍTICO: `userId` completo para identificación), y botón de Cerrar Sesión.|
|**Vista "Por Recoger"**|Lista simple de pedidos asignados (Estado 4) pendientes de confirmación de recogida.|
|**Vista "Ruta Activa"**|**Mapa Centrado:** Muestra la ubicación actual del repartidor y los marcadores de las paradas ordenadas por el VRP. **Lista Secuencial:** Muestra el orden óptimo de paradas (ej: 1. Recoger en Pizzería X, 2. Entregar a Cliente Y). **Botón: "Siguiente Parada"**|
|**Acción Clave: Entregar**|Al llegar a un punto de entrega, el Repartidor presiona|$$BOTÓN GRANDE: CONFIRMAR ENTREGA$$|. Esto cambia el estado del `Pedido` a **6 (Finalizado)** en Supabase.|

### 4.B. Vista Restaurante (Role: `restaurante`)

**Foco:** Flujo Operacional y Gestión de Pedidos Manuales.

|   |   |
|---|---|
|**Vista**|**Componentes y Acciones**|
|**Vista Principal**|Tablero Kanban de Pedidos (Estados 1, 2, 3).|
|**Header**|Muestra Nombre del Restaurante, Botón para|$$REGISTRAR NUEVO PEDIDO$$|.|
|**Modal: Nuevo Pedido**|Formulario para introducir: Nombre Cliente, Teléfono, Productos y Monto. **Campo CRÍTICO:** Se debe introducir la **DIRECCIÓN COMPLETA** para el repartidor. Adicionalmente, se usa un **Mapa** con un _pin_ movible para registrar las **Coordenadas GPS (Lat/Lng)** exactas. **Estas coordenadas son el input clave para la optimización de rutas.**|
|**Tablero Kanban**|Tres columnas: **Registrado (1)**, **En Preparación (2)**, **Listo para Recoger (3)**. Cada tarjeta de pedido debe tener un botón|$$MOVER A SIGUIENTE ESTADO$$|.|
|**Acción Clave: Listo**|Botón|$$MARCAR COMO LISTO PARA RECOGER$$|(Cambia el estado a **3**).|

### 4.C. Vista Administrador (Role: `admin`)

**Foco:** Despacho, Monitoreo y Mantenimiento de Entidades.

|   |   |
|---|---|
|**Vista**|**Componentes y Acciones**|
|**Vista Principal**|Pestaña **"Despacho"** (Por defecto).|
|**Pestaña Despacho**|**Panel 1: Pedidos Listos (Estado 3).** Listado de todos los pedidos listos, pendientes de asignación. **Panel 2: Repartidores Disponibles.** Lista de Repartidores con su estado actual (libre/en ruta). Botón:|$$ASIGNAR Y OPTIMIZAR RUTA$$|.|
|**Pestaña Entidades (CRUD)**|Vistas de mantenimiento para: **Restaurantes**, **Repartidores**, y **Cartas** (menú simple). El Admin introduce los datos de nuevos restaurantes y repartidores.|
|**Acción Clave: Asignación**|El Admin selecciona uno o varios pedidos del Panel 1 y un Repartidor del Panel 2, y hace clic en|$$ASIGNAR Y OPTIMIZAR RUTA$$|. Esto dispara: **1.** Cambio de estado a **4 (Asignado)**. **2.** Envío de la solicitud de VRP a la API.|

### 4.D. Vistas de Autenticación (Pre-Login)

Estas vistas son el punto de entrada a la aplicación.

|   |   |
|---|---|
|**Vista**|**Componentes y Acciones**|
|**Pantalla de Carga/Token**|Muestra un spinner o mensaje de "Cargando..." mientras se verifica el `__initial_auth_token` o se inicializa Supabase.|
|**Pantalla de Login**|Formulario para introducir Email y Contraseña. Botón:|$$ACCEDER$$|. Enlace:|$$¿Olvidaste tu Contraseña?$$|y|$$Crear Cuenta$$|.|
|**Pantalla de Registro**|Formulario para introducir Email, Contraseña y Nombre. Nota: El rol (`admin`, `restaurante`, `repartidor`) DEBE ser asignado por el Administrador en la Pestaña de Entidades, el registro solo crea el usuario Auth de Supabase. Botón:|$$REGISTRAR$$|. Enlace:|$$Volver a Login$$|.|
|**Pantalla de Recuperación de Contraseña**|Campo para introducir Email. Botón:|$$ENVIAR INSTRUCCIONES$$|(Utiliza el flujo de correo electrónico de Supabase).|

## 5. Requerimientos de Tecnología y Desarrollo

1. **Single PWA:** La aplicación debe ser 100% responsive y estar contenida en un único archivo React (`.jsx` o `.tsx`).
    
2. **Gestión de Rutas:** Se requiere un hook o servicio de React dedicado a gestionar las llamadas a la API de Geocodificación/Rutas (debe manejar la lógica de _retry_ con _exponential backoff_).
    
3. **Seguridad (Mínima):** Las peticiones a Supabase deben ser controladas por **Row Level Security (RLS)** de PostgreSQL. Por ejemplo, un Repartidor solo puede leer los pedidos donde su `id` sea igual a `assigned_deliverer_id`.
    
4. **Experiencia de Usuario:** Priorizar la claridad y los botones grandes y táctiles para las acciones críticas del Repartidor (marcar recogida/entrega).