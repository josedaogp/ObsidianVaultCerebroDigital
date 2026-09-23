Vamos a empezar por el principio absoluto de la herramienta más potente que existe ahora mismo para generación de imágenes y vídeo local (y por tanto, sin censura).

Aquí tienes la **Masterclass teórica sobre ComfyUI**.

---

## 1. ¿Qué es ComfyUI?

Imagina que quieres montar un ordenador.

- **Automatic1111** (la otra interfaz famosa) es como comprar el ordenador ya montado: tienes botones fijos, una carcasa cerrada y tocas lo que te dejan tocar.
    
- **ComfyUI** es tener todas las piezas sueltas sobre la mesa y **cables**. Tú decides qué conectas con qué.
    

**Definición técnica:** Es una interfaz gráfica de usuario (GUI) basada en **nodos** para ejecutar Stable Diffusion. En lugar de pestañas y menús, tienes un lienzo infinito donde conectas "cajas" (nodos) mediante "cables" para diseñar el flujo exacto por el que pasan los datos.

### ¿Por qué es el estándar actual?

1. **Velocidad:** Es mucho más rápido que otras interfaces.
    
2. **Gestión de memoria:** Funciona en tarjetas gráficas con menos potencia (VRAM) porque gestiona los recursos de forma inteligente.
    
3. **Control Total:** Te permite hacer cosas complejas (como mezclar dos modelos, usar 3 ControlNets a la vez, o hacer flujos de vídeo) que en otras interfaces son imposibles.
    

---

## 2. Anatomía de ComfyUI: Lo que verás en pantalla

Cuando abres ComfyUI, ves un fondo gris (el lienzo) y varias "cajas" conectadas. Vamos a diseccionar esas partes.

### A. Los Nodos (Las Cajas)

Cada nodo es una estación de trabajo que hace una tarea específica. Todo nodo tiene tres partes:

1. **Inputs (Izquierda):** Los datos que entran (puntos de conexión).
    
2. **Parámetros (Centro):** Los ajustes manuales (números, selectores, texto).
    
3. **Outputs (Derecha):** El resultado que sale hacia el siguiente nodo.
    

### B. Los Cables (Links)

Los cables transportan la información. Tienen colores específicos para que no te equivoques (no puedes conectar una "Imagen" en una entrada de "Modelo"):

- **Morado:** Datos del Modelo (Checkpoint).
    
- **Azul:** Códigos (CLIP/Texto).
    
- **Rosa:** Espacio Latente (la "masa" bruta de la imagen antes de ser píxeles).
    
- **Amarillo:** Imagen (píxeles visibles).
    

---

## 3. Los Nodos Imprescindibles (El flujo básico)

Para generar **una sola imagen**, necesitas conectar obligatoriamente estos 6 nodos. Apréndelos porque son el alfabeto de ComfyUI:

### 1. Load Checkpoint (El Cerebro)

Aquí cargas el **Modelo Principal**. Este nodo contiene todo el conocimiento visual.

- _Salidas:_ MODEL (reglas visuales), CLIP (entendimiento de texto), VAE (traductor de colores).
    

### 2. CLIP Text Encode (Las Instrucciones)

Necesitas dos de estos.

- Uno para el **Prompt Positivo** (lo que quieres ver).
    
- Uno para el **Prompt Negativo** (lo que NO quieres ver).
    
- Se conectan al `Load Checkpoint` para entender el idioma del modelo.
    

### 3. Empty Latent Image (El Lienzo)

Aquí defines la resolución (ej: 1024x1024).

- Se llama "Latent" porque la IA no pinta con píxeles de colores al principio, sino con "ruido matemático". Este nodo crea ese ruido vacío del tamaño que tú digas.
    

### 4. KSampler (El Artista)

Este es el nodo más importante. Es el que "dibuja".

- **Seed:** La semilla aleatoria (el DNI de la imagen).
    
- **Steps:** Cuántas veces repasará el dibujo (normalmente 20-30).
    
- **CFG (Classifier Free Guidance):** Cuánto caso te hace al prompt (normalmente entre 3.0 y 7.0). Si lo pones muy alto, la imagen se "quema".
    
