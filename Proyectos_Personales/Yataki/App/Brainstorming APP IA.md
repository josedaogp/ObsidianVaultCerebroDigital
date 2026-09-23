
## 1 Primer prompt: 1.1: El Núcleo (El "Por Qué" y el "Vibe")

* **Output:** Una **PVU clara**, **Principios de Diseño** y **Anti-Features** que actúan como la "Constitución" de tu proyecto.
### Input 1
```
Vamos a explorar una idea de nogocio para montarla.

  

Necesito que respondas a los prompts que te dejo a continuación, teniendo en cuenta el resto del contexto:

# 🗺️ Fase 1: Definición Profunda y Alcance del MVP

El objetivo es pasar de "la idea" a un "plan de producto" accionable. El objetivo es crear el "mapa" detallado antes de poner el primer ladrillo.

Aquí el *vibe* es tu visión, y la IA actúa como un **Product Manager (PM) socrático** que te obliga a definir cada detalle.

Dividámoslo en tres sub-fases.

---

### 1.1: El Núcleo (El "Por Qué" y el "Vibe")

Aquí destilamos la esencia.

* **Input:** La idea abstracta (Ej: "una app para compartir recetas").
* **Proceso (Vibe + IA):**
    * **Vibe:** "Siento que las apps de recetas actuales son un desastre, demasiado ruido, quiero algo zen y minimalista."
    * **IA (Rol: Estratega):** Vamos a validar ese "vibe".
    * **Prompts (Inputs para la IA):**
        > "Tengo la idea de crear una plataforma de reparto de restauración tipo Uber Eats, JustEats, etc, para zonas rurales. Es decir, aquellos restaurantes-bares de pueblos donde no llegan estas plataformas, ofrecer el servicio de reparto para contentar a restaurantes y clientes. El 'vibe' es justo ese; hacer rentable el negocio al mismo tiempo que solucionamos el problema de que los clientes de zonas rurales no disponen de un servicio profesional de reparto a domicilio de restaurantes de su pueblo o pueblos cercanos. ¿Cuál es el **problema principal** que estoy resolviendo para ellos?"
        >
        > "Describe el 'dolor' o dolores (pain point) de mi 'user persona' (restaurante-cliente) de la forma más vívida posible. ¿Cómo lo están solucionando ahora (la 'alternativa') y por qué es una mala-buena solución?"
        >
        > "Basado en ese 'dolor', formula una **Propuesta de Valor Única (PVU)** en una sola frase para mi app."
        >
        > "Para mantener el 'vibe' que tiene mi app, ¿cuáles deberían ser nuestros **Principios de Diseño**? (Ej: '1. Menos clics, más acción', '2. Cero distracciones sociales', '3. La velocidad es una feature')."
        >
        > "Define 3 **'Anti-Features'** (cosas que mi app **NUNCA** tendrá) para proteger esta propuesta de valor." (Ej: "No tendrá comentarios", "No tendrá 'likes'", "No tendrá feed de actividad").
* **Output:** Una **PVU clara**, **Principios de Diseño** y **Anti-Features** que actúan como la "Constitución" de tu proyecto.
```


### Output 1
```
¡Absolutamente! Me encanta este enfoque. Asumo mi rol de Product Manager socrático.

Tu "vibe" es crucial: estás atacando un problema de **viabilidad económica** (para ti y el restaurante) y de **acceso al servicio** (para el cliente). No es solo "poner una app", es diseñar un modelo logístico que _funcione_ donde los gigantes han decidido que no merece la pena.

Vamos a destripar esta idea según tus prompts.

---

### 1.1: El Núcleo (Definido)

Aquí están las respuestas a tus preguntas, diseñadas para solidificar tu visión.

#### 🎯 El Problema Principal que Resuelves

El **problema principal** que estás resolviendo es la **ineficiencia logística y la falta de escala en mercados de baja densidad**.

Desglosemos esto:

1. **Para los Clientes:** El problema es la **exclusión de la conveniencia**. Están acostumbrados a la inmediatez digital en todo (comprar en Amazon, ver Netflix), pero se ven forzados a un modelo analógico (llamar, coger el coche) para la comida a domicilio. Están desatendidos por las grandes plataformas porque, individualmente, no representan suficientes pedidos por kilómetro cuadrado.
    
2. **Para los Restaurantes:** El problema es una **barrera de entrada logística y tecnológica inasumible**. Quieren vender más, pero no pueden permitirse contratar a un repartidor (que estaría ocioso el 90% del tiempo) ni tienen la tecnología para gestionar pedidos de forma eficiente.
    

Tu solución no es "otra app de delivery", es un **agregador logístico hiper-local** que permite a 10 restaurantes compartir el coste de 1 repartidor, haciendo que el sistema sea viable para todos.

#### 😫 El "Dolor" y la "Alternativa" Actual

Visualicemos a tus _user personas_:

**1. El Cliente (Ej: "Familia en un pueblo a 10km del núcleo urbano"):**

- **Dolor (Vívido):** "Son las 9 de la noche de un viernes. Llueve. Los niños quieren pizza del restaurante 'El Horno' del pueblo de al lado (15 min en coche). No me apetece nada coger el coche, conducir 30 minutos (ida y vuelta) y esperar allí otros 15. ¿Por qué tengo que 'planificar' una pizza como si fuera una excursión? Me siento como un ciudadano de segunda comparado con mis amigos de la ciudad, que abren una app y lo tienen en la puerta. Al final, acabo haciendo una tortilla de patatas congelada. Frustrante."
    
- **La Alternativa Actual:**
    
    - **El "Take Away" tradicional:** Coger el coche, gastar gasolina, perder una hora.
        
    - **El "reparto casero" (si existe):** Llamar al restaurante y "rogar" que te lo traigan.
        
    - **Por qué es una mala solución:** Es de **alta fricción**, inconveniente, consume tiempo y (en el caso del reparto casero) es poco fiable, lento y depende de la buena voluntad del dueño del bar.
        

**2. El Restaurante (Ej: "Dueño de asador en pueblo de 3.000 hab."):**

- **Dolor (Vívido):** "Recibo 10-15 llamadas cada fin de semana preguntando si llevamos a domicilio. Cada 'No' que digo es dinero que vuela. He intentado que mi camarero lleve pedidos en horas valle, pero es un desastre: la sala se queda sola, el TPV se atasca, el camarero se agobia y el pedido llega tarde y frío. Contratar a un repartidor dedicado es una locura, ¿qué hace de lunes a jueves? No me salen los números. Veo cómo JustEats funciona en la capital, pero aquí no vienen."
    
- **La Alternativa Actual:**
    
    - **El "reparto-apaño":** El propio dueño o un empleado lo lleva cuando puede.
        
    - **No ofrecer el servicio:** Resignarse a perder esos ingresos.
        
    - **Por qué es una mala solución:** Es **antieconómica, no escalable y poco profesional**. Quema al personal, da mala imagen y limita el crecimiento del negocio a la capacidad física del local.
        

#### 💎 Propuesta de Valor Única (PVU)

Basado en esos dolores, tu PVU debe ser clara y directa:

> **"Llevamos los mejores platos de los restaurantes de tu comarca directamente a tu casa, haciendo el _delivery_ rentable y accesible por fin en zonas rurales."**

#### 📜 Principios de Diseño (Tu "Vibe" hecho reglas)

Para mantener el "vibe" de rentabilidad y servicio local, tus principios deben ser tu brújula:

1. **La Eficiencia Logística es la ÚNICA Feature:** Todo en la app (cliente y restaurante) debe estar diseñado para minimizar el tiempo y los kilómetros del repartidor. Agrupación de pedidos, rutas optimizadas y radios de acción claros.
    
2. **Simplicidad Radical (Cero Fricción):** Tu _user persona_ (cliente y hostelero) no quiere "jugar" con una app. Quieren pedir o gestionar un pedido. La app debe ser tan fácil de usar que casi parezca invisible.
    
3. **Fiabilidad > Velocidad:** En la ciudad se compite por "20 minutos o gratis". En el ámbito rural, se compite por **"llegar"**. Es mejor prometer 60 minutos y cumplirlos, que 30 y fallar. La confianza lo es todo.
    

#### ⛔ Anti-Features (Lo que NUNCA harás)

Para proteger tu PVU y tus principios, debes decir "NO" activamente a cosas que te desenfocarán:

1. **NUNCA tendremos "Tarifa Plana" o "Suscripciones Premium":** Modelos como "Uber One" o "Glovo Prime" se basan en alta recurrencia y densidad. En tu modelo, cada pedido debe ser rentable _per se_. Introducir suscripciones nublaría tus métricas de rentabilidad por pedido.
    
2. **NUNCA tendremos Pedidos "Multi-Restaurante" (en un solo carrito):** Permitir que un cliente pida un primero de la pizzería y un segundo del asador (en pueblos distintos) es la receta para el desastre logístico. Tu modelo se basa en rutas eficientes _desde_ un punto A _hacia_ un punto B (o C y D si están cerca).
    
3. **NUNCA tendremos un Catálogo Infinito o "Tiendas de Conveniencia":** No eres Glovo. No vas a llevar un paquete de tabaco, un cargador o la compra del súper. Eres **exclusivamente un agregador de restaurantes locales**. Añadir otros verticales rompe tu eficiencia logística y tu acuerdo de valor con el hostelero.
    

---

Hemos definido tu "Constitución". Tienes un problema claro, un dolor vívido y una propuesta de valor (y anti-valor) que te diferencia.

¿Te parece que esto captura la esencia de tu idea? Si es así, podemos pasar a la siguiente fase: definir el MVP (Producto Mínimo Viable).
```




