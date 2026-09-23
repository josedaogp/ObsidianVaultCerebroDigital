# 🚚 Módulo 2: Repartidor (La Hoja de Ruta)

Este módulo es la herramienta de trabajo del repartidor. Debe ser rápida, clara e "infallible".

## [D-01] Login y Disponibilidad

> Como Repartidor,
> 
> Quiero poder iniciar sesión y marcarme como 'Disponible' o 'No Disponible',
> 
> Para empezar o terminar mi turno y que el Motor Logístico [MOTOR-01] sepa si puede contar conmigo.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-D01.1] Login del Repartidor**
    
    - **Dado** que el usuario tiene el rol `driver` en la BBDD [CORE-01].
        
    - **Cuando** inicia sesión en la app.
        
    - **Entonces** es redirigido al Módulo de Repartidor (la pantalla "Mi Hoja de Ruta" [D-02]).
        
    - **Y** la sesión debe persistir (igual que [R-01.7]).
        
- **[AC-D01.2] Interruptor de Disponibilidad (Switch)**
    
    - **Dado** que el repartidor está en su pantalla principal.
        
    - **Entonces** debe ver un interruptor (switch) global y prominente: **"No Disponible" (Rojo) / "Disponible" (Verde)**.
        
- **[AC-D01.3] Ponerse "Disponible"**
    
    - **Dado** que el repartidor está "No Disponible" y no tiene un turno (`shift`) activo.
        
    - **Cuando** pulsa el interruptor a "Disponible".
        
    - **Entonces** la app debe primero mostrarle el pop-up de "Bote Inicial" [D-07].
        
    - **Y** una vez confirmado el bote, la app debe llamar a la API (`POST /drivers/status`) para cambiar su `drivers.current_status` a `ONLINE`.
        
    - **Y** (Opcional V1.1) empezar a enviar su `current_location` (GPS) al servidor.
        
    - **Y** el Motor [MOTOR-01] y el Admin [A-01] ahora lo ven como "Disponible".
        
- **[AC-D01.4] Ponerse "No Disponible"**
    
    - **Dado** que el repartidor está "Disponible".
        
    - **Cuando** pulsa el interruptor a "No Disponible".
        
    - **Entonces** la app debe llamar a la API para cambiar su estado a `OFFLINE`.
        
    - **Y** el Motor [MOTOR-01] deja de considerarlo para _nuevas_ asignaciones.
        
- **[AC-D01.5] Bloqueo de Disponibilidad (Crítico)**
    
    - **Dado** que el repartidor tiene tareas _activas_ en su Hoja de Ruta [D-02].
        
    - **Cuando** intenta ponerse "No Disponible".
        
    - **Entonces** la app debe impedirlo y mostrar un aviso: "Debes completar (o reportar) tus entregas actuales antes de desconectarte."
        

#### ➡️ Happy Path (Flujo Ideal)

1. Carlos (Repartidor) inicia sesión [CORE-01].
    
2. Aterriza en su "Hoja de Ruta" [D-02] (vacía).
    
3. Pulsa el switch a "Disponible".
    
4. La app le pregunta por su "Bote Inicial" [D-07]. Escribe "50.00" y acepta.
    
5. El switch se pone verde. Ahora está "ONLINE" y esperando que el Motor [MOTOR-01] le asigne trabajo.
    

#### ⚠️ Edge Cases y Errores (Casos Borde)

- **Sin Conexión:** Al cambiar el switch, la app no puede conectar. Debe mostrar un error "Sin conexión" y revertir el switch a su posición anterior.
    

#### 🎨 Notas de Diseño y Técnicas (Para el Desarrollador)

- **API:** `POST /drivers/status` (body: `{"status": "ONLINE" | "OFFLINE"}`). `POST /drivers/location` (body: `{"lat": ..., "lng": ...}`).
    
- **BBDD:** El switch actualiza `drivers.current_status`.
    

## [D-02] Recibir Asignación (Hoja de Ruta)

