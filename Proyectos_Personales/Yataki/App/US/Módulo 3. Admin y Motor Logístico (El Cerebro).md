# 💻 Módulo 3: Admin y Motor Logístico (El Cerebro)

Este es el cambio más grande. El Admin ya no es un "operador", es un "supervisor". El "Motor" (algoritmo) hace el trabajo pesado.

## [A-01] Dashboard de Operaciones (MODIFICADO)

> Como Admin,
> 
> Quiero un dashboard que me muestre en tiempo real dónde están mis repartidores y el estado de todos los pedidos,
> 
> Para supervisar que el algoritmo está funcionando bien.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-A01.1: Vista de Mapa (Crítico)]**
    
    - **Dado** que el Admin abre el panel.
        
    - **Entonces** ve un mapa (Mapbox/Google Maps) como elemento principal.
        
    - **Y** el mapa muestra:
        
        - Iconos de Restaurantes.
            
        - Iconos de Repartidores "Disponibles" (moviéndose en tiempo real, si el GPS está activo).
            
        - Iconos de Pedidos "En Preparación" (en el restaurante).
            
        - Iconos de Pedidos "Listos para Recoger" (parpadeando en el restaurante).
            
- **[AC-A01.2: Listas de Supervisión]**
    
    - **Dado** que el Admin está en el panel.
        
    - **Entonces** ve listas/columnas:
        
        - "Pedidos en Alerta" (ej. pedidos que el algoritmo no ha podido asignar).
            
        - "Repartidores Activos" (y cuántas tareas tienen).
            
        - "Log de Actividad" (ej. "Motor asignó Pedido 15 a Carlos").
            

## [MOTOR-01] El Motor de Logística (Algoritmo) (¡NUEVO!)

> Como Sistema (El Motor),
> 
> Quiero recibir pedidos confirmados ("En Preparación"), los tiempos de preparación y la ubicación de los repartidores disponibles,
> 
> Para calcular automáticamente el batch (lote) óptimo y la ruta secuencial [D-02] para el repartidor.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-M01.1: Disparador (Trigger)]**
    
    - **Dado** que un Restaurante [R-02.5] pulsa "Confirmar y Empezar Preparación".
        
    - **Cuando** esto ocurre.
        
    - **Entonces** el pedido (con su `tiempo_preparacion_base` de [R-00] y su `direccion_cliente`) entra en la "piscina" (pool) del Motor Logístico.
        
- **[AC-M01.2: Lógica de Cálculo (El "Cerebro")]**
    
    - **Dado** que el Motor tiene 3 pedidos "En Preparación" en "Pizzería Luigi" (listos en 10, 15 y 20 min).
        
    - **Y** tiene 2 repartidores disponibles (Carlos a 5 min, María a 10 min).
        
    - **Cuando** el Motor se ejecuta (ej. cada 30 segundos).
        
    - **Entonces** debe calcular la ruta óptima: ¿Es mejor enviar a Carlos a por el primer pedido solo? ¿O esperar 10 min y que Carlos recoja los 3 juntos (_batching_)?
        
    - **Variables de entrada:** `tiempo_preparacion`, `tiempo_viaje_repartidor_a_restaurante`, `tiempo_viaje_restaurante_a_clientes`.
        
- **[AC-M01.3: Asignación Automática]**
    
    - **Dado** que el Motor decide que el _batch_ óptimo es que Carlos recoja los 3 pedidos.
        
    - **Cuando** el primer pedido esté casi "Listo".
        
    - **Entonces** el Motor debe (automáticamente) llamar a la API `POST /admin/assign_batch` (la misma que [A-02]) y asignar la Hoja de Ruta [D-02] a Carlos.
        
- **[AC-M01.4: Gestión de Tiempos Extra]**
    
    - **Dado** que el Motor ya asignó un _batch_ a Carlos (para llegar a las 14:30).
        
    - **Cuando** el Restaurante [R-02.6] pulsa `+5 min`.
        
    - **Entonces** el Motor debe ser notificado y recalcular: ¿Sigue siendo Carlos el repartidor óptimo? ¿O hay que re-asignar el _batch_?
        

## [A-02] Asignación Manual (Botón de Pánico) (MODIFICADO)

