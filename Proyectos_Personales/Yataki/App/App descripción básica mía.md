## Descripción
App tipo Uber Eats o JustEat que servirá de nexo entre restaurantes, repartidores y clientes para la gestión de pedidos de restauración. El objetivo principal del MVP será poder servir a los repartidores y restaurantes para la optimizacion de rutas del repartidor, para perder el menor tiempo posible y optimizar el tiempo total de pedido.

## Actores que usarán la app
### Restaurantes
- Introducir pedido en la app (si no se hace que el cliente pida por la app, esto sería posiblemente una V2). Podemos hacer que introduzcan su carta en la app para que sea rápida la introducción de los pedidos. Se le puede asignar id automático. El pedido deberá incluir la info también de reparto del cliente y su número de teléfono.
- Marcar pedido como listo para recoger (esta información es la que recibe la app para introducirla en la optimización de tiempos de pedidos)
- Mostrar lista de pedidos entrantes (en caso de que el cliente pida por la app) o pedidos registrados encolados, en formato tablero kanban con estados. --> Hay que definir los estados de los pedidos.
- Poder pausar pedidos entrantes por falta de capacidad en cocina (en caso de que el cliente pida por la app)
- Ver estadísticas de sus repartos, ganancias, etc.
- Introducir carta de un restaurante
### Repartidores
- Ver pedidos a recoger y en qué restaurante (Fase de recogida)
- Ver la ruta óptima para entregar todos los pedidos recogidos (Fase trayecto)
- Marcar comienzo de la ruta cuando recoga todos los pedidos de un restaurante y vaya a entregarlos
- Marcar reparto finalizado cuando entregue un pedido
### Clientes (Posible V2 o V1.5)
- Hacer un pedido --> Info necesaria?
- Ver estado del pedido (ver las fases del repartidor y/o estados del restaurante del pedido) o algún flujo de estados simbólico para el cliente que coincida con uno o varios estados-fases de los otros actores.
- Hacer valoraciones del restaurante y del repartidor
- Pago de los pedidos (V2-V3)
### Administrador
- Ver estadísticas de los repartos y de cada restaurante
- Introducir datos de nuevo repartidor-restaurante.
- Introducir carta 

## Vistas principales de la app
- Dashboard Restaurante --> Cuando un restaurante inicie sesión, solo podrá ver los datos de su restaurante. Podrá introducir/modificar la carta
- Página/s repartidor --> Qué necesitaría?
- "Marketplace" donde aparezcan los restaurantes con su carta
- Dashboard administrador --> Podrá ver estadísticas, introducir datos de nuevo repartidor-restaurante, cartas, parámetros de ganancias para calcular precios finales para clientes, etc.

## A definir
- BBDD
- Single APP para todo o una APP por cada actor?
- Necesitamos ProgressiveApp?
- Estados de pedidos --> Tanto para cliente como para restaurante como para repartidores --> Un mismo flujo para todos?

## A tener en cuenta
- Lo primordial para un MVP es la optimización de rutas. Que podamos trabajar con un repartidor o dos y que sepan qué pedidos recoger, cuándo recogerlos, si esperar a que termine un pedido o no para optimizar la ruta, y qué ruta seguir en el mapa para llegar antes al destino y entregar todos los pedidos en trayecto de la forma óptima.
	- Para ello, hay que definir los inputs necesarios para este MVP, qué actor debe introducir esos inputs y cómo
	- Definir outputs claros y a qué actor debe llegar (solo repartidor casi seguro)
## Tecnologías a utilizar
- React para front
- Supabase para backend --> PostgreSQL por tanto para bbdd y login de supabase
- Flutter en fases futuras