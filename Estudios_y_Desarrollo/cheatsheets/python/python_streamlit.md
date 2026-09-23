#  Streamlit Cheatsheet: Crea Apps Web Interactivas en Python 🚀

## 🌟 **Conceptos Fundamentales**

*   **Streamlit:** Un framework open-source de Python que te permite crear aplicaciones web interactivas *sin necesidad de saber HTML, CSS o JavaScript*.  Ideal para data science, machine learning y prototipado rápido.
*   **Dataflow (Flujo de datos):** Streamlit se basa en un modelo de flujo de datos.  Cada vez que el usuario interactúa con un widget (botón, input, etc.), *todo el script se vuelve a ejecutar desde arriba hacia abajo*.  Streamlit gestiona automáticamente el re-renderizado de la UI.
*   **Widgets:** Elementos interactivos (botones, inputs de texto, sliders, gráficos, etc.) que permiten al usuario interactuar con la aplicación.
*   **Caching:** Streamlit tiene un sistema de caching muy potente que permite optimizar el rendimiento de la aplicación, evitando recalcular funciones costosas innecesariamente.
*   **Layout (Diseño):**  Streamlit proporciona opciones para organizar los widgets en columnas, pestañas, expanders, etc.
* **Magic commands**: Streamlit interpreta automáticamente variables y expresiones, mostrándolas en la app sin necesidad de usar `st.write()`.

## 💻 **Instalación**

```bash
pip install streamlit
```

## 🚀 **Ejecutar una Aplicación Streamlit**

```bash
streamlit run mi_app.py
```

## 📝 **Elementos Básicos (Escritura y Visualización)**

*   **`st.write()`:**  La función más básica.  Muestra texto, datos (dataframes, arrays, etc.), gráficos y más.
*   **Magic commands:** Simplemente escribe el nombre de una variable o una expresión, y Streamlit la mostrará.

    ```python
    import streamlit as st
    import pandas as pd

    st.write("Hola, mundo!")  # Texto

    df = pd.DataFrame({"col1": [1, 2, 3], "col2": [4, 5, 6]})
    st.write(df)  # DataFrame

    "Esto también se mostrará" # Magic command

    x = 10
    x # Magic command

    df #Magic command
    ```

*   **`st.title()`:**  Título de la aplicación.
*   **`st.header()`:**  Encabezado.
*   **`st.subheader()`:**  Subencabezado.
*   **`st.text()`:**  Texto plano (sin formato).
*   **`st.markdown()`:**  Texto en formato Markdown.
*   **`st.latex()`:**  Ecuaciones LaTeX.
*   **`st.caption()`**: Texto pequeño, para aclaraciones o notas al pie.
*   **`st.code()`:**  Bloque de código.
*   **`st.divider()`**:  Línea horizontal de separación.

## 👆 **Widgets de Entrada (Input)**

*   **`st.button()`:**  Botón.
    *   Devuelve `True` cuando se hace clic, `False` en caso contrario.

    ```python
    if st.button("Haz clic"):
        st.write("¡Botón clickeado!")
    ```

*   **`st.checkbox()`:**  Casilla de verificación.
    *   Devuelve `True` si está marcada, `False` si no.

    ```python
    if st.checkbox("Mostrar detalles"):
        st.write("Detalles...")
    ```

*   **`st.radio()`:**  Botones de opción (solo se puede seleccionar uno).
    *   Devuelve la opción seleccionada.

    ```python
    opcion = st.radio("Selecciona una opción:", ["A", "B", "C"])
    st.write(f"Opción seleccionada: {opcion}")
    ```

*   **`st.selectbox()`:**  Menú desplegable.
    *   Devuelve la opción seleccionada.

    ```python
    seleccion = st.selectbox("Elige un color:", ["Rojo", "Verde", "Azul"])
    ```

*   **`st.multiselect()`:**  Menú desplegable con selección múltiple.
    *   Devuelve una lista con las opciones seleccionadas.

    ```python
    opciones = st.multiselect("Elige tus frutas favoritas:", ["Manzana", "Plátano", "Naranja", "Uva"])
    ```

