#  Gradio Cheatsheet: Interfaces Web para tus Modelos de ML 🚀

## 🌟 **Conceptos Fundamentales**

*   **Gradio:** Una biblioteca de Python de código abierto que te permite crear *interfaces web (UI)* para tus modelos de *machine learning* (o cualquier función de Python) con *muy pocas líneas de código*.  No necesitas saber HTML, CSS ni JavaScript.
*   **Interfaz (Interface):** La clase principal de Gradio.  Define la entrada, la salida y la función que se va a ejecutar.
*   **Componentes (Components):**  Widgets de entrada y salida (inputs y outputs) que se usan para interactuar con la función.  Ejemplos: `Textbox`, `Image`, `Audio`, `Dataframe`, `Label`, etc.
*   **Función (fn):** La función de Python que quieres envolver con una interfaz web.  Puede ser un modelo de ML, una función de procesamiento de imágenes, una función de generación de texto, etc.
*   **Entradas (inputs):**  Lista de componentes de entrada.  Define cómo el usuario proporcionará los datos a la función.
*   **Salidas (outputs):**  Lista de componentes de salida.  Define cómo se mostrarán los resultados de la función.
* **Live mode**: La interfaz se actualiza en tiempo real a medida que el usuario interactúa con ella.
* **Queueing**: Gradio puede encolar las peticiones si hay muchos usuarios simultáneos.

## 💻 **Instalación**

```bash
pip install gradio
```

## 🚀 **Ejemplo Básico**

```python
import gradio as gr

def saludar(nombre):
  return "¡Hola, " + nombre + "!"

iface = gr.Interface(fn=saludar, inputs="text", outputs="text")
iface.launch()
```

*   `gr.Interface()`:  Crea una interfaz de Gradio.
    *   `fn`:  La función que se va a ejecutar.
    *   `inputs`:  El tipo de entrada (en este caso, texto). Puede ser un string o un componente.
    *   `outputs`:  El tipo de salida (en este caso, texto). Puede ser un string o un componente.
*   `iface.launch()`:  Inicia la interfaz web.  Se abrirá una pestaña en tu navegador.
*  El parámetro `share=True` permite compartir la app con un link público.

## 🖼️ **Componentes de Entrada (Inputs)**

*   **`gr.Textbox` (o `"text"`):**  Campo de entrada de texto.
    *   `lines`:  Número de líneas (si es mayor que 1, se convierte en un `textarea`).
    *   `placeholder`:  Texto de marcador de posición.
    *   `label`:  Etiqueta del componente.
    *   `type`:  Tipo de valor a devolver (`"text"` por defecto, o `"filepath"` para obtener la ruta al archivo temporal).
    * `interactive`: Si se puede editar.
*   **`gr.Number` (o `"number"`):**  Campo de entrada numérica.
    * `label`:
    * `value`: Valor inicial.
    * `precision`: Número de decimales.
*   **`gr.Slider`:**  Control deslizante (slider).
    *   `minimum`, `maximum`, `step`:  Rango y paso.
    *   `label`:
    *   `value`:  Valor inicial.
*   **`gr.Checkbox` (o `"checkbox"`):**  Casilla de verificación.  Devuelve `True` o `False`.
*   **`gr.CheckboxGroup`:**  Grupo de casillas de verificación.  Devuelve una lista con los valores seleccionados.
    *   `choices`:  Lista de opciones.
*   **`gr.Radio`:**  Botones de opción.  Devuelve el valor seleccionado.
    *   `choices`:  Lista de opciones.
*   **`gr.Dropdown`:**  Menú desplegable.
    *   `choices`:  Lista de opciones.
*   **`gr.Image` (o `"image"`):**  Para subir o capturar imágenes.
    *   `shape`:  (ancho, alto) de la imagen redimensionada (si se especifica).
    *   `type`:  Cómo se devuelve la imagen: `"numpy"` (array NumPy), `"pil"` (objeto PIL), `"filepath"`.
    *  `image_mode`: `"RGB"` (por defecto), `"L"` (escala de grises), etc.
    * `source`: `"upload"`(por defecto), `"webcam"`, `"canvas"`.