> Como Repartidor,
> 
> Quiero recibir mi lista de tareas (batch) asignadas por el Motor [MOTOR-01] en un orden secuencial claro,
> 
> Para saber exactamente cuál es mi ruta (recogidas y entregas).

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-D02.1] Lista de Tareas (Hoja de Ruta)**
    
    - **Dado** que el repartidor está "Disponible" [D-01].
        
    - **Cuando** el Motor [MOTOR-01] o el Admin [A-02] le asigna un _batch_.
        
    - **Entonces** su pantalla principal ("Mi Hoja de Ruta") debe poblarse con una lista de tareas (de la tabla `driver_tasks`).
        
- **[AC-D02.2] Notificación (V1 vs V1.1)**
    
    - **(Alcance MVP V1):** La app debe hacer _polling_ (consultar a la API cada 15-30 segundos) `GET /drivers/tasks` para ver si tiene nuevas tareas.
        
    - **(Alcance V1.1):** El servidor debe enviar una Notificación Push para despertar la app y avisar "¡Nuevo lote asignado!".
        
- **[AC-D02.3] Estructura Secuencial (Crítico)**
    
    - **Dado** que el repartidor ve su Hoja de Ruta.
        
    - **Entonces** las tareas deben estar _ordenadas_ por `driver_tasks.sequence` (1, 2, 3...).
        
    - **Ejemplo de Batch (2 pedidos):**
        
        1. **"RECOGER en Pizzería La Tradicional"** (2 pedidos)
            
        2. **"ENTREGAR a Ana Gómez"** (Cobrar 15.00€ Efectivo)
            
        3. **"ENTREGAR a Benito Sánchez"** (Cobrar 19.00€ TPV)
            
- **[AC-D02.4] Foco en Tarea Actual (Crítico)**
    
    - **Dado** que el repartidor ve la lista de 3 tareas.
        
    - **Entonces** solo la Tarea 1 ("RECOGER") debe estar activa y clicable.
        
    - **Y** las Tareas 2 y 3 deben estar "bloqueadas" (en gris, no clicables).
        
    - **Y** solo cuando complete la Tarea 1 (pulsando "Recogido" en [D-05]), la Tarea 2 se volverá activa.
        

#### ➡️ Happy Path (Flujo Ideal)

1. Carlos está "ONLINE".
    
2. El Motor [MOTOR-01] le asigna un _batch_ de 2 pedidos de la Pizzería.
    
3. La app de Carlos refresca la lista. Ahora ve 3 pasos (Recoger, Entregar, Entregar).
    
4. La Tarea 1 ("RECOGER...") está resaltada. Pulsa sobre ella para ver los detalles [D-03].
    

#### 🎨 Notas de Diseño y Técnicas (Para el Desarrollador)

- **API:** `GET /drivers/tasks` (debe devolver solo las tareas `PENDING` del `batch` actual, ordenadas por `sequence`).
    
- **UI:** Una `ListView` / `RecyclerView`. Cada ítem es una "Tarea" (`driver_tasks`).
    

## [D-03] Ver Detalles de Entrega y Cobro

> Como Repartidor,
> 
> Quiero ver claramente la dirección, notas y la instrucción de cobro (Efectivo/TPV/Pagado) para cada tarea,
> 
> Para evitar errores en la entrega y en el cobro.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-D03.1] Acceso al Detalle**
    
    - **Dado** que el repartidor está en su Hoja de Ruta [D-02].
        
    - **Cuando** pulsa en la tarea _activa_ (ej. "ENTREGAR a Ana Gómez").
        
    - **Entonces** navega a la pantalla "Detalle de Tarea".
        
- **[AC-D03.2] Información de Tarea (Recogida)**
    
    - **Dado** que la tarea es `task_type = 'PICKUP'`.
        
    - **Entonces** debe ver:
        
        - Nombre del Restaurante (Pizzería La Tradicional)
            
        - Dirección del Restaurante
            
        - Nº de pedidos a recoger (ej. "2 pedidos")
            
        - Botón de Navegación [D-04].
            
        - Botón "He Recogido" [D-05].
            
