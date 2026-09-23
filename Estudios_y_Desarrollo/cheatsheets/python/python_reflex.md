#  Reflex (Pynecone) Cheatsheet 🚀🐍

## 🌟 **Conceptos Básicos**

*   **Reflex (antes Pynecone):** Un framework full-stack para Python que te permite crear aplicaciones web *sin escribir JavaScript*.  Compila tu código Python a código frontend (React).
*   **Componentes:**  Bloques de construcción de la UI.  Reflex tiene muchos componentes pre-construidos (botones, inputs, tablas, etc.), y puedes crear los tuyos propios.  Son similares a los *widgets* en Flutter.
*   **Estado (State):**  Datos que pueden cambiar y afectar a la UI.  Reflex usa una clase `State` para gestionar el estado de la aplicación.  Los cambios en el estado provocan una actualización de la UI.
*   **Eventos (Event Handlers):**  Funciones que se ejecutan en respuesta a acciones del usuario (clics, escritura en inputs, etc.).  Se definen como métodos en la clase `State`.
*   **Routing:**  Define cómo se navega entre las diferentes páginas de la aplicación.
* **Frontend y backend**: El frontend se define con los componentes, el backend con las funciones que modifican el estado.

## 💻 **Instalación y Configuración**

1.  **Instalar Reflex:**

    ```bash
    pip install reflex
    ```
    Asegúrate de tener Python 3.7+ y Node.js 12+ instalados.

2.  **Crear un nuevo proyecto:**

    ```bash
    mkdir mi_app
    cd mi_app
    reflex init
    ```

3.  **Ejecutar la aplicación:**

    ```bash
    reflex run
    ```
    Esto iniciará la aplicación en modo de desarrollo (con hot-reloading).

## 🗂️ **Estructura de un Proyecto**

```
mi_app/
├── mi_app/           # Carpeta con el código de la app
│   ├── __init__.py
│   └── mi_app.py     # Archivo principal de la app (define el estado y las vistas)
├── assets/          # Carpeta para archivos estáticos (imágenes, CSS, etc.)
├── pcconfig.py      # Archivo de configuración de Reflex
└── .web             # Generated files.
```

## ⚛️ **Componentes (Componentes Comunes)**

Reflex tiene muchos componentes pre-construidos.  Aquí tienes algunos de los más comunes:

*   **Contenedores:**
    *   `rx.box`:  Contenedor genérico.
    *   `rx.center`: Centra su contenido.
    *   `rx.hstack` / `rx.vstack`:  Apila componentes horizontal/verticalmente (similar a `Row` y `Column` en Flutter).
        *   `spacing`:  Controla el espacio entre los elementos.
    *   `rx.grid`:  Crea un diseño de cuadrícula.
        * `template_columns`: Define las columnas.  Ej: `"repeat(3, 1fr)"`
        * `template_rows`: Define las filas.
    *   `rx.container`: Contenedor con un ancho máximo predefinido.
*   **Texto:**
    *   `rx.text`:  Muestra texto.
        * `as_`:  Define la etiqueta HTML (ej: "p", "span", "h1").
        *   `font_size`, `font_weight`, `color`, `text_align`, etc.
    *   `rx.heading`:  Encabezado (h1, h2, h3, etc.).
    *   `rx.span`:  Similar a `Text`, pero para usar dentro de otros componentes de texto.
    *   `rx.markdown`:  Renderiza texto en formato Markdown.
*   **Formularios:**
    *   `rx.input`:  Campo de entrada de texto.
        *   `on_change`:  Evento que se dispara cuando cambia el valor.
        *   `placeholder`:  Texto de marcador de posición.
        *   `type_`:  Tipo de input (ej: "text", "email", "password", "number").
        *  `default_value`: Valor inicial.
    *   `rx.button`:  Botón.
        *   `on_click`:  Evento que se dispara al hacer clic.
        *   `color_scheme`:  Esquema de color (ej: "blue", "red", "green").
    *   `rx.checkbox`:  Casilla de verificación.
        *   `on_change`:  Evento que se dispara cuando cambia el estado (marcado/desmarcado).
    *   `rx.number_input`: Input para números.
    *   `rx.select`:  Menú desplegable.
    *   `rx.slider`:  Control deslizante.
    *   `rx.textarea`:  Área de texto (multilínea).
    *   `rx.form`:  Contenedor para un formulario.  `on_submit` se dispara cuando se envía el formulario.