*   **`gr.Audio` (o `"audio"`):**  Para grabar o subir audio.
    *   `source`:  `"upload"` (por defecto) o `"microphone"`.
    *   `type`:  `"numpy"` (array NumPy, con la frecuencia de muestreo y los datos), `"filepath"`.
*   **`gr.File` (o `"file"`):**  Para subir archivos.
    *   `file_count`: `"single"` (por defecto), `"multiple"`.
    *   `type`:  `"file"` (objeto `tempfile._TemporaryFileWrapper`), `"binary"` (bytes), `"filepath"`.
*   **`gr.Dataframe` (o `"dataframe"`):**  Para mostrar y editar dataframes.
    *   `headers`:  Lista de nombres de columna.
    *   `type`:  `"pandas"` (por defecto) o `"numpy"`.
    *  `row_count`: Número de filas visibles.
    *  `col_count`: Número de columnas visibles.
*   **`gr.Timeseries`:**  Para datos de series temporales (ej: audio, video).
*   **`gr.ColorPicker`:**  Selector de color.
* **`gr.State`:** Variable oculta para almacenar información entre ejecuciones de la función (similar a `st.session_state` en Streamlit).
* **`gr.Variable`**: Como `State`, pero no se muestra en la UI.

## 🖨️ **Componentes de Salida (Outputs)**

*   **`gr.Label` (o `"label"`):**  Muestra una etiqueta (generalmente para clasificación).  Devuelve un diccionario con las probabilidades de cada clase.
    *   `num_top_classes`:  Número de clases a mostrar.
*   **`gr.Textbox` (o `"text"`):**  Muestra texto.
*   **`gr.Number` (o `"number"`):** Muestra un número.
*   **`gr.Image` (o `"image"`):**  Muestra una imagen.
*   **`gr.Audio` (o `"audio"`):**  Reproduce audio.
*   **`gr.Video`:**  Reproduce video.
*   **`gr.Dataframe` (o `"dataframe"`):**  Muestra un dataframe.
*   **`gr.File` (o `"file"`):** Muestra un enlace para descargar un archivo.
*   **`gr.HTML`:**  Muestra HTML.
*   **`gr.JSON` (o `"json"`):**  Muestra datos en formato JSON.
*   **`gr.Plot` (o `"plot"`):** Muestra gráficos de Matplotlib o Plotly.
*  `gr.Markdown`: Muestra texto en formato Markdown.
* `gr.Gallery`: Muestra una galería de imágenes.
* `gr.Model3D`: Muestra un modelo 3D.
* `gr.HighlightText`: Muestra texto con partes resaltadas.
*  `gr.Chatbot`: Muestra un chatbot.

## 🖼️ **Ejemplos Combinados**

```python
import gradio as gr
import numpy as np

def sepia_filter(input_img):
  sepia_filter = np.array([
      [0.393, 0.769, 0.189],
      [0.349, 0.686, 0.168],
      [0.272, 0.534, 0.131]
  ])
  sepia_img = input_img.dot(sepia_filter.T)
  sepia_img /= np.max(sepia_img)
  return sepia_img

iface = gr.Interface(
    fn=sepia_filter,
    inputs=gr.Image(shape=(200, 200)), # Componente de imagen
    outputs="image",
    examples=[
        ["images/cheetah1.jpg"], #Ejemplos para la app
        ["images/cheetah2.jpg"]
    ]
)
iface.launch()
```

## ✨ **Características Adicionales**