- **[AC-D03.3] Información de Tarea (Entrega)**
    
    - **Dado** que la tarea es `task_type = 'DELIVERY'`.
        
    - **Entonces** debe ver:
        
        - Nombre del Cliente (Ana Gómez)
            
        - Dirección del Cliente
            
        - Botón para llamar (`TEL:`) al `client_phone` (¡Vital!).
            
        - Notas del Restaurante (ej. "Piso 3, puerta A").
            
        - Botón de Navegación [D-04].
            
        - Botón "He Entregado" [D-05].
            
- **[AC-D03.4] Instrucción de Cobro (La más visible)**
    
    - **Dado** que la tarea es `task_type = 'DELIVERY'`.
        
    - **Entonces** debe haber una sección (ej. un recuadro grande, en rojo o verde) que indique la acción de cobro (del `shipments.payment_type` y `amount_to_collect`):
        
        1. **"¡COBRAR 35.50 € con TPV!"**
            
        2. **"¡COBRAR 15.00 € EN EFECTIVO!"**
            
        3. **"YA PAGADO. (No cobrar nada. Solo entregar)"**
            
- **[AC-D03.5] Botón de Incidencia**
    
    - **Dado** que está en una tarea (Recogida o Entrega).
        
    - **Entonces** debe haber un botón/link secundario: "Reportar Incidencia" (para [D-06]).
        

#### 🎨 Notas de Diseño y Técnicas (Para el Desarrollador)

- **UI:** Alto contraste. La información de cobro debe ser inequívoca. Usar iconos (💸, 💳, ✅) y colores.
    
- **Llamada:** Usar la función nativa del OS para iniciar una llamada (`TEL:`).
    

## [D-04] Navegación GPS

> Como Repartidor,
> 
> Quiero pulsar un botón de "Navegar" en la dirección (del restaurante o del cliente),
> 
> Para abrir la ruta en mi app de GPS (Google Maps/Waze) sin tener que teclearla.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-D04.1] Botón de Navegación**
    
    - **Dado** que el repartidor está en la pantalla de "Detalle de Tarea" [D-03] (sea `PICKUP` o `DELIVERY`).
        
    - **Entonces** debe haber un botón claro (ej. "Navegar" o un icono de mapa) al lado de la dirección.
        
- **[AC-D04.2] Lanzar App Externa**
    
    - **Dado** que el repartidor pulsa el botón "Navegar".
        
    - **Cuando** lo hace.
        
    - **Entonces** la app debe abrir la aplicación de mapas por defecto del dispositivo (Google Maps, Apple Maps, Waze).
        
- **[AC-D04.3] Pasar Coordenadas (Crítico)**
    
    - **Dado** que la app externa se abre.
        
    - **Cuando** se abre.
        
    - **Entonces** la ruta debe estar ya calculada hacia las coordenadas (`lat/lng`) de la tarea.
        
    - **(Importante):** Usará `restaurants.location` (si es PICKUP) o `shipments.client_location` (si es DELIVERY). Esto depende de que [R-03.3] haya funcionado bien.
        

#### ➡️ Happy Path (Flujo Ideal)

1. Carlos está en la pantalla de "RECOGER en Pizzería La Tradicional".
    
2. Pulsa el botón "Navegar".
    
3. Se abre Google Maps con la ruta más rápida a la pizzería.
    

#### ⚠️ Edge Cases y Errores (Casos Borde)

- **Coordenadas Malignas:** Si la dirección no tiene coordenadas (un error de BBDD), el botón debe estar deshabilitado o intentar una búsqueda por el texto de la dirección (menos fiable).
    

#### 🎨 Notas de Diseño y Técnicas (Para el Desarrollador)

- **Tecnología:** Se implementa usando `Intent` (Android) o `URL Schemes` (iOS) para lanzar aplicaciones de terceros pasando parámetros de geolocalización.
    

## [D-05] Marcar Estados del Envío

