# 🏨 Módulo 1: Restaurante (Gestión de Cocina y Pedidos)

Este módulo es el centro de control del hostelero. Es donde la magia de la cocina se encuentra con la logística.

## [R-00] Gestión de Carta y Tiempos (¡NUEVO!)

> Como Dueño de Restaurante,
> 
> Quiero poder añadir los platos de mi carta y su tiempo medio de preparación,
> 
> Para que el Motor Logístico [MOTOR-01] pueda calcular el ETA y optimizar las recogidas.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-R00.1] Acceso a la Gestión de Carta**
    
    - **Dado** que el usuario (Restaurante) está en su módulo (ej. en una pestaña de "Ajustes" o "Mi Restaurante").
        
    - **Entonces** debe haber una sección/botón claro de "Mi Carta" o "Gestión de Platos".
        
- **[AC-R00.2] Añadir/Editar Plato**
    
    - **Dado** que el usuario está en "Mi Carta".
        
    - **Cuando** pulsa "Añadir Plato" (o "Editar" uno existente).
        
    - **Entonces** ve un formulario con campos obligatorios: `Nombre del Plato` (Texto), `Tiempo Medio de Preparación (min)` (Entero, ej. 15).
        
    - **Y** campos opcionales: `Descripción` (Texto), `Precio` (Decimal).
        
- **[AC-R00.3] Impacto en el Motor (Crítico)**
    
    - **Dado** que un plato (ej. "Pizza Barbacoa") tiene un tiempo de `15` min.
        
    - **Cuando** el restaurante acepta un pedido [R-02.6] que contiene ese plato (y otros).
        
    - **Entonces** el Motor [MOTOR-01] debe usar el tiempo del plato _más largo_ del pedido como base para calcular el `shipments.estimated_pickup_time`.
        
- **[AC-R00.4] Disponibilidad del Plato (Switch)**
    
    - **Dado** que el usuario ve su lista de platos en "Mi Carta".
        
    - **Entonces** debe haber un _switch_ (On/Off) simple al lado de cada plato para marcarlo como "Agotado" / "Disponible".
        
    - **(Importante):** (Alcance V1.1) Si un plato está "Agotado", no debería poder seleccionarse al crear un pedido. (Para V1, esta gestión de carta es solo para _tiempos_).
        

#### ➡️ Happy Path (Flujo Ideal)

1. Luigi va a "Ajustes" -> "Mi Carta".
    
2. Pulsa "Añadir Plato".
    
3. Escribe: Nombre: "Pizza Carbonara", Tiempo: "12".
    
4. Escribe: Nombre: "Lasaña de Carne", Tiempo: "20".
    
5. Pulsa "Guardar". Ahora el Motor [MOTOR-01] usará estos datos.
    

#### 🎨 Notas de Diseño y Técnicas (Para el Desarrollador)

- **BBDD:** Almacenado en la tabla `menu_items`.
    
- **Lógica:** Esta US es fundamental. Sin ella, el [MOTOR-01] no tiene datos para operar.
    

## [R-01] Login del Restaurante

> _(Esta US ha sido absorbida por [CORE-01] Login por Roles. El flujo es idéntico al [D-01.1]. El usuario con rol `restaurant` es redirigido al [R-02] Dashboard)._

## [R-02] Dashboard de Envíos (Kanban)

> Como Dueño de Restaurante,
> 
> Quiero ver todos mis envíos del día en un dashboard visual tipo Kanban con los nuevos estados de cocina,
> 
> Para gestionar mi operación desde la recepción del pedido hasta que el repartidor se lo lleva.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-R02.1] Pantalla de Inicio**
    
    - **Dado** que el usuario (Restaurante) inicia sesión.
        
    - **Entonces** esta pantalla (Dashboard Kanban) es su pantalla de inicio.
        
