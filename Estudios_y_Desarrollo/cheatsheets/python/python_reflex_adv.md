#  Reflex (Pynecone) Avanzado: Dominando el Framework Full-Stack 🚀🐍🔥

## 🗄️ **Estado Avanzado (Advanced State Management)**

*   **Subestados (Substates):** Organiza el estado de tu aplicación en subestados anidados para mejorar la modularidad y el mantenimiento.  Define clases que hereden de `rx.State` y úsalas como tipos de variables de estado dentro de tu estado principal.

    ```python
    class UserState(rx.State):
        name: str = "Usuario"
        email: str = ""

    class AppState(rx.State):
        user: UserState = UserState()  # Subestado
        count: int = 0

        def update_name(self, new_name: str):
            self.user.name = new_name
    ```
    Accede a las variables del subestado con `self.user.name`.

*   **Variables computadas (Computed Properties):** Ya las vimos en el básico, pero son fundamentales. Recuerda usar `@rx.var` (o `@property`) para definir propiedades que se calculan dinámicamente a partir de otras variables de estado.  ¡Son *esenciales* para la eficiencia! Evitan recalcular valores innecesariamente.

*   **Persistencia del estado:**
    *   **Cookies:** Reflex guarda automáticamente el estado en cookies del navegador.  El estado se restaura cuando el usuario vuelve a la página.
    *   **Almacenamiento local (Local Storage):** Puedes usar el almacenamiento local del navegador para persistir datos que no quieres enviar al servidor.
    *   **Bases de datos:** Integra tu aplicación con bases de datos (SQLAlchemy, etc.) para persistencia a largo plazo.

* **`BaseVar`**: La clase base para todas las variables de estado. Puedes crear tus propios tipos de variables de estado extendiendo `BaseVar`.

*   **Estado compartido entre clientes:** Por defecto, cada cliente (navegador) tiene su propia instancia del estado.  Para compartir estado entre todos los clientes, usa una base de datos o un mecanismo de comunicación en tiempo real (websockets).

## 🌐 **API Routes (Backend Avanzado)**

*   **Creación de APIs RESTful:** Define funciones dentro de tu clase `State` y decóralas con `@rx.api`.  Estas funciones se convierten en endpoints de tu API.

    ```python
    class State(rx.State):
        items: list[str] = []

        @rx.api
        def add_item(self, item: str):
            self.items.append(item)
            return {"message": "Item añadido"}

        @rx.api
        def get_items(self):
            return {"items": self.items}

    ```
    Puedes acceder a estos endpoints con `fetch` (en JavaScript) o con `requests` (en Python) o desde el propio frontend de Reflex, con `self.api`.

*   **Parámetros de ruta y query:**  Define parámetros en la URL y recíbelos como argumentos en tu función API.

    ```python
    @rx.api
    def get_item(self, item_id: int):  # Parámetro de ruta
        # ...

    @rx.api
    def search_items(self, query: str): # Parámetro de query (ej: /search_items?query=palabra)
      #...
    ```

*   **Métodos HTTP:**  Por defecto, las funciones API responden a peticiones GET.  Puedes usar otros métodos (POST, PUT, DELETE, etc.) especificando el método en el decorador: `@rx.api(method="POST")`

*   **Autenticación:** Protege tus endpoints de API con autenticación. Reflex proporciona utilidades para la autenticación.

*   **Validación de datos:** Usa Pydantic para validar los datos de entrada en tus endpoints de API.

## ⚛️ **Componentes Personalizados (Custom Components)**

*   **Creación de componentes:** Extiende la clase `rx.Component`.

    ```python
    class MyCustomComponent(rx.Component):
        library = "my-library"  # Nombre de la biblioteca JavaScript (si la usas)
        tag = "MyCustomComponent"  # Nombre del tag del componente (en JavaScript)
        is_default = True # Si es un componente por defecto (exportado como default)

        # Propiedades del componente (se pasan como atributos en Python)
        prop1: str
        prop2: int = 0  # Valor por defecto

        # Puedes definir alias para las propiedades:
        _prop_aliases = {"mi_propiedad": "prop1"} # Alias de "mi_propiedad" a "prop1"

      # Puedes definir eventos personalizados:
        _event_triggers = {
            "on_my_event": rx.event.EventHandlerChain,  # Define un evento "on_my_event"
        }

    # En tu vista:
    my_custom_component = MyCustomComponent.create(prop1="Valor", prop2=10, on_my_event=State.handle_my_event)

    ```