*   **`st.slider()`:**  Control deslizante (slider).
    *   Devuelve el valor seleccionado.

    ```python
    valor = st.slider("Elige un valor:", 0, 100, 50)  # (min, max, valor_inicial)
    ```

*   **`st.select_slider()`:**  Slider con opciones discretas.
*   **`st.text_input()`:**  Campo de entrada de texto (una línea).
    *   Devuelve el texto ingresado.

    ```python
    nombre = st.text_input("Ingresa tu nombre:")
    ```

*   **`st.text_area()`:**  Área de texto (multilínea).
    *   Devuelve el texto ingresado.
*   **`st.number_input()`:**  Campo de entrada numérica.
*   **`st.date_input()`:**  Selector de fecha.
*   **`st.time_input()`:**  Selector de hora.
*   **`st.file_uploader()`:**  Permite al usuario subir archivos.
    *   Devuelve un objeto `UploadedFile` (que se comporta como un archivo).
*   **`st.color_picker()`:**  Selector de color.

## 📊 **Visualización de Datos (Gráficos y Tablas)**

*   **`st.dataframe()`:**  Muestra un dataframe de pandas de forma interactiva (con paginación, ordenamiento, etc.).
*   **`st.table()`:**  Muestra un dataframe como una tabla estática.
*   **`st.metric()`**: Muestra una métrica con un valor destacado.
*   **`st.json()`:** Muestra un objeto JSON.
*   **Gráficos (Streamlit tiene integración con varias bibliotecas de gráficos):**
    *   **`st.line_chart()`:**  Gráfico de líneas.
    *   **`st.area_chart()`:**  Gráfico de área.
    *   **`st.bar_chart()`:**  Gráfico de barras.
    *   **`st.pyplot()`:**  Muestra un gráfico de Matplotlib.
    *   **`st.plotly_chart()`:**  Muestra un gráfico de Plotly.
    *   **`st.altair_chart()`:**  Muestra un gráfico de Altair.
    *   **`st.vega_lite_chart()`:** Muestra un gráfico de Vega-Lite.
    *   **`st.bokeh_chart()`:** Muestra un gráfico de Bokeh.
    *   **`st.pydeck_chart()`:**  Muestra un mapa con PyDeck (para visualizaciones geoespaciales).
    *   **`st.map()`:**  Muestra un mapa simple (con puntos).
    *   **`st.graphviz_chart`**: Muestra gráficos de Graphviz.

## 🖼️ **Multimedia (Imágenes, Audio, Video)**

*   **`st.image()`:**  Muestra una imagen.
    *   Puede recibir una URL, una ruta a un archivo local, un objeto `PIL.Image`, un array NumPy, etc.
*   **`st.audio()`:**  Reproduce audio.
*   **`st.video()`:**  Reproduce video.

## 📐 **Layout (Diseño)**

*   **`st.sidebar`:**  Crea una barra lateral.  Los widgets que coloques dentro de `st.sidebar` aparecerán en la barra lateral.

    ```python
    with st.sidebar:
        st.header("Configuración")
        opcion = st.radio("Elige una opción:", ["A", "B"])
    ```

*   **`st.columns()`:**  Crea columnas.  Devuelve una lista de objetos `DeltaGenerator` (que se comportan como contenedores).

    ```python
    col1, col2, col3 = st.columns(3)  # Crea tres columnas

    with col1:
        st.header("Columna 1")
        st.write("Contenido...")

    with col2:
        st.header("Columna 2")

    with col3:
      st.write("Otra columna")

    ```

*   **`st.tabs()`**: Crea pestañas.

    ```python
    tab1, tab2, tab3 = st.tabs(["Pestaña 1", "Pestaña 2", "Pestaña 3"])

    with tab1:
        st.header("Contenido de la pestaña 1")

    with tab2:
        st.write("Contenido de la pestaña 2")
    ```

*   **`st.expander()`:**  Crea un contenedor expandible/colapsable.

    ```python
    with st.expander("Ver detalles"):
        st.write("Aquí van los detalles...")
    ```
*   **`st.container()`:**  Crea un contenedor genérico (útil para agrupar elementos).