- **[AC-R02.2] Estructura de Columnas (Nuevos Estados)**
    
    - **Dado** que el usuario está en el Dashboard.
        
    - **Entonces** ve 5 columnas/pestañas (Kanban) que puede deslizar horizontalmente:
        
        1. **"Registrado"** (`REGISTERED`): Pedidos nuevos que entran.
            
        2. **"En Preparación"** (`IN_PROGRESS`): Pedidos aceptados y cocinándose.
            
        3. **"Listo para Recoger"** (`READY_FOR_PICKUP`): Pedidos terminados, esperando repartidor.
            
        4. **"En Ruta"** (`IN_TRANSIT`): Repartidor ya recogió.
            
        5. **"Entregados"** (`DELIVERED`): Ciclo completado.
            
- **[AC-R02.3] Filtro de Datos (MVP)**
    
    - **Dado** que el Dashboard se carga.
        
    - **Entonces** por defecto, solo debe mostrar los envíos creados **"Hoy"** (desde las 00:00 del día actual).
        
- **[AC-R02.4] Tarjeta de Envío (Card)**
    
    - **Dado** que hay un envío en una columna.
        
    - **Cuando** el usuario ve la "tarjeta" de ese envío.
        
    - **Entonces** debe mostrar la información mínima y vital:
        
        - `Nombre del Cliente` (o Dirección si no hay nombre).
            
        - `Hora de Creación` (ej. "13:45h").
            
        - **Info de Cobro (¡Crítico!)**: Un icono y texto claro (ej. 💸 "Efectivo: 25,50€" o 💳 "TPV: 30,00€" o ✅ "Ya Pagado").
            
- **[AC-R02.5] Botón de "Nuevo Envío"**
    
    - **Dado** que el usuario está en el Dashboard.
        
    - **Entonces** debe ver un botón flotante (Floating Action Button - FAB) de "Añadir" (icono `+`) claramente visible.
        
    - **Y** al pulsar este botón, debe ser llevado a la pantalla/formulario [R-03] "Solicitar Envío".
        
- **[AC-R02.6] Acción en "Registrado" (Aceptar Pedido)**
    
    - **Dado** que un pedido (tarjeta) está en la columna "Registrado".
        
    - **Cuando** el usuario pulsa la tarjeta (o un botón "Aceptar" en ella).
        
    - **Entonces** ve un botón "Empezar Preparación".
        
    - **Y** al pulsarlo, el `shipment.status` cambia a `IN_PROGRESS` (y se actualiza `time_preparation_started`).
        
    - **Y** la tarjeta se mueve a la columna "En Preparación".
        
    - **Y** (Crítico) esta acción **activa el Motor Logístico [MOTOR-01]** para que empiece a buscar un repartidor.
        
- **[AC-R02.7] Acción en "En Preparación" (Añadir Tiempo)**
    
    - **Dado** que un pedido está en "En Preparación".
        
    - **Cuando** el usuario ve la tarjeta o sus detalles [R-06].
        
    - **Entonces** debe ver botones claros: **"+5 min"**, **"+10 min"**.
        
    - **Y** al pulsarlos, se llama a la API para actualizar el `shipments.estimated_pickup_time`.
        
    - **Y** el Motor [MOTOR-01] recibe esta actualización y recalcula la asignación si es necesario.
        
- **[AC-R02.8] Acción en "En Preparación" (Marcar como Listo)**
    
    - **Dado** que un pedido está en "En Preparación".
        
    - **Cuando** la cocina termina el plato.
        
    - **Entonces** el usuario pulsa un botón "Listo para Recoger".
        
    - **Y** el `shipment.status` cambia a `READY_FOR_PICKUP` (y se actualiza `time_ready_for_pickup`).
        
    - **Y** la tarjeta se mueve a la columna "Listo para Recoger".
        
    - **Y** esto envía una señal al repartidor asignado [D-02] ("¡Tu pedido está listo, ven ya!").
        
- **[AC-R02.9] Movimiento Automático de Tarjetas**
    
    - **Dado** que una tarjeta está en "Listo para Recoger" o "En Ruta".
        
    - **Cuando** el repartidor [D-05] pulsa "He Recogido" o "He Entregado".
        
    - **Entonces** la tarjeta debe moverse _automáticamente_ a la columna "En Ruta" o "Entregados" (la app debe refrescarse vía Polling V1 o WebSockets V1.1).
        