> Como Admin,
> 
> Quiero poder anular al algoritmo y asignar manualmente un pedido a un repartidor,
> 
> Para resolver incidencias o "pedidos en alerta" que el motor no ha podido gestionar.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-A02.1: Anulación Manual]**
    
    - **Dado** que el Admin ve un pedido "En Alerta" (en [A-01]).
        
    - **Cuando** pulsa "Asignar Manualmente".
        
    - **Entonces** ve el mismo flujo de [A-02] (Checkbox, seleccionar repartidor, confirmar).
        
- **[AC-A02.2: Detener Algoritmo]**
    
    - **Dado** que el Admin asigna manualmente un pedido.
        
    - **Entonces** el Motor [MOTOR-01] debe ser notificado de que ese pedido "ya no está en la piscina" y debe dejar de intentar asignarlo.
        

## [A-03] Panel de Liquidación (MODIFICADO)

> Como Admin,
> 
> Quiero un panel de 'Liquidación' que incluya el "bote" inicial del repartidor,
> 
> Para saber exactamente cuánto efectivo total debe tener en mano.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-A03.1: Vista de Liquidación (MODIFICADA)]**
    
    - **Dado** que el Admin abre "Liquidaciones" (filtra por "Hoy").
        
    - **Entonces** ve la tabla por repartidor con **nuevas columnas**:
        
        - `Repartidor`
            
        - `Bote Inicial` (de [D-07], ej. 50.00 €)
            
        - `Total Efectivo Cobrado` (ej. 120.50 €)
            
        - `Total TPV Cobrado` (ej. 85.00 €)
            
        - `Total en Mano (Esperado)` (Bote + Efectivo = 170.50 €)
            
        - `Estado` (Pendiente / Liquidado)
            
- **[AC-A03.2: Desglose por Repartidor (Drill-down)] (¡NUEVO DETALLE!)**
    
    - **Dado** que el Admin pulsa en la fila de un repartidor (ej. "Carlos Pérez").
        
    - **Entonces** navega a una vista de "Detalle de Liquidación" para ese turno (`shift_id`).
        
    - **Y** ve una lista detallada de todos los envíos (`shipments`) completados en ese turno, mostrando: `Cliente`, `Restaurante`, `Tipo de Cobro` e `Importe`.
        
    - **Y** ve un desglose por restaurante (Ej. "Total Pizzería Luigi: 50€ Efectivo, 30€ TPV", "Total Asador Pepe: 70.50€ Efectivo, 55€ TPV").
        
- **(¡NUEVO!)** Esta US se referenciaba pero no estaba detallada en este documento.
    

## [A-04] Marcar Liquidación Manual (¡NUEVO!)

> Como Admin,
> 
> Quiero poder marcar manualmente un turno ("shift") como 'Liquidado (OK)',
> 
> Para cerrar la contabilidad del día después de recibir físicamente el efectivo y los slips del TPV.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-A04.1: Acción de Liquidar]**
    
    - **Dado** que el Admin está en el panel de `[A-03]` y ve un turno con estado "Pendiente".
        
    - **Entonces** hay un botón/acción clara de "Liquidar Turno".
        
- **[AC-A04.2: Proceso Físico (Contexto)]**
    
    - **(Contexto):** El Admin se reúne físicamente con el repartidor. El repartidor le entrega el `Total Efectivo Cobrado` (ej. 120.50€) y los _slips_ (recibos) del TPV por el `Total TPV Cobrado` (ej. 85.00€). El repartidor se queda con su `Bote Inicial` (ej. 50€).
        
- **[AC-A04.3: Pop-up de Confirmación y Notas (Tu "Configurabilidad")]**
    
    - **Dado** que el Admin pulsa "Liquidar Turno" en el turno de Carlos (Total Esperado en Mano: 170.50€).
        
    - **Cuando** el Admin ha contado el dinero y comprobado los slips.
        
    - **Entonces** aparece un pop-up: "¿Confirmas la liquidación de Carlos? Total cobrado (Efectivo+TPV): 205.50€. Total esperado (Bote+Efectivo): 170.50€."
        
    - **Y** hay un campo de `Notas de Liquidación` (opcional) para registrar descuadres.
        
    - **Y** un botón final de "Confirmar Liquidación".
        