- **Sampler/Scheduler:** El algoritmo matemático de dibujo (ej: `euler`, `dpmpp_2m`).
    

### 5. VAE Decode (El Traductor)

El KSampler genera una imagen "Latente" (ruido incomprensible para el ojo humano). El **VAE** toma ese ruido y lo traduce a una imagen PNG visible (Píxeles).

### 6. Save Image (El Archivo)

Simplemente guarda el resultado final en tu disco duro y te lo muestra en pantalla.

---

## 4. Los "Modelos": El corazón del sistema

En ComfyUI vas a oír hablar de muchos tipos de archivos. Debes distinguir estos cuatro para tu nicho (NSFW/Realismo):

### 1. Checkpoints (.safetensors)

Son los modelos base. Pesan entre 2GB y 20GB.

- **SD 1.5:** Tecnología antigua, pero con muchísimos recursos. Bueno para pornografía estilizada, malo para realismo extremo 4K.
    
- **SDXL:** El estándar de alta calidad hasta hace poco.
    
- **Pony Diffusion (Basado en SDXL):** **IMPORTANTE PARA TI.** Es el modelo rey actualmente para NSFW y Hentai/Semi-realismo. Entiende anatomía compleja y poses sexuales mejor que ningún otro.
    
- **Flux.1 (Dev/Schnell):** Lo más nuevo (agosto 2024). Calidad fotorrealista extrema. Es el que mencionamos para el paso 1 de tu proyecto. Requiere más máquina.
    

### 2. LoRAs (Low-Rank Adaptation)

Son archivos pequeños (100MB - 300MB) que se "inyectan" en el modelo principal para enseñarle algo específico que no sabe.

- _Ejemplos:_ La cara de una actriz famosa, una postura sexual concreta (ej: "doggystyle"), un estilo de iluminación, o ropa específica.
    
- _Uso:_ Se pone un nodo "Load LoRA" entre el Checkpoint y los Prompts.
    

### 3. VAE (Variational AutoEncoder)

A veces viene dentro del checkpoint, a veces va aparte. Es el responsable de que los colores no se vean "lavados" o grisáceos.

### 4. ControlNet

Es un sistema para "obligar" a la IA a seguir una estructura.

- Si quieres que la modelo tenga **exactamente** la misma postura que una foto de referencia, usas ControlNet (con modelos como _OpenPose_ o _Canny_).
    

---

## 5. El "Arma Secreta": ComfyUI Manager

ComfyUI "pelado" viene con los nodos básicos. Pero la comunidad crea **Custom Nodes** (nodos personalizados) cada día. Para no volverte loco instalando código de Python manualmente, existe el **ComfyUI Manager**. Es un menú que instalas una vez y te permite:

- Buscar e instalar nuevos nodos.
    
- **"Install Missing Custom Nodes":** Si te descargas el flujo de trabajo (workflow) de otra persona y ves nodos en rojo (error), este botón busca lo que te falta y lo instala solo.
    

---

## Resumen para tu proyecto

Como formador, mi recomendación para tu **Paso 1 (Generación de base)** usando ComfyUI es esta:

1. Necesitaremos un **Workflow de Flux.1** (para el realismo máximo).
    
2. Usaremos un nodo **LoRA** entrenado con la cara de tu modelo.
    
3. Usaremos el **KSampler** para generar la imagen.
    

**¿Te ha quedado clara la estructura teórica de ComfyUI? ¿Quieres que pasemos a ver qué hardware necesitas para mover esto, o prefieres que te explique cómo se instala?**

---
Vamos a diseccionar la herramienta. Esto es **La Biblia de Términos y Parámetros de ComfyUI**.

---

## PARTE 1: Glosario de Conceptos y sus Relaciones (El "Por qué")

En ComfyUI, todo es una transformación de datos. Entender estos términos es entender la física de este universo.

### 1. Latent Space (Espacio Latente)

- **Definición:** Es el concepto más importante. La IA no "pinta" con colores (píxeles). Trabaja con una representación matemática comprimida de la imagen. Imagina que es el "negativo digital" o la "masa cruda" antes de hornearla.
    