#### 🎨 Notas de Diseño y Técnicas (Para el Desarrollador)

- **UI:** Un `TabView` o `HorizontalScrollView` con `ListViews` por columna.
    
- **API:** `GET /restaurants/shipments?date=today`. La app _frontend_ debe organizar los pedidos en las columnas según el `shipment.status`.
    
- **Lógica:** Esta pantalla no solo lee datos, sino que _escribe_ estados (`IN_PROGRESS`, `READY_FOR_PICKUP`) y tiempos (`estimated_pickup_time`), lo cual es vital para el Motor.
    

## [R-03] + [R-04] + [R-05] (Combinadas): Formulario de "Solicitar Envío"

> Como Dueño de Restaurante,
> 
> Quiero pulsar el botón "Nuevo Envío" [R-02.5] y rellenar un formulario simple,
> 
> Para especificar los detalles del cliente (Dirección, Teléfono, Notas) y el método de cobro (Pagado, Efectivo, TPV) y así crear un envío.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-R03.1] Acceso al Formulario**
    
    - **Dado** que el usuario está en el Dashboard Kanban [R-02].
        
    - **Cuando** pulsa el botón flotante "Añadir" (`+`).
        
    - **Entonces** la app debe navegar a la pantalla "Solicitar Envío" (un formulario vacío).
        
- **[AC-R03.2] Campos de Cliente (Obligatorios)**
    
    - **Dado** que el usuario está en el formulario.
        
    - **Cuando** intenta "Solicitar Envío" pero `Dirección del Cliente` O `Teléfono del Cliente` están vacíos.
        
    - **Entonces** el campo vacío debe mostrar un error (ej. "Campo obligatorio") y el envío NO debe crearse.
        
- **[AC-R03.3] Autocompletar Dirección (Crítico)**
    
    - **Dado** que el usuario empieza a escribir en `Dirección del Cliente`.
        
    - **Cuando** escribe (ej. "Calle Mayor 1, pueblo...").
        
    - **Entonces** la app DEBE mostrar sugerencias de **Google Places/Maps API** para que el usuario seleccione una dirección validada.
        
    - **Y** al seleccionar una sugerencia, la app debe guardar _internamente_ las coordenadas (`client_location`) para el Motor [MOTOR-01] y el GPS del repartidor [D-04].
        
- **[AC-R03.4] Campos Adicionales (Opcionales)**
    
    - **Dado** que el usuario está en el formulario.
        
    - **Entonces** debe ver campos opcionales para `Nombre del Cliente` y `Notas del Pedido` (ej. "Sin cebolla", "Llamar al timbre 2ºB", "Pedido: 2x Pizza, 1x CocaCola").
        
- **[AC-R03.5] Selección de Pago (Requisito Clave)**
    
    - **Dado** que el usuario está en el formulario.
        
    - **Entonces** debe ver una sección clara "Método de Cobro" con 3 opciones seleccionables (ej. `RadioButtons`):
        
        1. **"Ya Pagado"** (`PREPAID`)
            
        2. **"Cobrar en Efectivo"** (`CASH`)
            
        3. **"Cobrar con Tarjeta (TPV)"** (`CARD`)
            
- **[AC-R03.6] Lógica de Importe Condicional**
    
    - **Dado** que el usuario está en el formulario.
        
    - **Cuando** selecciona "Ya Pagado".
        
    - **Entonces** el campo `Importe a Cobrar` debe desaparecer o deshabilitarse (y su valor ser 0).
        
    - **Cuando** selecciona "Cobrar en Efectivo" O "Cobrar con Tarjeta (TPV)".
        
    - **Entonces** un campo `Importe a Cobrar (€)` debe aparecer y ser **obligatorio**.
        
    - **Y** si pulsa "Solicitar" con este campo vacío (o en 0), debe mostrar un error "Debes indicar un importe".
        
