---
tipo_paper: IA
enlace: https://huggingface.co/papers/2601.05167?utm_source=digest-papers&utm_medium=email&utm_campaign=2026-01-09
---
## Mi análisis
Se plantea la posibilidad de utilizar un SLM (Small Language Model) el 98% del tiempo para reducir costes en lugar de utilizar siempre un LLM. La idea es que el SLM sepa decidir cuándo necesita ayuda del LLM y hacer uso del recurso "call", símplemente añadiendo literalmente el token `<<call>>`. Para conseguirlo, el SLM debe ser expresamente entrenado con aprendizaje por refuerzo en casuísticas similares a las que será expuesto, para que aprenda en qué situaciones debe solicitar al LLM que continúe la respuesta que el SLM estaba dando. 
Los problemas clave son, por supuesto, el aprendizaje por refuerzo necesario previo a la implementación, y la gestión impoluta del contexto y memoria necesaria para que el LLM pueda continuar inmediatamente con la respuesta del SLM de forma adecuada.

---
## Análisis IA
### 📘 SECCIÓN 1: INFORME TÉCNICO INTERNO

_Para tu uso exclusivo. Sin "hype", solo ingeniería._

### 1. Ficha Rápida

- **Tipo de Paper:** Arquitectura de Sistemas / Optimización de Inferencia.
    
- **Problema Principal:** Los LLMs son demasiado lentos y caros para razonamiento complejo, pero los modelos pequeños (SLMs) son "tontos". Las soluciones actuales (Router/Cascading) son ineficientes porque si el modelo pequeño falla, se descarta todo su trabajo y se reenvía todo al grande.
    
- **Innovación Clave:** **RelayLLM** (Relevo). En lugar de enrutar _prompts_ enteros, el modelo pequeño actúa como un controlador activo que decide, token por token, cuándo pedir ayuda mediante un token especial `<call>`. Es una "colaboración intra-frase".
    

### 2. Análisis Técnico (La "Chicha")

- **Arquitectura de Alto Nivel (El Flujo):** Imagina un _Junior Developer_ (SLM) escribiendo código.
    
    1. **Input:** El usuario envía un prompt.
        
    2. **Proceso (SLM):** El modelo pequeño empieza a generar tokens.
        
    3. **Trigger (El Relevo):** Si el SLM detecta que no sabe continuar o la complejidad sube, genera un token especial `<call>`.
        
    4. **Intervención (LLM):** El control pasa al LLM (Teacher), que genera los siguientes tokens críticos (la parte difícil).
        
    5. **Retorno:** Una vez resuelto el bloqueo, el LLM devuelve el control al SLM para que termine la tarea (rellenar el resto).
        
    
    - _Analogía:_ Es como un sistema de `try-catch` o escalado de incidencias: el nivel 1 resuelve el 90%, y solo escala al nivel 3 (experto) cuando es estrictamente necesario.
        
- **Stack y Dependencias:**
    
    - **Frameworks:** Implementado sobre **vLLM** (crítico para la inferencia eficiente).
        
    - **Algoritmo de Entrenamiento:** Usan **GRPO** (Group Relative Policy Optimization), un método de Reinforcement Learning para enseñar al modelo pequeño _cuándo_ es humilde pedir ayuda y cuándo ser valiente.
        
    - **Modelos:** Probado con pares como Qwen2.5 (SLM) y modelos mayores.
        
- **Recursos de Cómputo:**
    
    - **Eficiencia:** Solo invoca al LLM para el **1.07%** de los tokens generados. Esto es brutal para la latencia y costes.
        
    - **Coste:** Reportan una reducción del **98.2%** en costes comparado con routers aleatorios, manteniendo el rendimiento.
        

### 3. Resultados y Realidad

- **SOTA vs. Realidad:**
    
    - Consigue una **exactitud del 49.52%** (promedio en 6 benchmarks), cerrando casi toda la brecha con el modelo grande (Teacher).
        
    - _Reality Check:_ La métrica clave no es solo la exactitud, sino la latencia percibida. Al cambiar de modelo a mitad de frase, la gestión de la caché KV (Key-Value Cache) es crítica. Si no se comparte memoria eficientemente, el "context switch" podría comerse el ahorro de tiempo.
        