*   **Imágenes:**
    *   `rx.image`:  Muestra una imagen.
        *   `src`:  Ruta a la imagen (local o URL).
        *   `width`, `height`, `alt`.
*   **Enlaces:**
    *   `rx.link`:  Crea un enlace (hipervínculo).
        *   `href`:  URL de destino.
        *   `is_external`:  Si se debe abrir en una nueva pestaña (`True`/`False`).
*   **Listas:**
    *   `rx.ordered_list` / `rx.unordered_list`:  Listas ordenadas (numeradas) / no ordenadas (con viñetas).
    *   `rx.list_item`:  Elemento de una lista.
*   **Tablas:**
    *   `rx.table`:  Crea una tabla.
        * `headers`: Lista de los encabezados de columna.
        * `rows`: Lista de filas, donde cada fila es una lista de celdas.
        *  `variant`: `"simple"`, `"striped"`, `"unstyled"`
    * `rx.table_container`: Envuelve un `rx.table` para hacerlo responsive.
*   **Otros:**
    *   `rx.divider`:  Línea horizontal de separación.
    *   `rx.spinner`:  Indicador de carga (spinner).
    * `rx.alert`: Muestra un mensaje de alerta.
        *  `status`: `"info"`, `"success"`, `"warning"`, `"error"`
        * `title`:
        * `description`:
    *  `rx.badge`: Muestra una etiqueta.
    *  `rx.icon`: Muestra un icono (de las bibliotecas de iconos soportadas, ej: `tag="chevron_down"`).
    *   `rx.modal`:  Muestra una ventana modal (diálogo).
        *   `is_open`:  Controla si el modal está abierto (`True`/`False`).
        *   `on_close`:  Evento que se dispara al cerrar el modal.
        *   `rx.modal_overlay`, `rx.modal_content`, `rx.modal_header`, `rx.modal_body`, `rx.modal_footer`.
    *   `rx.tabs`:  Crea pestañas (tabs).
        *   `rx.tab_list`, `rx.tab`, `rx.tab_panels`, `rx.tab_panel`.
    *   `rx.fragment`: Agrupa componentes sin añadir un nodo extra al DOM.

## 💾 **Estado (State)**

*   Define una clase que hereda de `rx.State`.
*   Las variables de clase (con tipo anotado) se convierten en variables de estado.
*   Los métodos de la clase se convierten en *event handlers*.
*   Usa `self.<variable>` para acceder a las variables de estado dentro de los métodos.

```python
class State(rx.State):
    count: int = 0  # Variable de estado
    text: str = "Hola"
    show: bool = False

    def increment(self):  # Event handler
        self.count += 1

    def change_text(self, new_text: str):
        self.text = new_text

    def toggle_show(self):
        self.show = not self.show

```

## 🖱️ **Eventos (Event Handlers)**
*  `on_click`: Se dispara al hacer click.
*   `on_change`:  Se dispara cuando cambia el valor de un input, checkbox, select, etc.
*   `on_blur`: Se dispara cuando un elemento pierde el foco.
*   `on_focus`: Se dispara cuando un elemento gana el foco.
*   `on_mount`: Se ejecuta cuando un componente se monta (similar a `initState` en Flutter).
*   `on_unmount`: Se ejecuta cuando un componente se desmonta.
*   `on_key_down`, `on_key_up`:  Se disparan al presionar/soltar una tecla.
*  `on_submit`: Se dispara cuando se envía un formulario.
*  Pasar argumentos: Los event handlers pueden recibir argumentos.  Por ejemplo, `on_change` de un `rx.input` recibe el nuevo valor del input.

```python
# En la clase State:
def handle_click(self):
    print("Botón clickeado")

def handle_input_change(self, new_value: str):
    self.text = new_value

# En la vista:
rx.button("Haz clic", on_click=State.handle_click)
rx.input(placeholder="Escribe algo", on_change=State.handle_input_change)
```

## 🧭 **Routing (Navegación)**

*   Cada función que define una vista (página) se convierte en una ruta.
*   El nombre de la función se convierte en el nombre de la ruta (por defecto).
*   Usa el decorador `@rx.page` para personalizar la ruta.