- **[AC-R03.7] Creación Exitosa del Envío**
    
    - **Dado** que el usuario ha rellenado todos los campos obligatorios.
        
    - **Cuando** pulsa el botón principal "Solicitar Envío".
        
    - **Entonces** la app debe mostrar un indicador de carga (spinner).
        
    - **Y** se debe realizar una llamada a la API (`POST /shipments`) para crear el envío con estado `REGISTERED`.
        
    - **Y** al recibir confirmación (éxito), la app debe navegar _automáticamente_ de vuelta al Dashboard Kanban [R-02], donde el nuevo pedido debe aparecer en la columna "Registrado".
        
- **[AC-R03.8] Cancelación**
    
    - **Dado** que el usuario está rellenando el formulario.
        
    - **Cuando** pulsa el botón "Atrás" del móvil o una "X" (Cancelar) en la cabecera.
        
    - **Entonces** la app debe mostrar un pop-up de confirmación: "¿Seguro que quieres descartar este envío?" y si acepta, volver al Dashboard [R-02] sin guardar nada.
        

#### 🎨 Notas de Diseño y Técnicas (Para el Desarrollador)

- **API:** Requiere un `POST /shipments`. El _backend_ recibirá los datos y creará la fila en la tabla `shipments` con el `status = 'REGISTERED'`.
    
- **Coste:** La API de Google Places Autocomplete **tiene un coste**. Es vital para el negocio.
    
- **Teclado:** Usar `keyboardType="phone"` para el teléfono y `keyboardType="decimal-pad"` para el importe.
    

## [R-06] Ver Detalles de un Envío

> Como Dueño de Restaurante,
> 
> Quiero pulsar en una "tarjeta" de envío en mi Kanban (en cualquier columna),
> 
> Para ver todos sus detalles (repartidor asignado, notas completas, tiempos, etc.).

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-R06.1] Acceso]**
    
    - **Dado** que el usuario está en el Dashboard Kanban [R-02].
        
    - **Cuando** pulsa sobre cualquier tarjeta de envío (en cualquier columna).
        
    - **Entonces** debe navegar a una nueva pantalla: "Detalles del Envío".
        
- **[AC-R06.2] Información Estática (Datos del Pedido)]**
    
    - **Dado** que está en la pantalla de "Detalles".
        
    - **Entonces** debe ver claramente _toda_ la información que introdujo en [R-03]:
        
        - Nombre del Cliente
            
        - Dirección Completa
            
        - Teléfono del Cliente (clicable para llamar si es necesario).
            
        - Notas del Pedido
            
        - Método de Cobro (Ej. "Cobrar 35.50€ con TPV")
            
- **[AC-R06.3] Información Dinámica (Datos de Logística)]**
    
    - **Dado** que está en la pantalla de "Detalles".
        
    - **Entonces** debe ver la información de estado actualizada por el sistema:
        
        - **Estado Actual:** (Ej. "En Ruta")
            
        - **Repartidor Asignado:** (Ej. "Carlos Pérez" o "Buscando repartidor..." si [MOTOR-01] aún está calculando).
            
        - **Tiempos:**
            
            - Hora de Creación: "14:15h"
                
            - Preparación Iniciada: "14:17h"
                
            - Listo para Recoger (Estimado): "14:32h"
                
            - Entregado (Estimado): "14:50h"
                
- **[AC-R06.4] Acciones Contextuales (¡NUEVO!)**
    
    - **Dado** que el usuario está en la pantalla de "Detalles".
        
    - **Cuando** el pedido tiene estado `IN_PROGRESS` ("En Preparación").
        
    - **Entonces** los botones "+5 min", "+10 min" y "Listo para Recoger" (de [R-02.7] y [R-02.8]) deben estar visibles y funcionales aquí también.
        