- **Relación:**
    
    - **Empty Latent:** Crea un lienzo de ruido vacío en este formato.
        
    - **VAE:** Es el encargado de convertir esto en algo que tus ojos pueden ver.
        
    - **Dato Clave:** Una imagen latente es mucho más pequeña que una real. Una imagen de 512x512 píxeles, en espacio latente es de 64x64 (factor de compresión x8). Por eso es tan rápido.
        

### 2. VAE (Variational AutoEncoder)

- **Definición:** Es el traductor/decodificador.
    
- **Función:** Convierte el _Latent Space_ (matemáticas) en _Píxeles_ (PNG/JPG).
    
- **Relación:** Si usas un VAE incorrecto, la imagen saldrá con colores "lavados" (grisáceos) o saturados de más (efecto "frito").
    
- **Tip:** Muchos modelos (Checkpoints) ya traen el VAE "Baked in" (integrado), pero para máxima calidad a veces se carga uno externo (como el `vae-ft-mse-840000`).
    

### 3. CLIP (Contrastive Language-Image Pre-training)

- **Definición:** El ojo que lee. Es la parte de la IA que entiende el texto.
    
- **Función:** Traduce tus palabras ("una mujer hermosa") a vectores matemáticos que el modelo puede usar para guiar el ruido latente.
    
- **Relación:** Se conecta directamente al nodo de _Prompt Positivo_ y _Negativo_.
    

### 4. Noise (Ruido)

- **Definición:** Al principio, la imagen es solo estática de televisión (ruido aleatorio).
    
- **Proceso:** Generar una imagen es, literalmente, el proceso de **quitar ruido** (Denoising) de forma ordenada hasta que emerge una figura clara.
    

### 5. Weights (Pesos)

- **Definición:** La "fuerza" o importancia que le das a algo.
    
- **Uso:** Puedes dar peso a palabras `(ojos azules:1.2)` o peso a un LoRA (0.8 de fuerza).
    

---

## PARTE 2: Nodos Clave y sus Parámetros (El "Cómo")

Aquí es donde vas a tocar botones. Vamos nodo por nodo con sus ajustes críticos.

### 1. Nodo: `Empty Latent Image` (El Lienzo)

Define el tamaño y la cantidad inicial.

- **Width / Height (Ancho/Alto):**
    
    - _Para qué sirve:_ Define la resolución base.
        
    - _Rangos Habituales:_
        
        - **SD 1.5:** 512x512, 512x768 (Vertical), 768x512 (Horizontal). **Peligro:** Si pones 1024x1024 aquí con SD 1.5, saldrán deformidades (dos cabezas).
            
        - **SDXL / Pony:** 1024x1024 (Estándar), 832x1216 (Vertical).
            
        - **Flux.1:** Funciona bien en casi cualquier resolución, pero 1024x1024 es el estándar de calidad.
            
- **Batch Size:**
    
    - _Para qué sirve:_ Cuántas imágenes generar de golpe.
        
    - _Rango:_ 1 a 4. (Más de 4 puede bloquear tu VRAM si no tienes una gráfica potente).
        

### 2. Nodo: `CLIP Text Encode` (El Prompt)

Aquí escribes lo que quieres.

- **Sintaxis de Peso (Parentesis):**
    
    - `(palabra:1.1)` = Aumenta la importancia un 10%.
        
    - `(palabra:0.9)` = Reduce la importancia un 10%.
        
    - `((palabra))` = Atajo para subir peso (equivale a 1.21 aprox).
        

### 3. Nodo: `KSampler` (El Motor Principal)

Este es el panel de control más complejo e importante.

#### A. Seed (Semilla)

- **Para qué sirve:** Es el número que define el patrón de ruido inicial.
    
- **Ajustes:**
    
    - `Control_after_generate`:
        
        - **Randomize:** Cambia el número en cada generación (para obtener imágenes nuevas cada vez).
            
        - **Fixed:** Mantiene el número (vital si te gusta una imagen y solo quieres cambiar un pequeño detalle del prompt).
            

#### B. Steps (Pasos)

- **Para qué sirve:** Cuántas veces la IA "refina" la imagen quitando ruido.
    