*   **Usar bibliotecas de React:**  Puedes envolver componentes de React existentes en componentes de Reflex.  Define el `library` y el `tag` de tu componente.  Importa el componente en el archivo `pcconfig.py`.

*   **Componentes controlados vs. no controlados:**
    *   **Controlados:** El estado del componente se gestiona *completamente* en el lado de Python (Reflex).  Usa `_setters` para actualizar propiedades en respuesta a eventos del frontend.
    *   **No controlados:** El estado del componente se gestiona principalmente en el lado del frontend (React).  Usa `_getters` para obtener valores del componente.
    * La mayoría de las veces, usarás componentes controlados.

* **`_render` method**: Puedes sobreescribir el método `_render` para personalizar completamente la renderización del componente (poco común, normalmente con los decoradores es suficiente)

## 🔗 **Eventos Avanzados (Advanced Event Handling)**

*   **`EventChain`:** Encadena varios event handlers.  Se ejecutan en orden.
    * `return [event1, event2, event3]` en un event handler.
*   **`background`:** Ejecuta un event handler en segundo plano (sin bloquear la UI). Ideal para tareas largas.

    ```python
        def long_task(self):
          # yield para notificar que se inicia
          yield
          # Tarea que tarda mucho...
          time.sleep(5)
          self.result = "Tarea completada"
          # yield para notificar que se finaliza.
    ```

* **`debounce` y `throttle`**:  Controla la frecuencia con la que se ejecutan los event handlers.
    *  `debounce`:  Espera un cierto tiempo después del último evento antes de ejecutar el handler.  Útil para inputs de texto (para no ejecutar el handler cada vez que se presiona una tecla).
    *   `throttle`:  Ejecuta el handler como máximo una vez cada cierto tiempo.

*   **Eventos personalizados:** Define tus propios eventos en componentes personalizados (usando `_event_triggers`).
* `cond`: Crea condicionales.

## 🎨 **Estilos Avanzados (Advanced Styling)**

*   **Temas (Themes):**  Personaliza completamente la apariencia de tu aplicación con temas. Define variables de CSS, estilos de componentes, etc.
*   **Estilos condicionales:**  Aplica estilos diferentes según el estado de la aplicación o el modo de color.
* **Animaciones**: Reflex soporta animaciones de Chakra UI.
* **CSS-in-JS**: Puedes usar bibliotecas de CSS-in-JS (como Emotion) con Reflex.

## 🌐 **Despliegue Avanzado (Advanced Deployment)**

*   **Optimización del rendimiento:**
    *   Usa `rx.memoize` para memorizar funciones y evitar cálculos repetidos.
    *   Usa `rx.lazy` para cargar componentes de forma diferida (lazy loading).
*   **Escalabilidad:**  Despliega tu aplicación en múltiples servidores para manejar más tráfico.
* **Docker**: Conteneriza tu app para un despliegue consistente.

## ➕ **Otros Temas Avanzados**

*   **WebSockets:**  Comunicación bidireccional en tiempo real entre el cliente y el servidor.
*   **Integración con FastAPI:**  Usa FastAPI para crear APIs más complejas y combínalas con tu aplicación Reflex.
*   **Testing:**  Escribe pruebas unitarias y de integración para tu aplicación Reflex.
*   **Internacionalización (i18n) y Localización (l10n):**  Adapta tu aplicación a diferentes idiomas y regiones.
* **SEO (Search Engine Optimization)**: Optimiza tu aplicación para motores de búsqueda. Reflex renderiza en el servidor, lo que es bueno para SEO.
* **Seguridad**: Protege tu aplicación contra ataques comunes (XSS, CSRF, etc.).
*  **`rx.script`**:  Ejecuta código JavaScript en el cliente.

Este "cheatsheet" de Reflex avanzado cubre muchos temas importantes. Recuerda que la práctica y la exploración de la documentación oficial son claves para dominar el framework. ¡Sigue construyendo y experimentando!