- **[AC-R06.5] Solo Lectura (Resto de Estados)]**
    
    - **Dado** que el pedido está en estado `REGISTERED`, `READY_FOR_PICKUP`, `IN_TRANSIT` o `DELIVERED`.
        
    - **Entonces** toda la información es de **solo lectura**. (Excepto la acción "Empezar Preparación" si está en `REGISTERED`).
        
    - **(Importante):** No hay botón de "Cancelar Envío" en el MVP V1. Una cancelación es una _incidencia_ que se gestiona manualmente por teléfono contigo (Admin).
        
- **[AC-R06.6] Navegación]**
    
    - **Dado** que el usuario está en la pantalla de "Detalles".
        
    - **Cuando** pulsa el botón "Atrás".
        
    - **Entonces** debe volver al Dashboard Kanban [R-02].
        

#### 🎨 Notas de Diseño y Técnicas (Para el Desarrollador)

- **UI:** Una pantalla simple de "Detalle". Usar iconos para diferenciar la información.
    
- **API:** Puede usar los datos ya cargados en [R-02] (pasando el objeto `Shipment` completo a la nueva pantalla) y/o hacer una llamada `GET /shipments/{id}` para obtener los datos más frescos (como el repartidor asignado).
    

## [R-07] Ver Estadísticas y Comisiones (¡NUEVO!)

> Como Dueño de Restaurante,
> 
> Quiero ver un resumen simple de mis ventas y la comisión que debo pagar a la plataforma,
> 
> Para entender la rentabilidad de mi servicio y gestionar mis finanzas.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-R07.1] Acceso a Estadísticas**
    
    - **Dado** que el usuario (Restaurante) está en su módulo.
        
    - **Entonces** debe haber una sección/pestaña clara de "Mis Ganancias" o "Estadísticas".
        
- **[AC-R07.2] Resumen de Ventas (KPIs)**
    
    - **Dado** que el usuario abre "Estadísticas" (con un filtro de tiempo: "Hoy", "Semana Actual", "Mes Actual").
        
    - **Entonces** debe ver tarjetas (KPIs) simples:
        
        - **"Total Ventas Gestionadas"** (Suma de `amount_to_collect` de todos los `DELIVERED`).
            
        - **"Total Pedidos Completados"** (Conteo de `DELIVERED`).
            
- **[AC-R07.3] Cálculo de Comisión (Crítico V1.5)**
    
    - **Dado** que el Admin [A-06] ha configurado una comisión (ej. 10%) en `restaurants.commission_rate`.
        
    - **Cuando** el restaurante ve sus estadísticas.
        
    - **Entonces** debe ver claramente:
        
        - **"Comisión de Plataforma"** (ej. 10% de "Total Ventas Gestionadas").
            
        - **"Balance a Liquidar por Restaurante"** (Total Ventas - Comisión). (Esto es lo que el restaurante _recibe_ de nosotros, o _paga_, dependiendo del modelo de liquidación).
            
- **[AC-R07.4] Lista de Pedidos (Detalle)**
    
    - **Dado** que el usuario ve el resumen.
        
    - **Entonces** debe haber una lista simple de los pedidos (`shipments`) incluidos en ese cálculo, mostrando `Fecha`, `Cliente`, `Importe Total` y `Comisión Aplicada` a ese pedido.
        

#### ➡️ Happy Path (Flujo Ideal)

1. Luigi va a la pestaña "Estadísticas" y filtra por "Semana Actual".
    
2. Ve: "Total Ventas: 850€", "Total Pedidos: 40".
    
3. Ve: "Comisión (10%): 85€".
    
4. Ve: "Balance a Liquidar: 765€".
    
5. Sabe exactamente cuánto dinero le debe la plataforma (o él a la plataforma).
    

#### 🎨 Notas de Diseño y Técnicas (Para el Desarrollador)

- **API:** Requiere `GET /restaurants/stats?range=week`. El _backend_ calculará esto basado en `shipments` (solo `DELIVERED`) y `restaurants.commission_rate`.
    
- **Lógica:** Esto da transparencia total al restaurante y justifica el cobro del servicio. Es vital para la retención del restaurante.