> Como Repartidor,
> 
> Quiero marcar cuándo he 'Recogido' un envío y cuándo lo he 'Entregado',
> 
> Para actualizar el estado al restaurante y al admin, y desbloquear mi siguiente tarea.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-D05.1] Acción de "Recogido"**
    
    - **Dado** que el repartidor está en la tarea `PICKUP` [D-03] (ej. en "Pizzería La Tradicional").
        
    - **Cuando** tiene la comida en la mano y pulsa el botón "He Recogido".
        
    - **Entonces** la app debe llamar a la API (`POST /tasks/{id}/complete`).
        
    - **Y** el _backend_ debe:
        
        1. Marcar `driver_tasks.status` = `COMPLETED`.
            
        2. Actualizar el `shipment.status` (de todos los pedidos del _batch_ en esa recogida) a `IN_TRANSIT`.
            
        3. Actualizar `shipment.time_picked_up`.
            
    - **Y** la app del Restaurante [R-02] debe mover esas tarjetas a la columna "En Ruta".
        
    - **Y** la app del Repartidor [D-02] debe marcar la Tarea 1 como completa y activar la Tarea 2.
        
- **[AC-D05.2] Acción de "Entregado"**
    
    - **Dado** que el repartidor está en la tarea `DELIVERY` [D-03] (ej. "Ana Gómez").
        
    - **Cuando** ha entregado la comida y realizado el cobro, pulsa el botón "He Entregado".
        
    - **Entonces** la app debe llamar a la API (`POST /tasks/{id}/complete`).
        
    - **Y** el _backend_ debe:
        
        1. Marcar `driver_tasks.status` = `COMPLETED`.
            
        2. Actualizar el `shipment.status` a `DELIVERED`.
            
        3. Actualizar `shipment.time_delivered`.
            
    - **Y** la app del Restaurante [R-02] debe mover esa tarjeta a la columna "Entregados".
        
    - **Y** la app del Repartidor [D-02] debe marcar la Tarea 2 como completa y activar la Tarea 3 (o finalizar el _batch_).
        
- **[AC-D05.3] Sincronización Offline (Crítico V1.1)**
    
    - **Dado** que el repartidor pulsa "He Entregado" pero está en un sótano sin cobertura.
        
    - **(Alcance MVP V1):** La app muestra un error "Sin conexión" y el repartidor debe reintentarlo.
        
    - **(Alcance V1.1):** La app actualiza la UI localmente (marca como "Pendiente de Sincronizar"), le permite continuar, y reintenta la llamada a la API en segundo plano.
        

#### ➡️ Happy Path (Flujo Ideal)

1. Carlos coge los pedidos. Pulsa "He Recogido". La Tarea 1 se tacha. La Tarea 2 ("ENTREGAR a Ana") se activa.
    
2. Va a casa de Ana, cobra. Pulsa "He Entregado". La Tarea 2 se tacha. La Tarea 3 se activa.
    
3. Repite. Al terminar, su "Hoja de Ruta" [D-02] vuelve a estar vacía.
    

#### 🎨 Notas de Diseño y Técnicas (Para el Desarrollador)

- **Lógica:** Esta es la "máquina de estados" principal. Cada acción del repartidor dispara una actualización en el _backend_ que ven todos los demás actores.
    
- **API:** `POST /tasks/{id}/complete`.
    

## [D-06] Añadir Nota de Incidencia

> Como Repartidor,
> 
> Quiero poder añadir una 'Nota de Incidencia' si un envío falla (ej. cliente no está, TPV sin batería),
> 
> Para que el Admin lo sepa y quede registrado (la "válvula de escape").

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-D06.1] Acceso a Incidencia**
    
    - **Dado** que el repartidor está en una tarea de "Detalle" [D-03].
        
    - **Entonces** debe haber un botón/link secundario (ej. "Reportar Incidencia").
        
- **[AC-D06.2] Formulario de Incidencia**
    
    - **Dado** que el repartidor pulsa "Reportar Incidencia".
        
    - **Cuando** lo hace.
        
    - **Entonces** aparece un pop-up o pantalla simple con un campo de texto `Notas` y un botón "Enviar Nota".
        
- **[AC-D06.3] Envío de Nota a BBDD**
    
    - **Dado** que el repartidor escribe (ej. "Cliente no contesta al teléfono ni al timbre") y pulsa "Enviar Nota".
        
    - **Cuando** lo hace.
        
    - **Entonces** la app debe llamar a la API (`POST /incidents`) para guardar esa nota en la tabla `incident_reports`, vinculada al `shipment_id`.
        
    - **Y** el Admin debe ver esta nota en su panel [A-05].
        