---
## 2 Segundo Prompt: El "Feature Brainstorm" (El "Qué")
### Input 2
```
Continuemos explorando la idea.

Con la Constitución clara, hacemos un brainstorming de *todo* lo que podría hacer la app, para luego poder cortar.
Glorasio: **PVU** = Propuesta de Venta Única
* **Input:** La PVU y los Principios de Diseño.
* **Proceso (Vibe + IA):**
    * **Vibe:** "Ok, para cumplir esta promesa de [PVU], el usuario necesitaría poder..."
    * **IA (Rol: Product Manager Creativo):**
    * **Prompts (Inputs para la IA):**
        > "Haz un brainstorming de 15 (o más) posibles 'features' que apoyen directamente la PVU: (PVU comentada anteriormente) y respeten los Principios: (Principios comentados anteriormente)."
        >
        > "Ahora, agrupa estas 'features' en 3-4 **'Epics'** (grandes bloques de funcionalidad). Por ejemplo: 'Gestión de Recetas', 'Planificación de Comidas', 'Perfil de Usuario', 'Login'."
* **Output:** Una lista exhaustiva de *posibles* features, agrupadas en *Epics* (temas).

```
### Output 2
```
¡Entendido! Asumo el rol de PM Creativo.

Tu **PVU** ("_Delivery rentable y accesible en zonas rurales_") y tus **Principios** (_1. Eficiencia Logística, 2. Simplicidad Radical, 3. Fiabilidad > Velocidad_) son nuestro faro.

Aquí tienes un _brainstorming_ de todas las _features_ que podríamos construir para sostener esa visión. Hemos mezclado features de las 3 apps necesarias (Cliente, Restaurante, Repartidor) porque tu negocio _es_ la interacción entre las tres.

---

### 🧠 Brainstorming de 18 Features Potenciales

1. **Validador de Código Postal/Dirección:** La primerísima pantalla. El usuario introduce su ubicación y el sistema dice "Sí, llegamos" o "No, aún no". (Principio: Fiabilidad).
    
2. **Listado de Restaurantes por "Comarca" o Radio de KM:** No mostrar restaurantes que están logísticamente fuera de alcance, aunque estén cerca "en línea recta". (Principio: Eficiencia).
    
3. **Menú Digital Hiper-Simple:** Sin fotos obligatorias, solo texto claro y precios. Fácil de navegar para el cliente, fácil de subir para el restaurante. (Principio: Simplicidad).
    
4. **Gestión de Disponibilidad de Platos (Restaurante):** Un simple _switch_ (On/Off) para que el restaurante marque "Se ha acabado el chuletón" en tiempo real. (Principio: Fiabilidad).
    
5. **Gestión de "Pausar" Pedidos (Restaurante):** Un botón rojo para que el restaurante diga "Estamos saturados, no aceptamos más pedidos durante 30 min". (Principio: Fiabilidad).
    
6. **App/Tablet de Gestión de Pedidos (Restaurante):** Un dispositivo dedicado en la cocina que recibe pedidos con un sonido claro y botones grandes de "Aceptar" / "Rechazar". (Principio: Simplicidad).
    
7. **Programación de Pedidos (Cliente):** Permitir al cliente pedir a las 17:00 para recibir la cena a las 21:00. _Esto es ORO para tu logística_, permite agrupar pedidos con antelación. (Principio: Eficiencia).
    
8. **Pasarela de Pago Integrada (Cliente):** Pago por tarjeta y Bizum. Sin efectivo (el efectivo complica la logística y la contabilidad). (Principio: Simplicidad).
    
9. **ETA (Tiempo Estimado) Realista y Dinámico (Cliente):** No prometer "30 min". Prometer "Entre 45 y 60 min" y cumplirlo. El ETA debe recalcularse si el repartidor se retrasa. (Principio: Fiabilidad).
    
10. **Seguimiento de Pedido por Estados (Cliente):** Estados muy simples: 1. Confirmado, 2. En preparación, 3. En ruta, 4. Entregado. (Principio: Fiabilidad).
    
11. **App de Repartidor (Repartidor):** La app que guía al repartidor, optimizada para cobertura rural (modo offline ligero si es posible).
    
12. **Sistema de "Batching" (Agrupación) de Pedidos (Sistema/Repartidor):** La característica _clave_. El sistema debe ser capaz de asignar al mismo repartidor dos pedidos del mismo restaurante que van a pueblos cercanos. (Principio: Eficiencia).
    
13. **Navegación GPS Optimizada (Repartidor):** Un botón que abre la ruta en Google Maps/Waze con la dirección exacta. (Principio: Eficiencia).
    
14. **Confirmación de Entrega (Repartidor):** Un simple botón de "Entregado" que cierra el ciclo.
    
15. **Ajuste de Tiempos de Preparación (Restaurante):** Al aceptar un pedido, el restaurante puede añadir "+10 min" si la cocina está ocupada. (Principio: Fiabilidad).
    
16. **Login/Registro Simple:** Email y contraseña, o login social (Google/Apple). Mínimos datos.
    
17. **Historial de Pedidos (Cliente):** Con un botón de "Repetir Pedido" para máxima simplicidad. (Principio: Simplicidad).
    
18. **Chat de Soporte Básico (Cliente <-> Admin):** Un canal para incidencias (Ej: "El repartidor no encuentra mi casa"). No un chat Cliente<->Restaurante que añade ruido.
    

---

### 🗂️ Agrupación en 4 "Epics"

Ahora, agrupemos esta lista en grandes bloques de funcionalidad (Epics).

#### Epic 1: Descubrimiento y Pedido (La App del Cliente)

Este Epic cubre todo el _flujo del cliente_ desde que abre la app hasta que paga.

- Validador de Código Postal/Dirección
    
- Listado de Restaurantes por "Comarca"
    
- Menú Digital Hiper-Simple
    
- Programación de Pedidos
    
- Pasarela de Pago Integrada
    
- Login/Registro Simple
    
- Historial de Pedidos (y "Repetir Pedido")
    

#### Epic 2: Gestión de la Cocina (La App del Restaurante)

Este Epic es la herramienta de trabajo del hostelero. Debe ser fiable y simple como un martillo.

- App/Tablet de Gestión de Pedidos
    
- Gestión de Disponibilidad de Platos (On/Off)
    
- Gestión de "Pausar" Pedidos (Tienda On/Off)
    
- Ajuste de Tiempos de Preparación
    

#### Epic 3: Logística y Entrega (La App del Repartidor + Sistema)

El "cerebro" y las "piernas" de tu operación. Aquí es donde se gana o se pierde dinero y se cumple la PVU.

- App de Repartidor
    
- Sistema de "Batching" (Agrupación) de Pedidos
    
- Navegación GPS Optimizada
    
- Confirmación de Entrega
    

#### Epic 4: Seguimiento y Soporte (La Promesa de Fiabilidad)

Este Epic agrupa las features que construyen confianza _después_ de que el cliente ha pagado.

- ETA Realista y Dinámico
    
- Seguimiento de Pedido por Estados (Confirmado, En ruta...)
    
- Chat de Soporte Básico (Cliente <-> Admin)
    

---

Hemos pasado de la "visión" a un "mapa de features" completo. Ahora tenemos un _backlog_ de todo lo que _podríamos_ construir.

El siguiente paso es el más doloroso pero el más importante: decidir qué **NO** vamos a hacer para la primera versión.

Si estás listo, podemos pasar a la Fase 1.3: **El Corte del MVP (Definiendo el "Mínimo" y el "Viable")**.
```

