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
        > "Tengo la idea de [TIPO_DE_APP]. El 'vibe' es [ADJETIVO_1, ADJETIVO_2]. ¿Cuál es el **problema principal** que estoy resolviendo para [USUARIO_OBJETIVO]?"
        >
        > "Describe el 'dolor' (pain point) de mi 'user persona' [TIPO_DE_USUARIO] de la forma más vívida posible. ¿Cómo lo están solucionando ahora (la 'alternativa') y por qué es una mala solución?"
        >
        > "Basado en ese 'dolor', formula una **Propuesta de Valor Única (PVU)** en una sola frase para mi app."
        >
        > "Para mantener el 'vibe' de [ADJETIVO_CLAVE], ¿cuáles deberían ser nuestros **Principios de Diseño**? (Ej: '1. Menos clics, más acción', '2. Cero distracciones sociales', '3. La velocidad es una feature')."
        >
        > "Define 3 **'Anti-Features'** (cosas que mi app **NUNCA** tendrá) para proteger esta propuesta de valor." (Ej: "No tendrá comentarios", "No tendrá 'likes'", "No tendrá feed de actividad").
* **Output:** Una **PVU clara**, **Principios de Diseño** y **Anti-Features** que actúan como la "Constitución" de tu proyecto.

---

### 1.2: El "Feature Brainstorm" (El "Qué")

Con la Constitución clara, hacemos un brainstorming de *todo* lo que podría hacer la app, para luego poder cortar.

* **Input:** La PVU y los Principios de Diseño.
* **Proceso (Vibe + IA):**
    * **Vibe:** "Ok, para cumplir esta promesa de [PVU], el usuario necesitaría poder..."
    * **IA (Rol: Product Manager Creativo):**
    * **Prompts (Inputs para la IA):**
        > "Haz un brainstorming de 15 posibles 'features' que apoyen directamente la PVU: [PEGAR_PVU] y respeten los Principios: [PEGAR_PRINCIPIOS]."
        >
        > "Ahora, agrupa estas 15 'features' en 3-4 **'Epics'** (grandes bloques de funcionalidad). Por ejemplo: 'Gestión de Recetas', 'Planificación de Comidas', 'Perfil de Usuario'."
* **Output:** Una lista exhaustiva de *posibles* features, agrupadas en *Epics* (temas).

---

### 1.3: La Definición Detallada (El "Cómo Funciona")

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

---

### 1.4: El "Corte" del MVP (El "Mínimo")

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