- **[AC-A04.4: Cambio de Estado y Registro]**
    
    - **Dado** que el Admin pulsa "Confirmar Liquidación".
        
    - **Entonces** se llama a la API (`POST /admin/shifts/{id}/liquidate`) guardando las `notas` y el `admin_user_id` que lo aprueba.
        
    - **Y** el `driver_shifts.status` cambia a `LIQUIDATED`.
        
    - **Y** el `driver_shifts.liquidated_at` se rellena.
        
    - **Y** el estado en la tabla `[A-03]` se refresca a "Liquidado" (verde).
        

#### ➡️ Happy Path (Flujo Ideal)

1. Admin y Carlos se reúnen. Carlos entrega 120.50€ en efectivo y 85.00€ en recibos de TPV.
    
2. El Admin cuenta y comprueba. Todo OK.
    
3. El Admin pulsa "Liquidar Turno" en el panel. Pulsa "Confirmar Liquidación" en el pop-up.
    
4. El turno de Carlos se marca como "Liquidado".
    

#### ⚠️ Edge Cases y Errores (Casos Borde)

- **Descuadre de Caja:** Carlos entrega 110€ en lugar de 120.50€. Faltan 10.50€.
    
    - _Solución:_ El Admin igualmente pulsa "Confirmar Liquidación", pero en el campo `Notas de Liquidación` [AC-A04.3] escribe: "Faltan 10.50€. Carlos informa que el cliente del pedido X (ID 45) no le pagó bien." La liquidación se cierra, pero el descuadre queda registrado en `[A-05]`.
        

#### 🎨 Notas de Diseño y Técnicas (Para el Desarrollador)

- **API:** `POST /admin/shifts/{shift_id}/liquidate` que acepte `{"notes": "..."}`.
    
- **Seguridad:** Esta acción debe estar restringida solo a usuarios con rol `admin`.
    

## [A-05] Ver Notas e Incidencias (¡DETALLADO!)

> Como Admin,
> 
> Quiero un 'Buzón de Incidencias' centralizado,
> 
> Para ver y gestionar todos los problemas reportados por repartidores [D-06] y las notas de descuadre de las liquidaciones [A-04].

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-A05.1: Acceso al Buzón]**
    
    - **Dado** que el Admin está en el panel web.
        
    - **Entonces** hay una pestaña/sección claramente visible llamada "Buzón de Incidencias".
        
- **[AC-A05.2: Ver Incidencias de Repartidor [D-06]]**
    
    - **Dado** que el Admin abre el "Buzón".
        
    - **Entonces** ve una lista/tabla de todas las incidencias de la tabla `incident_reports`.
        
    - **Y** debe mostrar columnas clave: `Fecha/Hora`, `Repartidor`, `Pedido (Cliente/Restaurante)`, `Estado (Pendiente/Resuelta)`, `Nota:` ("Cliente no contesta al teléfono...").
        
- **[AC-A05.3: Ver Notas de Liquidación [A-04]]**
    
    - **Dado** que el Admin abre el "Buzón" (o un filtro dentro de este).
        
    - **Entonces** puede ver una lista de todas las `liquidation_notes` que no están vacías (los descuadres de caja).
        
    - **Y** debe mostrar: `Fecha/Hora`, `Repartidor (Turno)`, `Nota:` ("Faltan 10.50€...").
        
- **[AC-A05.4: Marcar como Resuelta (Gestión MVP)]**
    
    - **Dado** que el Admin ve una incidencia con estado "Pendiente" (ej. "Cliente no contesta").
        
    - **Cuando** ha gestionado el problema (ej. ha llamado al cliente y cancelado el pedido en la BBDD).
        
    - **Entonces** puede marcar la incidencia como "Resuelta" (un simple `boolean` en la tabla `incident_reports`).
        

#### ➡️ Happy Path (Flujo Ideal)

1. Un repartidor [D-06] reporta "Cliente no contesta".
    
2. El Admin ve la nueva incidencia "Pendiente" en su buzón [A-05].
    
3. El Admin llama al cliente (no contesta) y luego al restaurante (para cancelar el pedido).
    
4. El Admin marca la incidencia como "Resuelta".
    

#### 🎨 Notas de Diseño y Técnicas (Para el Desarrollador)

- **API:** `GET /admin/incidents` (con filtros por `status`) y `GET /admin/shifts?liquidated=true&notes_exist=true`.
    
- **Valor:** Esta es la "red de seguridad" operativa. Es donde se gestionan todas las excepciones que el software no puede manejar automáticamente.
    