- **Trade-offs (Lo malo):**
    
    - **Complejidad de Entrenamiento:** No es "plug-and-play". Requiere un pipeline de entrenamiento en dos fases (Warm-up + RL/GRPO). Tienes que entrenar al modelo pequeño para que aprenda a usar el token `<call>`.
        
    - **Infraestructura:** Necesitas tener ambos modelos cargados o un servicio de inferencia que soporte este _handover_ rápido. No sirve llamar a la API de GPT-4 con latencia de red en medio de una frase.
        

### 4. Veredicto de Implementación

- **Nivel de Dificultad:** **Alto**. Integrar esto en un pipeline MLOps existente requiere orquestar dos modelos en tiempo real y gestionar el paso de contexto.
    
- **Código:** Disponible en GitHub (`Chengsong-Huang/RelayLLM`).
    
- **Conclusión:** **Merece la pena para escala masiva.** Si tienes un producto con millones de usuarios y tus facturas de inferencia son altas, esta arquitectura es la solución correcta. Para startups pequeñas, la complejidad de ingeniería sobrepasa el ahorro.
    

---

### 📢 SECCIÓN 2: ESTRATEGIA DE CONTENIDO (LinkedIn)

_3 opciones listas para publicar. Elige la que mejor resuene con tu audiencia hoy._

### OPCIÓN A: El Enfoque Educativo ("El Despiece")

_Ideal para posicionarte como alguien que entiende los "fundamentos"._

**Titular:** Tu modelo de IA no necesita ser un genio, necesita saber pedir ayuda.

**Cuerpo:** La mayoría de las arquitecturas de IA actuales son ineficientes: o usas un modelo gigante (lento y caro) para todo, o un modelo pequeño (rápido pero limitado).

Acabo de analizar el paper de **RelayLLM** y proponen una arquitectura mucho más inteligente que me recuerda al mentoring de ingeniería:

1. **El Junior al mando (SLM):** El modelo pequeño genera la mayoría de la respuesta.
    
2. **La "Llamada" (Token `<call>`):** Cuando llega a un paso de razonamiento complejo, no alucina. Emite un token especial que pausa su generación.
    
3. **El Senior interviene (LLM):** El modelo grande entra, resuelve solo esa parte difícil, y devuelve el control.
    

Resultado: Usamos el modelo "caro" solo para el **1% de los tokens**, pero mantenemos la calidad del resultado final.

¿Estamos ante el fin de los "Routers" tradicionales que deciden todo al principio?

#IA #SoftwareArchitecture #LLM #RelayLLM #MachineLearning

**💡 Sugerencia Visual:** Usa la **Figura 1** del paper (o redibuja un diagrama de flujo simple: SLM -> `<call>` -> LLM -> SLM).

---

### OPCIÓN B: El Enfoque Crítico ("Hype vs. Ingeniería")

_Para demostrar seniority y visión de negocio._

**Titular:** Reducir costes un 98% en IA suena a magia. Aquí está la letra pequeña de ingeniería.

**Cuerpo:** El nuevo paper de **RelayLLM** promete mantener la precisión de un modelo gigante usando casi exclusivamente uno pequeño. Los números son impresionantes: solo el 1.07% de los tokens requieren al modelo "maestro".

Pero como Arquitecto de Software, veo dos retos enormes para llevar esto a producción:

1. **La Latencia del "Context Switch":** Pasar el estado de la generación de un modelo a otro en mitad de una frase no es gratis. Sin una gestión perfecta de la memoria (KV Cache), el retraso puede anular el beneficio.
    
2. **Entrenamiento Complejo:** No es solo inferencia. Requiere entrenar al modelo pequeño con Reinforcement Learning (GRPO) para que aprenda _cuándo_ es incompetente. Enseñar humildad a una IA es computacionalmente costoso.
    

Es una arquitectura fascinante para grandes players con infraestructura propia (Meta, Google), pero ¿es viable para la empresa media hoy?

¿Qué opináis: complejidad de arquitectura vs. factura de inferencia? 👇

#MLOps #AIEngineering #TechDebate #Scalability

**💡 Sugerencia Visual:** Captura de la **Tabla de Resultados** (probablemente Tabla 1 o 2) donde comparan "Teacher LLM" vs "RelayLLM".

---

### OPCIÓN C: El Snippet Rápido (TL;DR)

_Para mantener presencia un día con poco tiempo._