- **[AC-D06.4] Impacto en Tarea (Gestión MVP)**
    
    - **Dado** que el repartidor envía una incidencia.
        
    - **Entonces** la tarea _no_ se marca como completada. Queda "Pendiente".
        
    - **(Importante):** El software solo _registra_ el problema. El repartidor debe **llamar por teléfono al Admin** para una resolución manual (ej. "El Admin me dice que cancele el pedido" o "El Admin me dice que vuelva en 10 min"). El Admin usará [A-02] para re-asignar o cancelar.
        

#### ➡️ Happy Path (Flujo Ideal)

1. Carlos está en casa de Ana, pero no contesta.
    
2. Carlos pulsa "Reportar Incidencia".
    
3. Escribe: "No contesta teléfono. He llamado al timbre 5 min." Pulsa "Enviar Nota".
    
4. La nota se guarda. Carlos llama por teléfono al Admin: "Oye, ¿qué hago con el pedido de Ana (ID 123)?".
    
5. El Admin ve la nota en [A-05] y le da instrucciones.
    

#### 🎨 Notas de Diseño y Técnicas (Para el Desarrollador)

- **API:** `POST /incidents` (body: `{"shipment_id": ..., "notes": "..."}`).
    
- **Valor:** Esta es la "válvula de escape" manual. Previene que el repartidor marque como "Entregado" algo que no lo ha sido.
    

## [D-07] Bote Inicial del Repartidor

> Como Repartidor,
> 
> Quiero introducir mi "bote" (cambio inicial en efectivo) al empezar mi turno,
> 
> Para que el Admin pueda hacer la liquidación [A-03] correctamente.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-D07.1] Pop-up al Iniciar Turno**
    
    - **Dado** que el repartidor está "No Disponible" y no tiene un turno (`driver_shifts`) activo.
        
    - **Cuando** pulsa el interruptor a "Disponible" [D-01.3].
        
    - **Entonces** (antes de ponerse ONLINE) la app debe mostrar un pop-up: "¡Empecemos! ¿Con cuánto cambio (bote) empiezas hoy?".
        
    - **Y** debe haber un campo numérico (decimal) y un botón "Empezar Turno".
        
- **[AC-D07.2] Guardar Turno (Shift) en BBDD**
    
    - **Dado** que el repartidor introduce "50.00" y pulsa "Empezar Turno".
        
    - **Cuando** lo hace.
        
    - **Entonces** la app debe llamar a la API (`POST /shifts/start`) para crear una nueva fila en `driver_shifts` con el `driver_id`, `start_time` y `initial_float = 50.00`.
        
    - **Y** solo después de esto, el repartidor se pone "Disponible" [D-01.3].
        
- **[AC-D07.3] Ver Bote Actual**
    
    - **Dado** que el repartidor está en un turno activo.
        
    - **Entonces** debe poder ver en algún sitio (ej. "Mi Perfil") su bote inicial de ese día.
        

#### ➡️ Happy Path (Flujo Ideal)

1. Carlos pulsa "Disponible".
    
2. Aparece pop-up: "¿Bote de hoy?".
    
3. Escribe "50.00" y pulsa "Empezar Turno".
    
4. Se crea el `shift` en la BBDD. Su estado cambia a "ONLINE". Está listo.
    
5. Al final del día, el Admin [A-03] verá esos 50.00€ en su panel de liquidación.
    

#### ⚠️ Edge Cases y Errores (Casos Borde)

- **Input Inválido:** El repartidor escribe "abc" (debe ser numérico) o "0.00" (debe ser > 0, o al menos advertir).
    
- **Turno ya Activo:** Si el repartidor cierra la app y la vuelve a abrir (pero sigue de turno), la app _no_ debe pedir el bote de nuevo. Debe detectar que hay un `driver_shifts` con `status = 'ACTIVE'`.
    

#### 🎨 Notas de Diseño y Técnicas (Para el Desarrollador)

- **API:** `POST /shifts/start` (body: `{"initial_float": 50.00}`).
    
- **Lógica:** Esta US es el pilar de la contabilidad [A-03].