*   **`examples`:**  Proporciona ejemplos para la interfaz (aparecen debajo de los inputs).  Es una lista de listas, donde cada sublista contiene los valores de entrada para un ejemplo.
*   **`title`:**  Título de la interfaz.
*   **`description`:**  Descripción de la interfaz.
*   **`article`**: Texto adicional (Markdown) que aparece debajo de la interfaz.
* **`theme`**: Tema visual de la interfaz.
*   **`allow_flagging`:** Permite a los usuarios "flaggear" (marcar) ejemplos problemáticos o interesantes.  Los datos flaggeados se guardan en un archivo CSV.
*  `flagging_options`: Opciones para el flag.
*   **`live`:**  Habilita el modo "live" (la interfaz se actualiza automáticamente a medida que el usuario interactúa con los inputs).
*   **`interpretation`:**  Agrega una función de interpretación para explicar las predicciones del modelo.
* **`enable_queue`**: Habilita una cola para manejar las peticiones.
*  `cache_examples`: Cachea los ejemplos.
* **`batch`**:  Procesa múltiples entradas a la vez.  La función `fn` debe aceptar una lista de entradas y devolver una lista de salidas.
*  `max_batch_size`: Tamaño máximo del batch.
* **`scroll_to_output`**: Desplaza automáticamente la vista hasta la salida.
*  **`show_error`**: Muestra los errores.
* **`preprocess`**:  Preprocesa la entrada antes de pasarla a la función `fn`.
*  **`postprocess`**: Post-procesa la salida de la función `fn` antes de mostrarla.
*  **`layout`**: `"horizontal"`, `"vertical"` o `"unaligned"` para controlar la disposición de los inputs y outputs.

## 🎨 **Layouts (Diseño)**

*   **`gr.Row()`:**  Crea una fila.  Los componentes dentro de una fila se muestran horizontalmente.
*   **`gr.Column()`:**  Crea una columna.  Los componentes dentro de una columna se muestran verticalmente.
*   **`gr.Tab()`:**  Crea pestañas.
    ```python
        with gr.Blocks() as demo: # Blocks permite un control más preciso del layout.
            with gr.Tab("Pestaña 1"):
                with gr.Row():
                    text1 = gr.Textbox(label="Input 1")
                    btn1 = gr.Button("Procesar 1")
                out1 = gr.Textbox(label="Output 1")

            with gr.Tab("Pestaña 2"):
                with gr.Column():
                    text2 = gr.Textbox(label="Input 2")
                    btn2 = gr.Button("Procesar 2")
                out2 = gr.Textbox(label="Output 2")

        demo.launch()

    ```
* **`gr.Blocks()`**: Permite crear layouts más complejos y personalizados.  Es una forma más flexible de organizar los componentes que `Interface`.  Con `Blocks`, tienes control total sobre el layout y puedes definir event handlers para cada componente individualmente.

## 🔗 **Eventos (Events)**

* Con `Blocks`, puedes definir event handlers para cada componente individualmente.
* `componente.change(fn, inputs, outputs)`: Se dispara cuando el valor del componente cambia.
* `componente.click(fn, inputs, outputs)`: Se dispara cuando se hace clic en el componente.
* `componente.select(fn, inputs, outputs)`: Se dispara al seleccionar.
* Y otros eventos: `blur`, `focus`, `edit`, `submit`, `upload`, etc.

## ⚙️ **Configuración**

*   **`launch()` options:**
    *   `share`:  Crea un enlace público para compartir tu interfaz (temporal, dura 72 horas).
    *   `server_name`:  Dirección IP o nombre de host del servidor.
    *   `server_port`:  Puerto del servidor.
    *   `auth`:  Autenticación básica (usuario/contraseña).
    *   `inbrowser`:  Abre la interfaz en el navegador automáticamente.
    * `inline`: Muestra la app inline (en un notebook).
    * `debug`: Modo debug.
    *  `show_api`: Muestra la documentación de la API.

## ➕ **Temas Avanzados**

*   **Custom Components:** Puedes crear tus propios componentes personalizados usando HTML, CSS y JavaScript.
*   **Integración con FastAPI:** Puedes integrar interfaces de Gradio en aplicaciones FastAPI.
*   **Despliegue (Deployment):**
    *   Hugging Face Spaces (ideal para modelos de ML).
    *   Servicios en la nube (AWS, Google Cloud, Azure, etc.).
    *   Docker.
*  **Gradio Client**: Interactúa con una app Gradio remotamente.
* **Iterative Outputs**: Genera la salida de forma iterativa, ideal para chatbots o procesos largos.
* **Progress Updates**: Muestra el progreso de una tarea.
