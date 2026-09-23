# Roadmap de Desarrollo (Backlog Priorizado V1.5)

Aquí está el backlog de User Stories (US) priorizado según el orden de desarrollo lógico. El desarrollo debe seguir estas fases para construir la plataforma de forma incremental y estable.

### Fase 1: Cimientos y Creación (El primer "Write")

_El objetivo es que un restaurante pueda crear un pedido y el admin pueda verlo._

1. **[CORE-01]** --> Login por Roles (Autenticación de Admin, Restaurante, Repartidor).
    
2. **[A-06]** --> Gestión de Restaurantes (Admin) (Necesario para crear la cuenta del restaurante y su comisión).
    
3. **[R-00]** --> Gestión de Carta y Tiempos (Prerrequisito para que el restaurante pueda operar).
    
4. **[R-03/04/05]** --> Formulario de "Solicitar Envío" (El Restaurante crea el envío con estado `REGISTERED`).
    

### Fase 2: Visualización y Asignación Manual (El "Read" y el "Update" manual)

_El objetivo es que el Admin pueda ver el pedido y asignarlo manualmente a un repartidor._

5. **[A-01]** --> Dashboard de Operaciones (Admin) (El Admin ve el pedido `REGISTERED` en "Pendientes").
    
6. **[D-01]** --> Login y Disponibilidad (Repartidor) (El Repartidor se pone `ONLINE`).
    
7. **[D-07]** --> Bote Inicial (Repartidor) (Requerido por el flujo de `[D-01]`).
    
8. **[A-02]** --> Asignación Manual (Admin) (El "botón de pánico" para asignar el pedido a un repartidor `ONLINE`).
    

### Fase 3: El Viaje del Repartidor (El "Execute" manual)

_El objetivo es que el repartidor pueda completar el ciclo de vida del pedido._

9. **[D-02]** --> Recibir Asignación (Hoja de Ruta) (El Repartidor ve la tarea asignada por el Admin).
    
10. **[D-03]** --> Ver Detalles de Entrega y Cobro (El Repartidor ve la dirección y cuánto cobrar).
    
11. **[D-04]** --> Navegación GPS (El Repartidor abre Google Maps).
    
12. **[D-05]** --> Marcar Estados del Envío (El Repartidor pulsa "Recogido" y "Entregado").
    

### Fase 4: El Bucle de la Cocina (Gestión del Restaurante)

_El objetivo es que el restaurante vea el progreso y gestione la cocina._

13. **[R-02]** --> Dashboard de Envíos (Kanban) (El Restaurante ve la tarjeta moverse de "Pendientes" a "En Ruta" y "Entregados" y puede usar los botones de cocina: "Empezar", "+5min", "Listo").
    
14. **[R-06]** --> Ver Detalles de un Envío (El Restaurante puede hacer "zoom in" en un pedido).
    

### Fase 5: El Cerebro (Automatización)

_El objetivo es reemplazar la asignación manual [A-02] por el algoritmo automático._

15. **[MOTOR-01]** --> Motor de Asignación Automática (El Sistema asigna automáticamente los pedidos "En Preparación" al repartidor óptimo).
    

### Fase 6: La Red de Seguridad (Gestión de Errores)

_El objetivo es gestionar cuando las cosas van mal._

16. **[D-06]** --> Añadir Nota de Incidencia (El Repartidor reporta un problema, ej. "cliente no está").
    
17. **[A-05]** --> Buzón de Incidencias (El Admin ve las notas de [D-06] y las de liquidación).
    

### Fase 7: El Dinero (Contabilidad)

_El objetivo es asegurar que el dinero se rastrea y liquida correctamente._

18. **[A-03]** --> Panel de Liquidación (El Admin ve el resumen de caja del repartidor).
    
19. **[A-04]** --> Marcar Liquidación Manual (El Admin "cierra la caja" del repartidor).
    
20. **[R-07]** --> Ver Estadísticas y Comisiones (El Restaurante ve sus ventas y comisiones).