## 💾 **Estado de la Sesión (Session State)**

*   **`st.session_state`:**  Un diccionario que almacena el estado de la sesión.  Permite que los widgets conserven sus valores entre ejecuciones del script.

    ```python
    import streamlit as st

    if "contador" not in st.session_state: # Inicialización
        st.session_state.contador = 0

    if st.button("Incrementar"):
        st.session_state.contador += 1

    st.write(f"Contador: {st.session_state.contador}")
    ```
    *   Puedes acceder a los valores del `session_state` como si fuera un diccionario: `st.session_state["clave"]` o `st.session_state.clave`.
    * Es *fundamental* para crear aplicaciones más complejas.

## ⏱️ **Caching**

*   **`@st.cache_data`:**  Decora una función para cachear sus resultados.  Si la función se llama con los mismos argumentos, Streamlit recupera el resultado de la caché en lugar de volver a ejecutar la función.  Ideal para funciones que cargan datos, realizan cálculos costosos, etc.
*   **`@st.cache_resource`:**  Cachea recursos globales (conexiones a bases de datos, modelos de machine learning, etc.).
```python
    @st.cache_data  # ¡Muy importante!
    def cargar_datos(ruta):
        df = pd.read_csv(ruta)
        return df

    datos = cargar_datos("mi_archivo.csv") # La primera vez se ejecuta, las siguientes se recupera de la caché.
```

*   **`clear_cache()`:**  Borra la caché.

## 💬 **Mensajes de Estado**
*  `st.success()`: Muestra un mensaje de éxito (en verde).
*   **`st.info()`:**  Muestra un mensaje informativo (en azul).
*   **`st.warning()`:**  Muestra un mensaje de advertencia (en amarillo).
*   **`st.error()`:**  Muestra un mensaje de error (en rojo).
*   **`st.exception()`:** Muestra una excepción.

## ✨ **Otros Elementos Útiles**

*   **`st.spinner()`:**  Muestra un spinner (indicador de carga) mientras se ejecuta un bloque de código.

    ```python
    with st.spinner("Cargando datos..."):
        # Código que tarda un tiempo...
        time.sleep(5)
    ```

*   **`st.progress()`:**  Muestra una barra de progreso.
*  **`st.stop()`:**  Detiene la ejecución del script.
*   **`st.rerun()`:**  Vuelve a ejecutar el script desde el principio.
*   **`st.empty()`:** Crea un placeholder que se puede actualizar más tarde.
*   **`st.toast()`:**  Muestra un mensaje emergente (toast).
* **`st.balloons()` / `st.snow()`**: Efectos visuales (globos o nieve).

## ⚙️ **Configuración**

*   **`st.set_page_config()`:**  Configura la página (título, icono, layout, etc.).  Debe ser la *primera* llamada a Streamlit en el script.

    ```python
    import streamlit as st

    st.set_page_config(
        page_title="Mi Aplicación",
        page_icon=":rocket:",
        layout="wide",  # "centered" (por defecto) o "wide"
        initial_sidebar_state="expanded",  # "expanded" (por defecto), "collapsed" o "auto"
    )
    ```

* **Archivo `config.toml`**:  Puedes configurar Streamlit de forma global (para todas las aplicaciones) o a nivel de proyecto creando un archivo `config.toml` en la carpeta `.streamlit` (en la raíz del proyecto o en el home del usuario).

## ➕ **Temas Avanzados**

*   **Componentes personalizados:** Puedes crear tus propios componentes de Streamlit usando HTML, CSS y JavaScript (o cualquier framework web que compile a JavaScript).
*   **Integración con otras bibliotecas:** Streamlit se integra bien con muchas bibliotecas de Python (pandas, NumPy, Matplotlib, Plotly, scikit-learn, TensorFlow, PyTorch, etc.).
*  **Despliegue (Deployment)**:
    *   Streamlit Community Cloud (gratuito para apps públicas).
    *   Heroku, AWS, Google Cloud, Azure, etc.
    *   Docker.
* **Formularios**: Agrupa widgets y envíalos con un botón.
* **Callbacks**: Ejecuta funciones cuando cambia el valor de un widget.