```python
import reflex as rx

class State(rx.State):
    pass

def index() -> rx.Component:
    return rx.text("Página principal")

@rx.page(route="/acerca", title="Acerca de") #ruta personalizada
def about() -> rx.Component:
    return rx.text("Acerca de nosotros")

app = rx.App(state=State)
app.add_page(index)
app.add_page(about) # No es necesario si ya usaste el decorador @rx.page
app.compile()
```

*  **Navegación programática**:
    *   `return rx.redirect("/otra_ruta")`
    *   `return rx.redirect("https://www.google.com", external=True)`
    * `return rx.set_focus("<id_del_elemento>")`
    *  En un event handler: `self.redirect('/ruta')`

## 🎨 **Estilos (Styling)**

*   **Estilos inline:**  Usa el argumento `style` en los componentes.  `style` recibe un diccionario con propiedades CSS.

    ```python
    rx.text("Texto rojo", style={"color": "red", "font_size": "20px"})
    ```

*   **`StyleDict`:**  Define estilos en un diccionario para reutilizarlos.

    ```python
    my_style = {
        "color": "blue",
        "font_weight": "bold",
        "padding": "10px",
    }

    rx.text("Texto con estilo", style=my_style)
    ```

*   **`color_mode`:**  Reflex tiene soporte para modo claro/oscuro.
    *   `rx.use_color_mode()`:  Hook para obtener y cambiar el modo de color actual.
    *   `rx.color_mode_cond(light_value, dark_value)`:  Devuelve un valor diferente dependiendo del modo de color.

*   **CSS personalizado:**
    *   Crea un archivo CSS en la carpeta `assets`.
    *   Importa el archivo CSS en tu aplicación (en `pcconfig.py` o usando `rx.App(style=...)`).
    * Define clases en el archivo CSS
    * Usa la propiedad `class_name`

* **`rx.style`**: Define estilos globales

## 🔗 **Enlazar Componentes y Estado (Binding)**

*   Usa `rx.State.<variable>` para enlazar el valor de un componente al estado.
*   Si quieres una propiedad computada, puedes usar una *computed property*.  Se definen como propiedades (con `@property`) en la clase `State`.

```python
class State(rx.State):
    text: str = "Hola"

    @rx.var # o @property, si no quieres que se vea como una variable de estado.
    def upper_text(self) -> str:
        return self.text.upper()
#En la vista:
rx.text(State.upper_text) #Texto en mayúsculas
```

## 🌐 **Despliegue (Deployment)**

*   **`reflex export`:**  Exporta la aplicación para despliegue.  Crea archivos estáticos (HTML, CSS, JavaScript) y código Python para el backend.
*   Puedes desplegar la aplicación en cualquier servicio que soporte aplicaciones web estáticas y Python (Vercel, Netlify, AWS, etc.).
*  Reflex ofrece su propio servicio de hosting (de pago).

## ➕ **Características Avanzadas**

*   **Componentes personalizados:**  Puedes crear tus propios componentes extendiendo `rx.Component`.
*   **API Routes:**  Crea APIs RESTful en Python (usando FastAPI). Se definen como funciones dentro de la clase State y con el decorador `@rx.api`.
*  **Base de datos**: Reflex soporta bases de datos (SQLAlchemy).
*  **Autenticación**: Reflex tiene soporte para autenticación de usuarios.
*  **Tareas en segundo plano**: Ejecuta tareas de larga duración sin bloquear la UI. `yield`
* **Middleware**: Ejecuta código antes/después de las peticiones.
*  **Uso de Javascript**: Puedes usar librerías de Javascript.

## 💡 **Ejemplo Completo**

```python
import reflex as rx

class State(rx.State):
    count: int = 0

    def increment(self):
        self.count += 1

    def decrement(self):
        self.count -= 1

def index() -> rx.Component:
    return rx.center(
        rx.vstack(
            rx.heading(f"Contador: {State.count}"),
            rx.hstack(
                rx.button("Incrementar", on_click=State.increment, color_scheme="green"),
                rx.button("Decrementar", on_click=State.decrement, color_scheme="red"),
            ),
            spacing="2em",
        ),
        padding="2em",
        height="100vh", #Ocupa toda la altura
    )

app = rx.App(state=State)
app.add_page(index)
app.compile()