- **Rangos Habituales:**
    
    - **1 - 10:** Borroso, inacabado.
        
    - **20 - 30 (Estándar de Oro):** Suficiente para el 90% de los modelos (SD1.5, SDXL, Pony).
        
    - **30 - 50:** Ligera mejora en texturas finas (pelo, piel), pero tarda más.
        
    - **+50:** Rendimientos decrecientes. Apenas notarás mejora y perderás tiempo.
        
    - _Excepción:_ Modelos **Turbo** o **Flux-Schnell** usan solo 4-8 pasos.
        

#### C. CFG Scale (Classifier Free Guidance)

- **Para qué sirve:** La "obediencia". Cuánto se fuerza a la IA a seguir tu prompt vs. su propia creatividad.
    
- **Rangos Habituales:**
    
    - **1.0 - 2.0:** La IA ignora casi todo tu prompt. Muy creativo pero caótico.
        
    - **3.0 - 4.0:** Usado para **Flux.1** y modelos muy realistas.
        
    - **7.0 - 8.0 (Estándar):** El valor por defecto de SD 1.5 y SDXL. Buen equilibrio.
        
    - **+12.0:** La imagen se "quema" (colores saturados, contraste altísimo, artefactos extraños).
        

#### D. Sampler Name (El Algoritmo)

- **Para qué sirve:** La fórmula matemática que usa para quitar el ruido. Cada uno da un "sabor" distinto a la textura.
    
- **Los "Top Tier":**
    
    - `euler_a` (Ancestral): Rápido. Añade ruido mientras quita ruido. _Efecto:_ Si subes los pasos, la imagen cambia totalmente. Da pieles suaves.
        
    - `dpmpp_2m`: Muy estable.
        
    - `dpmpp_2m_sde`: Lento pero con muchísimo detalle. Favorito para realismo.
        
    - `euler` (Normal): El estándar de Flux.
        

#### E. Scheduler (El Programador)

- **Para qué sirve:** Controla _cómo_ se reduce el ruido en cada paso (si quita mucho al principio y poco al final, o viceversa).
    
- **Tipos:**
    
    - `normal`: Lineal.
        
    - `karras`: **El favorito para realismo.** Mejora los detalles finos al final del proceso.
        
    - `sgm_uniform`: Usado a veces por SDXL/Flux.
        

#### F. Denoise (Fuerza de Denoising)

- **Para qué sirve:** Cuánto de la imagen original se permite cambiar.
    
- **Rangos:**
    
    - **1.00:** Creación pura (desde ruido vacío). Todo es inventado.
        
    - **0.50 - 0.70:** Creatividad alta sobre una imagen base (cambia la cara pero mantiene la postura).
        
    - **0.10 - 0.35:** Ajustes finos (Hires Fix). Mantiene la imagen igual pero añade nitidez o cambia texturas leves.
        

### 4. Nodo: `Load Checkpoint`

Simplemente cargas el archivo `.safetensors`. No tiene parámetros complejos, pero es el origen de todo.

### 5. Nodo: `Load LoRA`

Se coloca _en medio_ del camino entre el Checkpoint y el Prompt.

- **Strength_model (Fuerza):**
    
    - **1.0:** Fuerza completa. A veces es demasiado y "fríe" la imagen.
        
    - **0.6 - 0.8:** Rango dulce (Sweet spot) para la mayoría de LoRAs de personajes (para que se parezca pero sea flexible).
        

---

## Resumen Estratégico para tu Formación

Si vas a generar **Realismo NSFW con Flux o Pony**, memoriza esta configuración inicial en tu **KSampler**:

1. **Modelo:** Pony Diffusion V6 (o Flux.1 Dev).
    
2. **Steps:** 25-30.
    
3. **CFG:** 7.0 (Pony) / 3.5 (Flux).
    
4. **Sampler:** `dpmpp_2m` o `euler`.
    
5. **Scheduler:** `karras` (Casi obligatorio para piel realista).
    
6. **Resolution (Empty Latent):** 1024x1024.
    

Esto es el cimiento técnico. Con esto claro, ya no estás adivinando, estás diseñando.

**¿Te sientes cómodo con estos términos? ¿Quieres que te muestre un ejemplo de cómo se conectan estos nodos específicamente para el modelo FLUX (que es el que usaremos para máxima calidad)?**