---

---
## 3 Tercer Prompt: La Definición Detallada (El "Cómo Funciona")
### Input 3
```
Genial. Prosigamos con el siguiente paso-parte.

Esta es la parte clave. Aquí es donde definimos *exactamente* qué hace cada cosa, antes de pensar en BBDD o pantallas.

* **Input:** La lista de *Epics* y *features* de la sub-fase anterior.
* **Proceso (Vibe + IA):**
    * **Vibe:** "Vale, la feature 'Guardar Receta' es clave. ¿Pero cómo funciona exactamente? ¿Qué campos tiene? ¿Qué pasa si falla?"
    * **IA (Rol: Analista de Negocio / PM Técnico):**
    * **Prompts (Inputs para la IA):**
        > "Toma la 'Epic' [NOMBRE_EPIC]. Desglósala en **'User Stories' (Historias de Usuario)** con el formato: 'Como [USUARIO], quiero [ACCIÓN], para [BENEFICIO]'."
        >
        > *(Este es el prompt más importante de todos)*
        > "Seleccionemos la 'User Story' más crítica: [PEGAR_USER_STORY]. Profundiza en ella. Define sus **'Criterios de Aceptación' (Acceptance Criteria)**. Lístalos como un checklist detallado de 'Dado... Cuando... Entonces...' (Given... When... Then...)."
        >
        > **Ejemplo de prompt:** "Para la User Story: 'Como usuario, quiero guardar una nueva receta para poder consultarla después'. Dame los Criterios de Aceptación."
        >
        > **Ejemplo de Output de la IA (¡Esto es oro!):**
        > ```
        > * **Dado** que el usuario está en el formulario de 'Nueva Receta',
        >     **cuando** rellena los campos (Título, Ingredientes, Pasos) y pulsa 'Guardar',
        >     **entonces** la receta debe aparecer en su 'Lista de Recetas' personal.
        > * **Dado** que el usuario intenta guardar una receta,
        >     **cuando** el campo 'Título' está vacío,
        >     **entonces** debe mostrarse un error "El título es obligatorio" y la receta no debe guardarse.
        > * **Dado** que el usuario intenta guardar una receta,
        >     **cuando** el campo 'Ingredientes' está vacío,
        >     **entonces** debe mostrarse un aviso de confirmación "¿Seguro que quieres guardar sin ingredientes?".
        > * **Dado** que el usuario guarda una receta,
        >     **cuando** pierde la conexión a internet,
        >     **entonces** la app debe guardar la receta localmente (draft) e intentar sincronizarla más tarde.
        > ```
        >
        > "Define el **'Happy Path'** (el flujo ideal sin errores) para esta 'User Story' en 5 pasos."
        >
        > "¿Cuáles son los 3 **'Edge Cases' (casos borde)** o flujos de error más comunes para esta 'User Story'?"
* **Output:** Un conjunto de **User Stories** con **Criterios de Aceptación** explícitos. Este documento es tu "biblia".
```



---