## [A-06] Gestión de Restaurantes (¡NUEVO!)

> Como Admin,
> 
> Quiero poder crear y gestionar las cuentas de los restaurantes,
> 
> Para darles acceso a la app y gestionar sus comisiones.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-A06.1: Crear Restaurante]**
    
    - **Dado** que el Admin está en el panel.
        
    - **Entonces** ve una sección "Restaurantes".
        
    - **Y** puede "Crear Restaurante" introduciendo: `Nombre`, `Dirección`, `Email de Login`, `Contraseña Temporal`.
        
- **[AC-A06.2: Gestionar Comisión]**
    
    - **Dado** que el Admin edita un restaurante.
        
    - **Entonces** puede definir su `Tasa de Comisión por Envío` (ej. 3.00 €) o `Comisión %`.
        
    - **Y** este valor se usará para calcular las estadísticas en [R-07] y [A-03].
        
- **[AC-A06.3: Acceder a Carta]**
    
    - **Dado** que el Admin ve un restaurante.
        
    - **Entonces** puede acceder a la "Carta" [R-00] de ese restaurante para ver/editar sus platos y tiempos de preparación (ayudando al restaurante si no es muy tecnológico).

## [A-07] Gestión de Repartidores (Registro) (¡NUEVO!)

> **Como** Admin, **Quiero** poder crear y gestionar las cuentas de los repartidores desde mi panel, **Para** darles acceso a la app [CORE-01] y poder asignarles tareas.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-A07.1: Acceso al Panel de Repartidores]**
    
    - **Dado** que el Admin está en el panel web.
        
    - **Entonces** ve una sección/pestaña "Repartidores".
        
    - **Y** ve una lista de todos los repartidores existentes.
        
- **[AC-A07.2: Acción de "Crear Repartidor"]**
    
    - **Dado** que el Admin está en la lista de repartidores.
        
    - **Cuando** pulsa el botón "Añadir Repartidor".
        
    - **Entonces** es llevado a un formulario de creación.
        
- **[AC-A07.3: Formulario de Creación (Campos)]**
    
    - **Dado** que el Admin está en el formulario.
        
    - **Entonces** debe rellenar campos para `users` y `drivers`:
        
        - `Email` (debe ser único en `users`).
            
        - `Contraseña Temporal`.
            
        - `Nombre` (`drivers.first_name`).
            
        - `Apellido` (`drivers.last_name`).
            
        - `Teléfono` (`drivers.phone_number`).
            
- **[AC-A07.4: Guardado y Creación en BBDD]**
    
    - **Dado** que el Admin rellena los campos y pulsa "Guardar".
        
    - **Cuando** lo hace.
        
    - **Entonces** el _backend_ debe:
        
        1. Validar que el `email` no existe.
            
        2. Crear la fila en `users` (con `password_hash` y `role = 'driver'`).
            
        3. Crear la fila en `drivers` (con `first_name`, `last_name`, `phone_number`), vinculándola al `user_id`.
            
    - **Y** el Admin es redirigido a la lista de repartidores con un mensaje "Repartidor creado con éxito".
        
- **[AC-A07.5: Editar Repartidor]**
    
    - **Dado** que el Admin pulsa "Editar" en un repartidor.
        
    - **Entonces** puede cambiar `Nombre`, `Apellido` y `Teléfono`.
        
- **[AC-A07.6: Desactivar Cuenta]**
    
    - **Dado** que el Admin edita un repartidor.
        
    - **Entonces** debe haber un _switch_ o botón para cambiar `users.is_active` (true/false) (para despedir o desactivar temporalmente).
        

#### ➡️ Happy Path (Flujo Ideal)

1. El Admin contrata a un nuevo repartidor, Carlos.
    
2. Va al Panel Admin -> "Repartidores" -> "Añadir Repartidor".
    
3. Rellena: `carlos.perez@reparto.es`, `ContraseñaTemporal456`, `Carlos`, `Pérez`, `611222333`.
    
4. Pulsa "Guardar".
    
5. Le da las credenciales a Carlos, que ya puede iniciar sesión en la app móvil.
    

#### 🎨 Notas de Diseño y Técnicas (Para el Desarrollador)

- **API:** `POST /admin/drivers` (para crear), `PUT /admin/drivers/{id}` (para editar).