**Cuerpo:** Dato para arquitectos de IA: No necesitas un modelo de 70B parámetros para generar conjunciones y preposiciones.

El nuevo framework **RelayLLM** demuestra que el modelo "experto" solo es necesario para el **1%** de los tokens en tareas de razonamiento. El resto es "paja" que un modelo pequeño puede manejar perfectamente.

La eficiencia no está en hacer modelos más pequeños, está en saber cuándo usarlos. 🧠

#Efficiency #AI #TechTrends #Architecture

---

### 📺 Recurso Adicional (YouTube)

... [RelayLLM: Efficient Token-Level Reasoning](https://www.youtube.com/watch?v=zeky5tXjQwg) ...

Este vídeo es relevante porque analiza exactamente el paper **RelayLLM (Enero 2026)** que solicitaste, explicando visualmente cómo funciona el mecanismo de "relevo" (`<call>` token) y el entrenamiento con GRPO, lo cual te ayudará a entender mejor el flujo antes de explicarlo.

## Publicación LinkedIn
### Mi texto
TE RESUMO ESTE PAPER DE IA PARA QUE NO TENGAS QUE LEERLO TÚ: RelayLLM

Imagina que tienes a un junior con 3 meses de experiencia cobrando lo que un senior con toda una vida.

No es por desmerecer al junior, simplemente es que no tiene sentido.

Algo parecido pasa cuando usamos nuestros LLMs potentes, como puede ser GPT-5.2 pro, para una tarea que... sí, es compleja... pero ¿qué parte de su respuesta es realmente compleja?

Un alto porcentaje del uso del LLM se destina a generar sentido en la frase. Hablo de preposiciones, determinantes, puntos, comas, "Claro, aquí tienes la respuesta", "Has tenido una muy buena idea", etc. Tokens que de igual modo estás pagando caro.

Ese problema es justo el que se pretende solucionar en el paper "RelayLLM: Efficient Reasoning via Collaborative Decoding".

En él, sus autores proponen un método de "relevo", en el que un SLM (Small Language Model, que ellos categorizan como <2B de parámetros) lleve la voz cantante hasta que él solito se percate de que está siendo un verdadero inútil. En ese momento, llamaría a su senior, su LLM.
Entonces el LLM hace el trabajo duro, y cuando termina devuelve el relevo al SLM.

Para lograrlo, el SLM debe ser entrenado previamente mediante Aprendizaje por refuerzo para enseñar al modelo cuándo debe delegar. (Spoiler: Esto es lo "no tan bonito" si quisieramos implementarlo en producción).

So... What?
Podríamos prescindir de los routers estáticos o inteligentes, pasaríamos de redirigir prompts a alternar tokens.
Además, se podría reducir la complejidad y el coste de modo que el LLM -según dicen-, solo sea usado para generar el 1.07% de la respuesta.
Se abre la puerta a ejecutar modelos incluso en un dispositivo móvil, ya que un SLM liviano podría correr y delegar mediante una petición a un LLM cuando sea necesario.
### El retocado
**TE RESUMO ESTE PAPER DE IA PARA QUE NO TENGAS QUE LEERLO TÚ: RelayLLM** 👇

Imagina que tienes a un **Junior con 3 meses de experiencia** cobrando lo mismo que un **Senior con toda una vida**.

No es por desmerecer al Junior, simplemente es que **financieramente no tiene sentido**.

Algo parecido pasa cuando usamos nuestros LLMs más potentes (imagina un GPT-5.2 pro, a 168$/millon de tokens para el output) para una tarea que... sí, es compleja... pero ¿qué parte de su respuesta es _realmente_ compleja?

Un porcentaje altísimo de la inferencia se destina a **generar "paja" sintáctica**: preposiciones, determinantes, el típico _"Claro, aquí tienes la respuesta"_, _"En conclusión"_... Tokens que estás pagando a precio de oro y que no requieren un doctorado.

💡 Ese es el problema que pretende resolver el paper **"RelayLLM: Efficient Reasoning via Collaborative Decoding"**.

Sus autores proponen un cambio de paradigma: el **"relevo de tokens"** (de ahí RelayLLM jeje).

El concepto es brillante por su simplicidad:

1. Un **SLM** (Small Language Model, <2B parámetros) lleva la voz cantante. Es rápido y barato.
    
2. Escribe él solo hasta que se da cuenta de que es un verdadero inútil.
    
3. En ese momento, **llama a su Senior** (el LLM potente).
    
4. El Senior hace el trabajo duro, y devuelve el relevo al pequeño.
    

🛠 PEEEERO... **(El detalle técnico)** Para lograr esto, el SLM debe ser entrenado previamente mediante **Aprendizaje por Refuerzo**. No para saber más, sino para aprender _humildad_: saber cuándo debe delegar. (Esto es lo "no tan bonito" si quisieras implementarlo mañana en producción).

🚀 **So... What? ¿Por qué debería importarme?**

- **Adiós al Routing estático:** Pasamos de redirigir _prompts_ enteros a alternar _tokens_ dentro de una misma frase.
    
- **Eficiencia extrema:** Según el paper, el modelo caro solo se usa para generar el **1.07%** de la respuesta.
    
- **IA en el Edge:** Se abre la puerta a ejecutar modelos en dispositivos móviles (el SLM en tu iPhone), haciendo llamadas a la nube _solo_ para los milisegundos de razonamiento complejo.
    

👇 **Pregunta seria: ¿Cuánta viabilidad le ves a esta solución?**

PD: En comentarios te dejo el enlace al paper, por si quieres echarle un ojo.

#ArtificialIntelligence #MachineLearning #RelayLLM #CostOptimization #IngenieriaDeDatos

## Carrusel a hacer
1. Un chico majo con una pizarra como que va a explicar algo. En la pizarra debe aparecer el nombre del paper, "𝐑𝐞𝐥𝐚𝐲𝐋𝐋𝐌: 𝐄𝐟𝐟𝐢𝐜𝐢𝐞𝐧𝐭 𝐑𝐞𝐚𝐬𝐨𝐧𝐢𝐧𝐠 𝐯𝐢𝐚 𝐂𝐨𝐥𝐥𝐚𝐛𝐨𝐫𝐚𝐭𝐢𝐯𝐞 𝐃𝐞𝐜𝐨𝐝𝐢𝐧𝐠". Además debe aparecer arriba "Aprendiendo de papers: "
2. Un cañón disparando dinero a una frase andante "Claro, aquí tienes tu respuesta". En el cañón debe aparecer el texto "GPT-5.2 pro" señalando que ese es su nombre. De encabezado, debe poner "El problema a resolver:" o algo así
3. Un chico con el nombre SLM mandando (ordenando a escribir o algo así) a un hombre con el nombre LLM. En el encabezado debe poner "¡SLM al mando!"
4. El chico dubitativo con una frase casi escrita en la misma pizarra que la diapositiva 1.
5. El chico llamando al LLM y el LLM escribiendo lo que le falta en la pizarra
6. El chico siendo adiestrado para llamar al hombre.
### Prompts finales para nanobanana
**Estilo a usar en todos los prompts:**

> _Estilo:_ Ilustración 3D isométrica moderna y limpia, estilo "tech friendly". Paleta de colores coherente: Azules eléctricos y blancos para lo "Junior/Simple", y Naranjas/Dorados cálidos para lo "Senior/Complejo/Dinero". Fondos limpios de estudio. Iluminación suave. Render de alta calidad.

---

#### 🎡 Prompts para NanoBanana (Listos para copiar y pegar)

#### Slide 1: La Portada (El Gancho Académico)

- **Concepto:** Tu idea del chico y la pizarra es perfecta para la intro.
    
- **Heading:** APRENDIENDO DE PAPERS:
    

> **Prompt para NanoBanana:** Una ilustración 3D amigable de un joven ingeniero "tech" sonriente, de pie junto a una gran pizarra blanca interactiva. En la parte superior de la imagen, como un encabezado flotante, aparece el texto exacto: "APRENDIENDO DE PAPERS:". En la pizarra, escrito con tipografía clara y moderna, aparece el título del paper: "𝐑𝐞𝐥𝐚𝐲𝐋𝐋𝐌: 𝐄𝐟𝐟𝐢𝐜𝐢𝐞𝐧𝐭 𝐑𝐞𝐚𝐬𝐨𝐧𝐢𝐧𝐠 𝐯𝐢𝐚 𝐂𝐨𝐥𝐥𝐚𝐛𝐨𝐫𝐚𝐭𝐢𝐯𝐞 𝐃𝐞𝐜𝐨𝐝𝐢𝐧𝗴". El entorno es un estudio de innovación limpio. [Insertar Estilo Global aquí]. Asegúrate de que el texto sea perfectamente legible.

#### Slide 2: El Problema (El desperdicio)

- **Concepto:** El cañón disparando dinero a la frase simple. Es tu mejor metáfora visual.
    
- **Heading:** EL PROBLEMA A RESOLVER:
    

> **Prompt para NanoBanana:** En la parte superior, el encabezado de texto: "EL PROBLEMA A RESOLVER:". Debajo, una escena 3D isométrica donde un cañón futurista masivo, etiquetado claramente en su lateral con el texto "GPT-5.2 pro", está disparando un torrente de monedas de oro y billetes en lugar de balas. El dinero golpea y entierra a una pequeña y simple burbuja de texto con patas que camina, la cual contiene la frase exacta: "Claro, aquí tienes tu respuesta". Representación visual de desperdicio excesivo. [Insertar Estilo Global aquí]. El texto debe ser exacto.

#### Slide 3: La Solución (El Junior empieza)

- **Concepto:** He ajustado tu idea de "mandar" por "trabajar primero", que es más fiel al paper. El Junior trabaja, el Senior espera.
    
- **Heading:** EL "JUNIOR" (SLM) EMPIEZA EL TRABAJO
    

> **Prompt para NanoBanana:** En la parte superior, el encabezado de texto: "EL 'JUNIOR' (SLM) EMPIEZA EL TRABAJO". Una escena de oficina tech 3D. Un personaje robot pequeño y ágil, con una etiqueta brillante en el pecho que dice "SLM (Junior)", está tecleando furiosamente en una consola, generando una larga cadena de bloques de texto azules simples. Detrás de él, un personaje robot mucho más grande y poderoso, con una etiqueta que dice "LLM (Senior)", está relajado en un sofá, esperando con los brazos cruzados. [Insertar Estilo Global aquí].

#### Slide 4: El Bloqueo (La duda)

- **Concepto:** El chico dubitativo ante la frase a medias. Conecta visualmente con la slide anterior.
    
- **Heading:** EL MOMENTO DEL BLOQUEO...
    

> **Prompt para NanoBanana:** En la parte superior, el encabezado de texto: "EL MOMENTO DEL BLOQUEO...". El mismo robot pequeño "SLM (Junior)" de la imagen anterior está ahora parado frente a una pantalla holográfica. En la pantalla hay una frase a medio terminar que se detiene abruptamente: "La solución óptima para la computación cuántica es...". El robot pequeño tiene un gran signo de interrogación rojo brillante sobre su cabeza y se rasca la cabeza con gesto de confusión y pánico. [Insertar Estilo Global aquí].

#### Slide 5: El Relevo (La llamada)

- **Concepto:** El momento clave. El pequeño pide ayuda, el grande actúa.
    
- **Heading:** ...Y LA LLAMADA AL EXPERTO (RELAY)
    

> **Prompt para NanoBanana:** En la parte superior, el encabezado de texto: "...Y LA LLAMADA AL EXPERTO (RELAY)". Continuación de la escena anterior. El robot pequeño "SLM (Junior)" está presionando un gran botón de emergencia que emite una señal visual con el texto "<<call>>". Inmediatamente, el robot grande "LLM (Senior)" ha saltado de su sofá y está frente a la pantalla, completando el resto de la frase con bloques de texto dorados y complejos, mientras el pequeño observa aliviado. [Insertar Estilo Global aquí].

#### Slide 6: El "Truco" (El entrenamiento)

- **Concepto:** El adiestramiento para la humildad. Es el cierre perfecto para explicar la dificultad técnica.
    
- **Heading:** EL TRUCO: ENTRENAR LA HUMILDAD (RL)
    

> **Prompt para NanoBanana:** En la parte superior, el encabezado de texto: "EL TRUCO: ENTRENAR LA HUMILDAD (RL)". Una escena de aula o dojo futurista 3D. El robot pequeño "SLM (Junior)" está sentado en un pupitre con una diadema de entrenamiento neuronal, prestando mucha atención. Un holograma de un instructor señala una pizarra grande con una regla escrita: "REGLA Nº1: SI NO SABES, PIDE AYUDA". En el suelo hay libros con títulos como "Aprendizaje por Refuerzo". [Insertar Estilo Global aquí]. El texto debe ser legible.