### Output 3
```
¡Perfecto! Asumo el rol de PM Técnico.

Este es un ejercicio fundamental. Para darle una lógica de negocio, voy a ordenar las _Epics_ siguiendo el "viaje del pedido" (el _order lifecycle_):

1. **Epic 2: Gestión de la Cocina** (El restaurante tiene que estar listo para recibir pedidos).
    
2. **Epic 1: Descubrimiento y Pedido** (El cliente tiene que poder hacer el pedido).
    
3. **Epic 3: Logística y Entrega** (El repartidor tiene que poder entregarlo).
    
4. **Epic 4: Seguimiento y Soporte** (El cliente debe tener visibilidad y ayuda durante el proceso).
    

Aquí tienes el desglose de las 4 _Epics_ en **Historias de Usuario (User Stories)**.

---

### Epic 2: Gestión de la Cocina (App del Restaurante)

- Como **Dueño de Restaurante**, quiero **iniciar sesión** en mi app de gestión, para **poder recibir y gestionar pedidos**.
    
- Como **Dueño de Restaurante**, quiero **recibir una alerta clara** (visual y sonora) cuando entra un nuevo pedido, para **no perderme ninguna venta**.
    
- Como **Dueño de Restaurante**, quiero **ver los detalles de un nuevo pedido** (platos, precio, datos del cliente), para **poder aceptarlo o rechazarlo**.
    
- Como **Dueño de Restaurante**, quiero **ajustar el tiempo de preparación** al aceptar un pedido (ej. "+10 min"), para **dar una estimación realista** al cliente y al repartidor.
    
- Como **Dueño de Restaurante**, quiero **marcar un plato como 'No disponible'** (on/off) en mi menú digital, para **evitar que los clientes pidan algo que se ha agotado**.
    
- Como **Dueño de Restaurante**, quiero **'Pausar' mi tienda** (dejar de recibir pedidos temporalmente) con un solo botón, para **cuando mi cocina esté saturada**.
    
- Como **Dueño de Restaurante**, quiero **marcar un pedido como 'Listo para recoger'**, para **avisar al sistema y al repartidor** de que pueden pasar a por él.
    

### Epic 1: Descubrimiento y Pedido (App del Cliente)

- Como **Cliente**, quiero **introducir mi código postal o dirección** nada más abrir la app, para **saber instantáneamente si el servicio llega a mi casa**.
    
- Como **Cliente**, quiero **ver una lista clara de los restaurantes** que reparten en mi zona, para **poder elegir dónde pedir**.
    
- Como **Cliente**, quiero **ver el menú de un restaurante** (platos, precios, descripciones simples), para **poder elegir mi comida**.
    
- Como **Cliente**, quiero **añadir y quitar platos** de mi cesta de la compra de forma sencilla, para **configurar mi pedido**.
    
- Como **Cliente**, quiero **poder programar mi pedido** para una hora específica más tarde (ej. 'recibir a las 21:00'), para **organizarme con antelación**.
    
- Como **Cliente**, quiero **pagar mi pedido de forma segura** con tarjeta o Bizum, para **finalizar la compra sin fricción**.
    
- Como **Cliente**, quiero **crear una cuenta simple** (o usar login social), para **no tener que rellenar mis datos (dirección, pago) cada vez**.
    
- Como **Cliente**, quiero **ver mi historial de pedidos** y poder **repetir un pedido anterior** con un solo clic, para **ahorrar tiempo**.
    

### Epic 3: Logística y Entrega (App del Repartidor + Sistema)

- Como **Repartidor**, quiero **iniciar sesión en mi app** y marcarme como 'Disponible' (online), para **empezar a recibir asignaciones de pedidos**.
    
- Como **Repartidor**, quiero **recibir la asignación de un pedido** (o varios agrupados) con información clara (restaurante, dirección cliente), para **saber qué tengo que hacer**.
    
- Como **Sistema**, quiero **agrupar ('batch') dos o más pedidos del mismo restaurante** que van a direcciones cercanas, para **asignarlos a un solo repartidor y optimizar la ruta** (¡Esta es clave!).
    
- Como **Repartidor**, quiero **ver mi ruta optimizada en un mapa** (1º recoger en Restaurante A, 2º entregar a Cliente B, 3º entregar a Cliente C), para **ser lo más eficiente posible**.
    
- Como **Repartidor**, quiero **abrir la dirección** (del restaurante o del cliente) en mi app de GPS (Google Maps/Waze) con un solo clic, para **navegar sin errores**.
    
- Como **Repartidor**, quiero **marcar un pedido como 'Recogido'**, para **informar al sistema y al cliente** de que ya voy de camino.
    
- Como **Repartidor**, quiero **marcar un pedido como 'Entregado'**, para **cerrar el servicio y quedar libre para el siguiente**.
    

### Epic 4: Seguimiento y Soporte (La Promesa de Fiabilidad)

- Como **Cliente**, quiero **ver el estado de mi pedido** en tiempo real (Ej: Confirmado, Preparando, En ruta, Entregado), para **saber qué está pasando y reducir mi ansiedad**.
    
- Como **Cliente**, quiero **ver un Tiempo Estimado de Entrega (ETA) realista** en la app, para **poder organizarme y saber cuándo esperar la comida**.
    
- Como **Cliente**, quiero **recibir notificaciones push** sobre los cambios de estado importantes (ej. 'Tu pedido está en camino'), para **estar informado sin tener que mirar la app**.
    
- Como **Cliente**, quiero **tener un botón de 'Ayuda'** en mi pedido, para **contactar con soporte (chat o teléfono) si hay una incidencia grave** (ej. 'pedido incorrecto' o 'no llega').
    
- Como **Admin (Soporte)**, quiero **recibir las incidencias de los clientes** en un panel central, para **poder gestionarlas y dar una solución rápida** (ej. reembolsar, contactar al repartidor).
    

---

### 🔍 Profundización: User Story Más Crítica

De todas las historias listadas, la que define tu **Propuesta de Valor Única (PVU)** de "rentabilidad logística" es la del _batching_. Si esta falla, tu modelo de negocio en zonas rurales no es viable.

Vamos a destriparla.

**User Story Seleccionada:**

> "Como **Sistema**, quiero **agrupar ('batch') dos o más pedidos del mismo restaurante** que van a direcciones cercanas, para **asignarlos a un solo repartidor y optimizar la ruta**."

#### ✅ Criterios de Aceptación (Acceptance Criteria)

- **Dado** que el `Pedido A` (del Restaurante X) ha sido aceptado por la cocina, **y** el `Pedido B` (también del Restaurante X) es aceptado en una ventana de tiempo configurable (ej. 10 minutos) respecto al `Pedido A`, **y** la dirección de entrega del `Pedido B` está dentro de un radio geográfico configurable (ej. 3 km) de la dirección del `Pedido A`, **y** el tiempo de preparación de ambos pedidos es similar, **entonces** el Sistema debe marcarlos como "Agrupables" y asignarlos juntos a un único Repartidor.
    
- **Dado** que un Repartidor recibe un "batch" (lote) de `Pedido A` y `Pedido B`, **cuando** abre su hoja de ruta, **entonces** la app debe mostrarle la secuencia optimizada (1. Recoger en Restaurante X, 2. Entregar en Dirección A, 3. Entregar en Dirección B).
    
- **Dado** que el Sistema está evaluando agrupar el `Pedido B` con el `Pedido A` (ya asignado), **cuando** la nueva ruta (`Recoger A+B`, `Entregar A`, `Entregar B`) provoca que el `Pedido A` se entregue _fuera_ de su ETA prometido, **entonces** el Sistema NO debe agruparlos y debe buscar un Repartidor diferente para el `Pedido B`. (Principio: _Fiabilidad > Eficiencia_).
    
- **Dado** que el `Pedido A` es del `Restaurante X` y el `Pedido B` es del `Restaurante Y`, **cuando** sus direcciones de entrega están cerca, **entonces** el Sistema NUNCA debe agruparlos (cumpliendo nuestra Anti-Feature de "No pedidos Multi-Restaurante").
    

#### 🚗 Happy Path (El flujo ideal)

1. **19:30:** El Cliente 1 (Ana, en C/ Mayor) pide una pizza en "Pizzería Luigi". El Sistema asigna un ETA de 20:15.
    
2. **19:33:** El Cliente 2 (Ben, en C/ Estación, a 1.5 km de Ana) pide dos lasañas en "Pizzería Luigi". El Sistema asigna un ETA de 20:20.
    
3. **19:35:** La Pizzería marca ambos pedidos como "En preparación".
    
4. **19:36:** El Sistema detecta que ambos pedidos son del mismo restaurante, van a direcciones cercanas y tienen ETAs similares. Los agrupa en un "batch".
    
5. **19:37:** El Sistema asigna el "batch" al Repartidor 1 (Carlos), que ve en su app: "Recoger 2 pedidos en Pizzería Luigi".
    
6. **19:50:** Carlos recoge ambos pedidos.
    
7. **20:10:** Carlos entrega el pedido a Ana. Marca "Entregado".
    
8. **20:18:** Carlos entrega el pedido a Ben. Marca "Entregado".
    
9. (Resultado: Un solo viaje, dos entregas, ambos clientes satisfechos, costes logísticos reducidos a la mitad).
    

#### ⚠️ Edge Cases (Casos Borde y Errores)

1. **Canibalización de ETA:** El Sistema agrupa A y B. Pero el Repartidor tiene que ir primero a A (que está más lejos) y B (que pidió antes) recibe su pedido 20 minutos tarde.
    
    - _Solución:_ El algoritmo de optimización de ruta debe tener en cuenta los ETAs prometidos como una restricción principal.
        
2. **Restaurante Lento:** El Sistema agrupa A y B. El Repartidor llega, pero el Pedido A está listo y el Pedido B tardará 15 minutos más. El Pedido A se enfría esperando, y su cliente recibe la comida fría y tarde.
    
    - _Solución:_ El "batching" solo debe confirmarse _después_ de que el restaurante marque AMBOS pedidos como "Listos para recoger", o si sus tiempos de preparación estimados coinciden.
        
3. **Agrupación Tímida:** Los parámetros del Sistema (radio de 2km, ventana de 5 min) son tan estrictos que, en la práctica, casi nunca se agrupan pedidos. El negocio no es rentable.
    
    - _Solución:_ Los parámetros de "radio" y "ventana de tiempo" deben ser variables que el Admin pueda ajustar fácilmente para encontrar el punto óptimo.
        

---

Tenemos la "biblia" completa de _qué_ hay que construir, desglosada hasta el nivel de requisito técnico. El siguiente paso lógico es usar esta biblia para decidir qué es lo _mínimo_ que necesitamos para empezar.

¿Quieres que pasemos a la fase de **"El Corte del MVP"** para definir qué se queda dentro y qué se queda fuera de la Versión 1?
```
---
## 4 Cuarto prompt: El "Corte" del MVP (El "Mínimo")
### Input 4.1
```
Continuemos.
Ahora que tenemos *todo* definido (y es gigante), usamos la tijera.

* **Input:** Todas las *User Stories* detalladas.
* **Proceso (Vibe + IA):**
    * **Vibe:** "Quiero todo esto, pero tengo que lanzar en 3 semanas. ¿Qué es lo *absolutamente* esencial para que la PVU se cumpla?"
    * **IA (Rol: Lead Developer Realista):**
    * **Prompts (Inputs para la IA):**
        > "Voy a usar el método **MoSCoW (Must-have, Should-have, Could-have, Won't-have)**. Toma esta lista de 'User Stories' [PEGAR_LISTA_DE_USER_STORIES] y clasifícalas. Sé *extremadamente* estricto con los 'Must-have'. Justifica por qué cada 'Must-have' es vital para la PVU."
        >
        > "Genera el **'Backlog de Producto del MVP v1.0'**. Debe contener *únicamente* la lista de 'User Stories' clasificadas como 'Must-have'. Este será el alcance cerrado de nuestro desarrollo."
* **Output Final (de toda la Fase 1):** Un **Backlog de MVP priorizado y detallado**, compuesto por *User Stories* con *Criterios de Aceptación* claros.
```

### Output 4.2
```
¡Entendido! Asumo el rol de **Lead Developer Realista**.

Vamos a ser claros: con un _vibe_ de "lanzar en 3 semanas", no estamos construyendo la plataforma perfecta. Estamos construyendo un **andamio funcional** para validar la hipótesis central.

La hipótesis no es "¿Quiere la gente pedir a domicilio?" (la respuesta es sí). La hipótesis es: **"¿Podemos crear un modelo logístico _rentable_ en zonas rurales agrupando pedidos?"**

Por lo tanto, nuestro MVP v1.0 debe ser un "cuchillo": solo hace una cosa, pero la hace perfectamente. Cortamos todo lo que sea "calidad de vida", "retención" o "escalado automático". Nos centramos en el "ciclo de vida del pedido".

Aquí está tu clasificación **MoSCoW** (Must-have, Should-have, Could-have, Won't-have) de todas las _user stories_ que definimos.

---

### 🟨 Clasificación MoSCoW (Versión Estricta)

#### M: Must-have (Vital para la PVU)

_Esta es la V1.0. Si falta algo de esto, el sistema no funciona o la PVU (rentabilidad) es imposible de probar._

|**Epic**|**User Story**|**Justificación (Por qué es VITAL para el MVP)**|
|---|---|---|
|**Cliente**|"Como Cliente, quiero **introducir mi código postal**... para **saber si el servicio llega**."|**Vital:** Es tu _guardabarrera_. Sin esto, aceptas pedidos en zonas donde no operas, rompiendo tu promesa de fiabilidad y tu logística.|
|**Cliente**|"Como Cliente, quiero **ver la lista de restaurantes**... para **poder elegir**."|**Vital:** Navegación básica. Si no ve restaurantes, no hay pedido.|
|**Cliente**|"Como Cliente, quiero **ver el menú**... para **elegir mi comida**."|**Vital:** Navegación básica.|
|**Cliente**|"Como Cliente, quiero **añadir/quitar platos** de mi cesta..."|**Vital:** El _core_ de la transacción.|
|**Cliente**|"Como Cliente, quiero **pagar mi pedido**... con **tarjeta**."|**Vital:** Sin pago, no hay negocio. (Nota: Dejamos Bizum para V2 para simplificar integraciones. Una forma de pago es _Must-have_).|
|**Cliente**|"Como Cliente, quiero **crear una cuenta simple**..."|**Vital:** Necesitamos su dirección y datos de contacto para la entrega. Un _guest checkout_ es más complejo.|
|**Cliente**|"Como Cliente, quiero **ver el estado de mi pedido** (Confirmado, Preparando, En ruta)..."|**Vital:** Es la base de tu principio de **Fiabilidad**. Sin esto, el cliente está a ciegas y colapsará tu (inexistente) soporte.|
|**Cliente**|"Como Cliente, quiero **ver un ETA realista**..."|**Vital:** Parte del principio de **Fiabilidad**. Gestiona las expectativas. Para V1, puede ser un ETA "estático" (ej. "45-60 min").|
|**Restaurante**|"Como Dueño, quiero **iniciar sesión**..."|**Vital:** Acceso básico a la herramienta.|
|**Restaurante**|"Como Dueño, quiero **recibir una alerta clara**... cuando entra un pedido."|**Vital:** Si el restaurante no ve el pedido, el ciclo se rompe.|
|**Restaurante**|"Como Dueño, quiero **ver y aceptar/rechazar** un pedido."|**Vital:** El _handshake_ (apretón de manos) que inicia la operación.|
|**Restaurante**|"Como Dueño, quiero **'Pausar' mi tienda**..."|**Vital:** Crítico para la **Fiabilidad**. Si la cocina se satura y no puede "cerrar", el sistema colapsará aceptando pedidos que no puede servir.|
|**Restaurante**|"Como Dueño, quiero **marcar un pedido como 'Listo para recoger'**."|**Vital:** Es la señal (el _trigger_) que avisa al repartidor. Sin esto, la logística se basa en adivinanzas.|
|**Repartidor**|"Como Repartidor, quiero **iniciar sesión** y marcarme como 'Disponible'."|**Vital:** Necesitamos saber quién está trabajando para asignarle pedidos.|
|**Repartidor**|"Como Repartidor, quiero **recibir la asignación** de uno o más pedidos..."|**Vital:** El _core_ de la app del repartidor.|
|**Repartidor**|"Como Repartidor, quiero **abrir la dirección**... en mi GPS."|**Vital:** Crítico para la **Eficiencia**. Copiar y pegar direcciones manualmente es lento y propenso a errores.|
|**Repartidor**|"Como Repartidor, quiero **marcar como 'Recogido'**."|**Vital:** Actualiza el estado para el cliente (¡Fiabilidad!).|
|**Repartidor**|"Como Repartidor, quiero **marcar como 'Entregado'**."|**Vital:** Cierra el ciclo del pedido.|
|**Logística (Admin)**|"Como **Admin/Operador**, quiero **ver todos los pedidos 'Listos'** en un panel."|**Vital:** (Esta es una _nueva_ story de MVP). En V1, el _batching_ (agrupación) NO será automático. Es demasiado complejo.|
|**Logística (Admin)**|"Como **Admin/Operador**, quiero **asignar manualmente** 1 o más pedidos a un repartidor."|**Vital:** Este es el MVP de tu PVU. El _batching_ lo hace un cerebro humano (tú) en V1, no un algoritmo. Esto es rápido de construir.|

---

#### S: Should-have (Importante, pero no vital para V1)

_Se hará en V1.1 o V2. Duele cortarlos, pero no bloquean el lanzamiento._

- "Como Dueño, quiero **ajustar el tiempo de preparación**..." (En V1, usaremos un tiempo fijo por restaurante).
    
- "Como Dueño, quiero **marcar un plato como 'No disponible'**..." (En V1, si pasa, el restaurante tendrá que llamar al cliente. Feo, pero funcional).
    
- "Como Cliente, quiero pagar con **Bizum**..." (V1 solo tarjeta para ir rápido).
    
- "Como Cliente, quiero **ver mi historial** y **repetir pedido**..." (Es retención, no adquisición. Se cae de V1).
    
- "Como Repartidor, quiero ver mi **ruta optimizada en la app**..." (En V1, el Admin que hace el _batching_ manual le dice "entrega primero a Ana, luego a Ben").
    
- "Como Cliente, quiero **notificaciones push**..." (En V1, el cliente tiene que _entrar_ a la app para ver el estado. Es _pull_, no _push_).
    
- "Como Cliente, quiero un **botón de 'Ayuda'** (chat)..." (En V1, pondremos un email o teléfono en la sección 'Ajustes'. No un sistema de tickets).
    

---

#### C: Could-have (Deseable, si sobra tiempo)

_Ideas para el futuro que no aportan al core del MVP._

- "Como Cliente, quiero **programar mi pedido**..." (Esto añade mucha complejidad logística al _batching_. En V1, solo se pide "para ahora").
    

---

#### W: Won't-have (No se hará en esta versión)

_Anti-Features o features que van contra nuestros principios o complejidad._

- **Sistema de _Batching_ Automático (Algoritmo):** (Es la V2. Es demasiado complejo para 3 semanas. Lo "falseamos" con el _Admin Panel Manual_).
    
- **Seguimiento del Repartidor en un Mapa (Live):** (Complejo y no aporta a la _Fiabilidad_ tanto como un buen ETA y estados claros).
    
- **Pedidos Multi-Restaurante:** (Lo definimos como Anti-Feature).
    

---

### ✅ Output Final: El Backlog de Producto del MVP v1.0

Este es el alcance _cerrado_ para el desarrollo. Es la lista de **Must-haves**, y nada más.

#### Epic 1: Descubrimiento y Pedido (Cliente)

- **[C-01]** Como Cliente, quiero introducir mi código postal o dirección nada más abrir la app, para saber instantáneamente si el servicio llega a mi casa.
    
- **[C-02]** Como Cliente, quiero ver una lista clara de los restaurantes que reparten en mi zona, para poder elegir dónde pedir.
    
- **[C-03]** Como Cliente, quiero ver el menú de un restaurante (platos, precios), para poder elegir mi comida.
    
- **[C-04]** Como Cliente, quiero añadir y quitar platos de mi cesta de la compra de forma sencilla, para configurar mi pedido.
    
- **[C-05]** Como Cliente, quiero crear una cuenta simple (Email, Pass, Dirección, Teléfono), para no tener que rellenar mis datos cada vez.
    
- **[C-06]** Como Cliente, quiero pagar mi pedido de forma segura con tarjeta de crédito, para finalizar la compra.
    
- **[C-07]** Como Cliente, quiero ver el estado de mi pedido en tiempo real (Confirmado, Preparando, En ruta, Entregado), para saber qué está pasando.
    
- **[C-08]** Como Cliente, quiero ver un Tiempo Estimado de Entrega (ETA) (ej. "45-60 min"), para poder organizarme.
    

#### Epic 2: Gestión de la Cocina (Restaurante)

- **[R-01]** Como Dueño de Restaurante, quiero iniciar sesión en mi app de gestión, para poder recibir y gestionar pedidos.
    
- **[R-02]** Como Dueño de Restaurante, quiero recibir una alerta clara (visual y sonora) cuando entra un nuevo pedido, para no perderme ninguna venta.
    
- **[R-03]** Como Dueño de Restaurante, quiero ver los detalles de un nuevo pedido y poder "Aceptarlo" o "Rechazarlo".
    
- **[R-04]** Como Dueño de Restaurante, quiero 'Pausar' mi tienda (dejar de recibir pedidos temporalmente) con un solo botón, para cuando mi cocina esté saturada.
    
- **[R-05]** Como Dueño de Restaurante, quiero marcar un pedido como 'Listo para recoger', para avisar al sistema y al repartidor.
    

#### Epic 3: Logística y Entrega (Repartidor + Admin)

- **[D-01]** Como Repartidor, quiero iniciar sesión en mi app y marcarme como 'Disponible' (online), para empezar a recibir asignaciones de pedidos.
    
- **[D-02]** Como Repartidor, quiero recibir la asignación de uno o más pedidos con información clara (restaurante, dirección cliente, orden de entrega si es un _batch_).
    
- **[D-03]** Como Repartidor, quiero abrir la dirección (del restaurante o del cliente) en mi app de GPS (Google Maps/Waze) con un solo clic.
    
- **[D-04]** Como Repartidor, quiero marcar un pedido como 'Recogido'.
    
- **[D-05]** Como Repartidor, quiero marcar un pedido como 'Entregado'.
    
- **[A-01]** Como Admin/Operador, quiero un panel web simple donde vea todos los pedidos 'Aceptados' y 'Listos para recoger' en un mapa o lista.
    
- **[A-02]** Como Admin/Operador, quiero poder seleccionar uno o más pedidos y asignarlos a un Repartidor 'Disponible' para crear un _batch_ manual.
    

---

Este es tu MVP. Tienes 3 aplicaciones (Cliente, Restaurante, Repartidor) y 1 panel de Admin.

El _core loop_ (Pedido -> Aceptación -> _Batching Manual_ -> Recogida -> Entrega -> Estado actualizado) está completo. Es lo mínimo absoluto para probar tu PVU.

Con este _backlog_ cerrado, ¿quieres que pasemos a la **Fase 2: Diseño de la Solución (UX/UI y Arquitectura)**?
```
### Input 4.2
```
No crees que para el MVP podría ser más interesante no incluir al cliente en la app, y centrarnos en el restaurante y el repartidor? Partimos de la base de que el restaurante ya tenía reparto a domicilio y quiere contratar nuestros servicios de reparto. Entonces seguiría recogiendo los pedidos como acostumbra (llamada telefónica, mensaje o como sea, me da igual). El restaurante metería los datos en la app, y la app se encargaría de gestionar los pedidos para agruparlos, e indicarle al repartidor la ruta optima etc para que el pedido esté en tiempo. Eso sí, el pago podrá ser en efectivo etc. Esta sería una primerísima fase que no duraría más de dos semanas en "el mercado", pero que nos ayudaría a nosotros a saber si nuestro negocio es rentable
```
### Output 4.2
```
¡Alto ahí!

Tienes toda la razón. Asumo mi rol de Lead Developer Realista al 100%.

Este es un **pivote de MVP brillante**. Es lo que en el mundillo se llama un "MVP Mago de Oz" o, más precisamente, un **MVP de "Conserje" enfocado en B2B**.

Estás re-definiendo radicalmente el MVP. Pasamos de ser un 'marketplace' tipo Uber Eats (que es un negocio B2C2B) a ser un **"Software de Logística como Servicio" (SaaS) puro para restaurantes (un negocio B2B)**.

Tu hipótesis es mucho más limpia: _¿Puedo ofrecer un servicio de reparto compartido (logística) más barato y eficiente de lo que le cuesta al restaurante hacerlo él mismo (su camarero o él mismo en coche)?_

SÍ. Esto es mucho más rápido y directo para validar tu **hipótesis de rentabilidad**. Me encanta.

---

### Analizando tu Propuesta de MVP B2B

- **Lo que GANAMOS:**
    
    - **Velocidad:** Eliminamos de un plumazo la App de Cliente. Eso es 1/3 del desarrollo y toda la complejidad de UI/UX, pasarelas de pago, marketing de captación de clientes, etc.
        
    - **Enfoque:** Nos centramos en el _único_ problema: la eficiencia logística. El _batching_ y la ruta del repartidor.
        
    - **Prueba Real:** Validamos si los restaurantes (nuestro cliente _real_) ven valor en pagar por nuestra logística.
        
- **El RIESGO / PROBLEMA:**
    
    - **Fricción Manual:** El restaurante está a tope un viernes. ¿De verdad va a tener a alguien metiendo a mano en una app la dirección que le han dado por teléfono? Es un punto de fricción altísimo.
        
    - **El Efectivo:** Has introducido una _feature_ que, lejos de simplificar, es **10 veces más compleja que la app del cliente**: el **pago en efectivo**.
        

Déjame ser brutalmente honesto como tu Lead Dev: **el pago en efectivo es una pesadilla logística y técnica.**

Pensemos en los _Criterios de Aceptación_ de esa _feature_:

- ¿El repartidor lleva cambio? ¿Cuánto?
    
- ¿El repartidor tiene que llevar un datáfono (TPV móvil) por si el cliente dice "Ah, al final pago con tarjeta"?
    
- ¿El repartidor tiene que "liquidar" el efectivo al final del turno? ¿A quién? ¿Al restaurante? ¿A nosotros?
    
- Si el repartidor recoge 100€ en efectivo (que es dinero del _restaurante_ por los platos), ¿cómo cobramos _nosotros_ nuestra comisión de logística de, digamos, 15€? ¿El restaurante nos lo paga a fin de mes? ¿El repartidor se queda 15€ y le da 85€ al restaurante?
    

El efectivo rompe la simplicidad.

### 💡 Contra-propuesta: El "MVP B2B Ultra-Ligero" (Sin Efectivo)

Mantengamos tu idea B2B, pero maticemos el pago para que sea viable en dos semanas.

**Nuestra V1.0 B2B será un "servicio de flota externalizada", no un "gestor de cobros".**

1. **El Restaurante cobra como siempre:** El restaurante gestiona el pedido por teléfono/WhatsApp. Si el cliente paga por Bizum al restaurante, perfecto. Si el cliente es de confianza y le paga al restaurante a fin de mes, perfecto. **El problema del cobro es 100% del restaurante.**
    
2. **Nuestra App es solo de "Envíos":** El restaurante no mete el "precio" en nuestra app, solo los datos del _envío_.
    
3. **App de Restaurante (Gestor de Envíos):**
    
    - Un formulario simple: "Crear Envío".
        
    - Campos: `Dirección del Cliente` (quizás con autocompletar de Google), `Teléfono del Cliente` (para que llame el repartidor), `Notas` (ej. "Casa con perro grande").
        
    - Botón: "Solicitar Recogida".
        
    - Verá estados: "Buscando repartidor", "Repartidor en camino", "Entregado".
        
4. **Nuestro Modelo de Negocio V1:** A final de la semana, le enviamos al restaurante una factura: "Has hecho 45 envíos con nosotros. A 3€/envío (por ejemplo), son 135€".
    
5. **Logística (Batching):** El _batching_ (agrupación) sigue siendo la clave. Nuestro **Panel de Admin** recibirá los "Solicitar Recogida", veremos 3 pedidos de "Pizzería Luigi", los agruparemos manualmente y se lo asignaremos a un repartidor.
    

---

### ✅ El Nuevo Backlog de MVP v1.0 (B2B SaaS)

Esto SÍ se puede construir en dos semanas. Es pura logística.

#### Epic 1: Gestión de Envíos (App del Restaurante)

- **[R-01]** Como Dueño, quiero **iniciar sesión**.
    
- **[R-02]** Como Dueño, quiero **rellenar un formulario simple** (Dirección, Teléfono, Notas) para **"Solicitar un Envío"**.
    
- **[R-03]** Como Dueño, quiero **ver el estado** de mis envíos pendientes ("Asignado", "Entregado").
    

#### Epic 2: Logística y Entrega (App del Repartidor + Admin)

- **[D-01]** Como Repartidor, quiero **iniciar sesión** y marcarme como 'Disponible'.
    
- **[D-02]** Como Repartidor, quiero **recibir asignaciones de envío** (1 o más, agrupados) con la ruta optimizada (1. Recoger en Restaurante X, 2. Entregar a Cliente Y...).
    
- **[D-03]** Como Repartidor, quiero **abrir la dirección** en mi GPS.
    
- **[D-04]** Como Repartidor, quiero marcar como **'Recogido'**.
    
- **[D-05]** Como Repartidor, quiero marcar como **'Entregado'**.
    
- **[A-01]** Como Admin (Nosotros), quiero **ver todos los "Envíos Solicitados"** en un panel.
    
- **[A-02]** Como Admin, quiero **agrupar manualmente** 1 o más envíos y **asignarlos a un Repartidor**.
    

---

Hemos eliminado el 60% de la complejidad (App de Cliente, Pagos, Menús).

Este MVP prueba la hipótesis central: **¿Somos capaces de gestionar el _batching_ y la logística de forma rentable para nosotros y valiosa para el restaurante?** Sí o no.

Estoy mucho más de acuerdo con este enfoque para una primera prueba de mercado de dos semanas.

¿Te parece bien este "MVP B2B ultra-ligero"? Si es así, podemos pasar a la **Fase 2: Diseño de la Solución** para estas 3 piezas (App Restaurante, App Repartidor, Panel Admin).
```
### Input 4.3
```
Genial. En realidad, sí que usaré el efectivo y/o el TPV del restaurante. Es como lo he hablado con los primeros restaurantes asociados. Aún así, quiero saber si existen opciones mixtas, en las que yo le ofrezca una pasarela de pago online al restaurante que ya existan. Tipo, una página donde tu metes tus datos, y alguien puede pagar ahí bajo una solicitud que haga el restaurante, o algo así. Ningún restaurante va a aceptar que el cliente pague a fin de mes ni nada de eso. Esa es tu primera tarea, responderme a esta cuestión.

  

Por otro lado, segunda tarea. Para cada US de cada épica, detalla la User Story con TODOS ABSOLUTAMENTE TODOS los detalles necesario para ser desarrollada. Por ejemplo, dices que como dueño quiero ver el estado de mis envíos, pero qué estados voy a tener? Haz especial incapié en completar todos los detalles de todas las US. Empieza con la primera US [R-01] a ver si me convence. Te iré guiando para hacer el resto.
```
### Output 4.3
```
¡Perfecto! Este es el tipo de "roce con la realidad" que define un buen MVP.

Asumo mi rol de Lead Dev. Vamos a atajar tus dos tareas.

---

### Tarea 1: El Asunto del Pago (Efectivo, TPV y Opciones Online)

Tienes toda la razón. Mi contra-propuesta anterior (facturar a fin de mes al _cliente_) era ingenua para un restaurante local. El modelo que has hablado con ellos (efectivo/TPV) es el correcto, pero tenemos que definirlo bien para que tu negocio sea viable.

**El Modelo Híbrido B2B (Cómo SÍ funciona):**

1. **El Restaurante GESTIONA el Pedido y el PAGO:** El cliente llama al restaurante. El dueño le dice: "Son 25€. ¿Cómo pagas? ¿En efectivo al repartidor, o te paso un enlace de pago ahora?".
    
2. **El Restaurante COBRA:**
    
    - **Opción A (Efectivo):** El dueño marca en _nuestra app_ "Cobro en efectivo: 25€" al crear el envío.
        
    - **Opción B (TPV/Bizum del Restaurante):** El cliente le hace un Bizum al restaurante. El dueño lo comprueba. Marca en _nuestra app_ "Ya Pagado".
        
    - **Opción C (Tu Pasarela):** Respondiendo a tu pregunta, SÍ, puedes ofrecer esto.
        
3. **El Repartidor (NUESTRO empleado) ACTÚA:**
    
    - Si el pedido está "Ya Pagado", solo entrega la comida.
        
    - Si está "Cobro en efectivo", el repartidor COBRA los 25€ (con cambio) y entrega la comida.
        

**El GRAN Problema (Tu Tarea Nº1): La Liquidación**

Al final del turno, tu repartidor tiene, digamos, 150€ en efectivo. **Este dinero NO es tuyo, es de los restaurantes.** Además, tú tienes que cobrar tu comisión de logística (ej. 3€ por envío).

Aquí es donde el 99% de los MVPs de _delivery_ fracasan.

**Mi recomendación técnica para un MVP de 2 semanas:**

- **Evita el efectivo a toda costa.** Es una pesadilla operativa, de seguridad y contable.
    
- Si _debes_ aceptarlo (porque te lo exigen), tu app debe tener un módulo de **"Liquidación"** para el repartidor. Al final del turno, el repartidor ve: "Debes 120€ a Pizzería Luigi, 30€ a Asador Pepe. Tienes 150€ en mano". Y tiene que ir físicamente a liquidar.
    

**Respondiendo a tu pregunta sobre pasarelas de pago existentes:**

Sí, la solución más simple se llama **"Payment Links"**.

- **Stripe Payment Links:** Es la mejor opción. Stripe (una pasarela de pagos) te permite crear un enlace de pago por un importe específico (ej. 25€).
    
- **Cómo funcionaría (MVP V2, no V1):**
    
    1. El dueño, en _tu app_, crea el envío y pone "Importe: 25€".
        
    2. Pulsa "Crear enlace de pago".
        
    3. Tu app, conectada a Stripe, genera un link (ej. `pay.stripe.com/abc123`).
        
    4. El dueño copia ese link y se lo manda al cliente por WhatsApp.
        
    5. El cliente abre el link, paga con su tarjeta.
        
    6. Tu app recibe la confirmación de Stripe y marca el pedido como "Pagado".
        
- **Para el MVP V1:** No integres esto. Es demasiado. En su lugar, diles a los restaurantes que usen **Bizum para Negocios** (que les da su banco) o que se creen una cuenta de **PayPal.Me**. Es su herramienta, no la tuya.
    

**Conclusión Tarea 1:** Para el MVP B2B V1, tu app debe tener **DOS modos al crear el envío:**

1. **"Ya Pagado"** (El restaurante se ha encargado. Sea por TPV, Bizum, etc.).
    
2. **"Cobro en Efectivo"** (El repartidor debe cobrar `[importe]` €).
    

Esto hace que la App de Repartidor sea más compleja, porque ahora es una cartera.

---

### Tarea 2: Desglose Detallado de User Story [R-01]

Me parece perfecto. Este es el nivel de detalle necesario. Vamos a destripar la primera User Story del **Epic 1: Gestión de Envíos (App del Restaurante)**.

#### User Story: [R-01] Login del Restaurante

> **Como** Dueño de Restaurante, **Quiero** poder iniciar sesión en la app de gestión con un email y contraseña, **Para** poder acceder a mi panel y empezar a solicitar envíos.

---

#### 📐 Criterios de Aceptación (Acceptance Criteria)

_Este es el checklist de "QUÉ" debe hacer._

- **[AC-R01.1: Campos Vacíos]**
    
    - **Dado** que el usuario está en la pantalla de "Login",
        
    - **Cuando** pulsa el botón "Entrar" y el campo "Email" o "Contraseña" está vacío,
        
    - **Entonces** el campo vacío debe mostrar un borde rojo y un mensaje de error debajo (ej. "Este campo es obligatorio") y no se debe realizar ninguna llamada a la API.
        
- **[AC-R01.2: Formato Email Inválido]**
    
    - **Dado** que el usuario está en la pantalla de "Login",
        
    - **Cuando** introduce un texto en "Email" que no tiene formato de email (ej. "pizzerialuigi" o "luigi@com") y pulsa "Entrar",
        
    - **Entonces** el campo "Email" debe mostrar un borde rojo y un mensaje de error (ej. "Introduce un email válido") y no se debe realizar ninguna llamada a la API.
        
- **[AC-R01.3: Credenciales Incorrectas]**
    
    - **Dado** que el usuario introduce credenciales (email/contraseña) que no coinciden con ninguna cuenta registrada en la base de datos,
        
    - **Cuando** pulsa "Entrar",
        
    - **Entonces** la app debe mostrar un mensaje de error global (ej. "Email o contraseña incorrectos") y el usuario debe permanecer en la pantalla de Login. (Nota: Por seguridad, nunca decimos _cuál_ de los dos es incorrecto).
        
- **[AC-R01.4: Login Exitoso]**
    
    - **Dado** que el usuario introduce un email y contraseña correctos y registrados,
        
    - **Cuando** pulsa "Entrar",
        
    - **Entonces** la app debe mostrar un indicador de carga (spinner) mientras se valida, y al confirmarse, el usuario debe ser redirigido a la pantalla principal (Dashboard/Crear Envío) y el token de sesión debe guardarse de forma segura en el dispositivo.
        
- **[AC-R01.5: Ver/Ocultar Contraseña]**
    
    - **Dado** que el usuario está escribiendo en el campo "Contraseña",
        
    - **Cuando** pulsa el icono "ojo" (o "mostrar") dentro del campo,
        
    - **Entonces** los caracteres de la contraseña deben volverse visibles (de '•••••' a '12345'). Al pulsarlo de nuevo, deben volver a ocultarse.
        
- **[AC-R01.6: Link "He olvidado mi contraseña" (Versión MVP)]**
    
    - **Dado** que el usuario está en la pantalla de "Login",
        
    - **Cuando** pulsa el enlace/botón de "He olvidado mi contraseña",
        
    - **Entonces** debe aparecer un pop-up o alerta simple con el mensaje: "Para recuperar tu contraseña, por favor, contacta con soporte en [TU_EMAIL_DE_SOPORTE] o en el [TU_TELEFONO_DE_SOPORTE]." (Nota: Un flujo automático de reseteo de contraseña está FUERA del alcance de un MVP de 2 semanas).
        
- **[AC-R01.7: Persistencia de Sesión]**
    
    - **Dado** que un usuario ha iniciado sesión exitosamente y ha cerrado la app (no "cerrado sesión", solo cerrado la app),
        
    - **Cuando** vuelve a abrir la app,
        
    - **Entonces** la app debe llevarle directamente al Dashboard (pantalla principal), sin pasar por el Login. (Debe haber un botón de "Cerrar Sesión" en otra parte de la app, como en "Ajustes").
        

---

#### ➡️ Happy Path (Flujo Ideal)

1. El Dueño abre la "App Restaurante".
    
2. Ve la pantalla de Login con: Logo, campo "Email", campo "Contraseña", botón "Entrar", y link "He olvidado mi contraseña".
    
3. Escribe `luigi@pizzeria.com` en "Email".
    
4. Escribe `SuperSecreto123` en "Contraseña".
    
5. Pulsa "Entrar".
    
6. La app muestra un _spinner_ de carga por 0.5 segundos.
    
7. La app le redirige a la pantalla "[R-02] Crear Envío". Su sesión está activa.
    

---

#### ⚠️ Edge Cases y Errores

1. **Sin Conexión:** El usuario pulsa "Entrar". La app intenta conectar con el servidor, pero no hay internet (WiFi/Datos caídos). La app debe mostrar un error claro: "No hay conexión a internet. Por favor, comprueba tu conexión."
    
2. **Cuenta Desactivada:** El usuario introduce credenciales _correctas_, pero su cuenta ha sido desactivada en el Panel de Admin (por impago de facturas, por ejemplo). La app debe mostrar un error específico: "Tu cuenta ha sido desactivada. Contacta con soporte."
    
3. **Toques Múltiples:** El usuario, impaciente, pulsa el botón "Entrar" 5 veces seguidas. La app debe ser capaz de gestionar esto y realizar _una sola_ llamada a la API, no 5.
    

---

#### 🎨 Notas de Diseño y Técnicas

- **UI:** Minimalista. Logo arriba, dos campos centrados, botón principal destacado.
    
- **Autenticación:** Backend debe usar JWT (JSON Web Tokens).
    
- **Almacenamiento (App):** El token JWT debe guardarse en el almacenamiento seguro del dispositivo (Keychain para iOS, Keystore para Android). NO en `localStorage`.
    
- **Seguridad:** La contraseña debe _hashearse_ en el backend (ej. `bcrypt`). Nunca guardar contraseñas en texto plano.
    

---

Este es el nivel de detalle que transforma una idea en una tarea de desarrollo.

Si estás de acuerdo con este nivel de desglose, **dime y continúo con la User Story [R-02]: "Como Dueño, quiero rellenar un formulario simple para 'Solicitar un Envío'"**.
```