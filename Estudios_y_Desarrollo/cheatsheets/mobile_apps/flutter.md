# 🐦 Cheatsheet de Flutter: Tu Guía Rápida para Desarrollo Multiplataforma 🚀

## 🧱 **Conceptos Fundamentales**

*   **Widgets:** Todo en Flutter es un widget.  Bloques de construcción de la UI.
*   **Stateful vs. Stateless Widgets:**
    *   **StatelessWidget (sin estado):**  Widgets que no cambian.  Su apariencia se basa en la configuración inicial y no se modifica.  Ej: `Text`, `Icon`, `Image`.
    *   **StatefulWidget (con estado):**  Widgets que *pueden* cambiar.  Tienen un objeto `State` asociado que almacena datos que pueden modificarse y causar una reconstrucción del widget.  Ej: `Checkbox`, `TextField`, `Slider`.
*   **Hot Reload (🔥) y Hot Restart (🔄):**  Características *esenciales* para el desarrollo rápido.
    *   **Hot Reload:**  Inyecta los cambios de código en la aplicación en ejecución *sin perder el estado actual*.  Muy rápido (generalmente menos de un segundo).
    *   **Hot Restart:**  Reinicia la aplicación, pero *mucho más rápido* que un reinicio completo (compilación desde cero).  Se pierde el estado.
*   **Context:**  El `BuildContext` es un "handle" (manejador) que te da información sobre la ubicación de un widget en el árbol de widgets.  Se usa para acceder a temas, navegar, mostrar diálogos, etc.
*   **`build()` method:**  El método más importante en un widget.  Describe la UI del widget.  Flutter llama a `build()` cada vez que necesita redibujar el widget (cuando cambia el estado o cuando el widget padre se reconstruye).
*   **`setState()`:**  Función *crucial* en un `StatefulWidget`.  Se llama para notificar a Flutter que el estado ha cambiado y que el widget debe reconstruirse.  ¡Nunca modifiques el estado directamente sin llamar a `setState()`!
*   **`MaterialApp` y `Scaffold`:**
    *   **`MaterialApp`:**  Widget de nivel superior que configura la aplicación para usar Material Design.  Define temas, rutas, título de la aplicación, etc.
    *   **`Scaffold`:**  Implementa la estructura visual básica de Material Design (app bar, body, floating action button, drawer, bottom navigation bar, etc.).  Normalmente, cada "pantalla" de tu app es un `Scaffold`.
*  **Dart:** El lenguaje de programación usado en flutter.

## 🚀 **Configuración y Estructura de un Proyecto**

*   **`pubspec.yaml`:**  Archivo de configuración del proyecto.  Define el nombre de la aplicación, la descripción, la versión, las dependencias (paquetes), los assets (imágenes, fuentes, etc.).
*   **`lib/main.dart`:**  Punto de entrada de la aplicación.  Contiene la función `main()` y normalmente el widget raíz de la aplicación.
*   **`flutter create <nombre_proyecto>`:**  Comando para crear un nuevo proyecto Flutter.
*   **`flutter run`:**  Comando para ejecutar la aplicación en un emulador/dispositivo conectado.
*   **`flutter build apk` / `flutter build ios`:**  Comandos para construir versiones de lanzamiento (release) para Android y iOS.
*  **Añadir dependencias**:
    *   `flutter pub add <nombre_paquete>`
    *   Editar manualmente el `pubspec.yaml`

## 🎨 **Widgets de Diseño (Layout)**

*   **`Container`:**  Widget muy versátil para aplicar padding, márgenes, bordes, color de fondo, etc.  Puede contener un solo widget hijo.
*   **`Row`:**  Coloca sus widgets hijos en una línea horizontal.
*   **`Column`:**  Coloca sus widgets hijos en una línea vertical.
*   **`Expanded`:**  Expande un widget hijo de `Row`, `Column` o `Flex` para que ocupe el espacio disponible.  Usa la propiedad `flex` para controlar la proporción de espacio que ocupa cada `Expanded`.
*   **`Flexible`:**  Similar a `Expanded`, pero más flexible (permite que el hijo se ajuste al espacio disponible o que se desborde).
*   **`Stack`:**  Superpone sus widgets hijos uno encima del otro.  Útil para colocar elementos en posiciones específicas o crear efectos de superposición.
    *   **`Positioned`:**  Se usa *dentro* de un `Stack` para posicionar un widget hijo de forma absoluta (con coordenadas `top`, `bottom`, `left`, `right`).
*   **`Padding`:**  Aplica padding alrededor de un widget.
*   **`Center`:**  Centra su widget hijo.
*   **`Align`:**  Alinea su widget hijo dentro de sí mismo.
*   **`SizedBox`:**  Crea una caja de un tamaño específico.  Útil para crear espacios o para restringir el tamaño de un widget.
    * `SizedBox.expand()`: Expande para ocupar todo el espacio disponible.
*   **`AspectRatio`:**  Fuerza a que un widget tenga una relación de aspecto específica.
*   **`Wrap`**:  Similar a `Row` o `Column`, pero si los hijos no caben, los envuelve a la siguiente línea.
*  **`ListView`**: Muestra una lista *desplazable* de widgets.  *Muy importante*.
    *   `ListView.builder`:  Construye la lista de forma *eficiente*, creando solo los widgets que están visibles en la pantalla.  Ideal para listas largas o dinámicas.
    *   `ListView.separated`:  Similar a `ListView.builder`, pero permite insertar un widget separador entre cada elemento de la lista.
*   **`GridView`:**  Muestra una cuadrícula (grid) desplazable de widgets.
    *   `GridView.builder`:  Construye la cuadrícula de forma eficiente.
*   **`SingleChildScrollView`:**  Hace que un solo widget sea desplazable (si su contenido es más grande que el espacio disponible).
* `Spacer`: Crea un espacio flexible, dentro de Rows o Columns.

## 🖼️ **Widgets de Presentación (UI)**

*   **`Text`:**  Muestra texto.
    *   `style`:  `TextStyle` (para dar formato al texto: fuente, tamaño, color, negrita, cursiva, etc.).
*   **`RichText`:**  Muestra texto con diferentes estilos en la misma línea.
*   **`Icon`:**  Muestra un icono (de Material Icons o de un paquete de iconos).
    *   `Icons.<nombre_icono>` (ej: `Icons.add`, `Icons.home`).
*   **`Image`:**  Muestra una imagen.
    *   `Image.asset('assets/imagen.png')`:  Carga una imagen desde los assets del proyecto.
    *   `Image.network('<URL>')`:  Carga una imagen desde una URL.
    *   `Image.file(File('ruta/a/archivo'))`:  Carga una imagen desde un archivo.
    *   `fit`:  `BoxFit` (controla cómo se ajusta la imagen al espacio disponible: `cover`, `contain`, `fill`, `fitWidth`, `fitHeight`).
*   **`CircularProgressIndicator`:**  Indicador de progreso circular (spinner).
*   **`LinearProgressIndicator`:**  Indicador de progreso lineal.
*   **`Divider`:**  Línea horizontal de separación.
*   **`Card`:**  Un contenedor con esquinas redondeadas y sombra (estilo Material Design).
*   **`ListTile`:**  Un widget diseñado para mostrar una fila en una lista (con título, subtítulo, icono principal, icono secundario, etc.).
*  `CircleAvatar`: Muestra una imagen en un círculo.
*  `Chip`: Muestra una etiqueta o categoría.

## 👆 **Widgets de Interacción (Input)**

*   **`TextField`:**  Campo de entrada de texto.
    *   `controller`:  `TextEditingController` (para controlar el texto del campo, obtener el valor actual, etc.).
    *   `onChanged`:  Función que se llama cada vez que el texto cambia.
    *   `onSubmitted`:  Función que se llama cuando el usuario presiona Enter.
    *   `decoration`:  `InputDecoration` (para personalizar la apariencia del campo: borde, etiqueta, icono, etc.).
    *   `keyboardType`:  Controla el tipo de teclado que se muestra (texto, numérico, email, etc.).
    * `obscureText`: Oculta el texto (para contraseñas).
*   **`ElevatedButton` (antes `RaisedButton`):**  Botón con efecto de elevación (estilo Material Design).
    *   `onPressed`:  Función que se llama cuando se presiona el botón.  Si es `null`, el botón se deshabilita.
    *   `child`:  El widget que se muestra dentro del botón (normalmente un `Text`).
*   **`TextButton` (antes `FlatButton`):**  Botón de texto (sin elevación).
*   **`OutlinedButton` (antes `OutlineButton`):** Botón con borde.
*   **`IconButton`:**  Botón que muestra un icono.
*   **`FloatingActionButton`:**  Botón flotante (circular, generalmente en la esquina inferior derecha; estilo Material Design).
*   **`Checkbox`:**  Casilla de verificación.
    *   `value`:  `bool` (si está marcado o no).
    *   `onChanged`:  Función que se llama cuando cambia el estado.
*   **`Radio`:**  Botón de opción (solo se puede seleccionar uno de un grupo).
*   **`Switch`:**  Interruptor (on/off).
*   **`Slider`:**  Control deslizante para seleccionar un valor dentro de un rango.
*   **`DropdownButton`:**  Menú desplegable.
*   **`GestureDetector`:**  Detecta gestos (toques, arrastres, pellizcos, etc.) en su widget hijo.
    *   `onTap`:  Función que se llama cuando se toca el widget.
    *   `onDoubleTap`, `onLongPress`, `onPanUpdate`, etc.
*   **`InkWell`:**  Similar a `GestureDetector`, pero agrega un efecto visual de "onda" (ripple effect) al tocar (estilo Material Design).

## 🧭 **Navegación**

*   **`Navigator`:**  Gestiona la navegación entre pantallas (rutas).
    *   `Navigator.push(context, MaterialPageRoute(builder: (context) => NuevaPantalla()))`:  Navega a una nueva pantalla.
    *   `Navigator.pop(context)`:  Vuelve a la pantalla anterior.
    *   `Navigator.pushNamed(context, '/ruta')`: Navega a una ruta con nombre.
*   **`MaterialPageRoute`:**  Ruta que usa transiciones de Material Design.
*   **Rutas con nombre:**  Define rutas con nombres en `MaterialApp` (en la propiedad `routes`) para facilitar la navegación.

## 🗄️ **Estado (State Management)**

*   **`setState()` (ya mencionado):**  La forma más básica de gestionar el estado.
*   **`Provider`:**  Paquete *muy popular* para la gestión del estado.  Proporciona una forma sencilla y eficiente de compartir datos entre widgets.
    *   `ChangeNotifier`:  Clase que se usa para notificar a los widgets que dependen de ella cuando cambia el estado.
    *   `ChangeNotifierProvider`:  Proporciona una instancia de `ChangeNotifier` a sus descendientes.
    *   `Consumer`:  Obtiene una instancia de `ChangeNotifier` y reconstruye su widget hijo cuando el `ChangeNotifier` notifica cambios.
*   **`Riverpod`:**  Otro paquete popular para la gestión del estado, similar a `Provider` pero con algunas diferencias (mayor flexibilidad, mejor testing, etc.).
*   **`Bloc` (Business Logic Component):**  Patrón de arquitectura más complejo para separar la lógica de presentación de la UI.  Usa streams y sinks.
*   **`GetX`:**  Un "microframework" que proporciona gestión del estado, navegación, gestión de dependencias y otras utilidades.  Muy popular por su simplicidad y rendimiento.

## 🌐 **Red y Datos**

*   **`http` package:**  Para realizar solicitudes HTTP (GET, POST, etc.).
    *   `http.get(Uri.parse('<URL>'))`:  Realiza una solicitud GET.
    *   `http.post(Uri.parse('<URL>'), body: {'clave': 'valor'})`:  Realiza una solicitud POST.
*   **`jsonDecode()` (del paquete `dart:convert`):**  Convierte una cadena JSON en un objeto Dart (normalmente un `Map`).
*   **`jsonEncode()`:**  Convierte un objeto Dart en una cadena JSON.
*   **`Future`:**  Representa un valor potencial (o error) que estará disponible en algún momento en el futuro.  Se usa para operaciones asíncronas (como solicitudes HTTP).
    *   `async` y `await`:  Palabras clave para trabajar con `Future`s de forma más sencilla (hacen que el código asíncrono se vea y se comporte como código síncrono).
*   **`Stream`:**  Representa una secuencia de datos asíncronos.
*   **`SharedPreferences`:**  Almacena datos simples (clave-valor) de forma persistente en el dispositivo.
*  **Bases de datos**:
    * `sqflite`: Base de datos SQLite.
    * `Firebase`: Base de datos en la nube de Google.

## 🎨 **Temas (Themes)**

*   **`ThemeData`:**  Define el tema de la aplicación (colores, fuentes, estilos de botones, etc.).
    *   `primaryColor`, `accentColor`, `backgroundColor`, `textTheme`, etc.
*   **`Theme.of(context)`:**  Accede al tema actual desde cualquier widget.

## 🧪 **Testing**

*   **Unit tests:**  Prueban unidades individuales de código (funciones, clases).
*   **Widget tests:**  Prueban widgets individuales.
*   **Integration tests:**  Prueban la aplicación completa o partes grandes de ella.
*   **`flutter test`:**  Comando para ejecutar pruebas.

## ✨ **Otros Widgets y Conceptos Útiles**

*   **`AnimatedContainer`:**  Versión animada de `Container`.  Anima automáticamente los cambios en sus propiedades.
*   **`Hero`:**  Crea una animación "hero" (un widget que "vuela" entre dos pantallas durante la navegación).
*   **`AlertDialog`:**  Muestra un cuadro de diálogo (alerta).
*   **`SnackBar`:**  Muestra un mensaje breve en la parte inferior de la pantalla.
*   **`Form`:**  Contenedor para agrupar campos de formulario y validarlos.
    *   `TextFormField`:  `TextField` diseñado para usarse dentro de un `Form`.
    *   `Validators`:  Funciones para validar la entrada del usuario.
*  **Internacionalización (i18n) y Localización (l10n)**: Adaptar la app a diferentes idiomas y regiones.
*  `FutureBuilder`:  Widget que se construye a sí mismo basándose en el último snapshot de un `Future`.
*  `StreamBuilder`:  Widget que se construye a sí mismo basándose en el último snapshot de un `Stream`.
*  `SafeArea`:  Evita que el contenido de la app se superponga con elementos del sistema (barra de estado, notch, etc.).
*  `MediaQuery`:  Obtiene información sobre el tamaño de la pantalla, orientación, etc.
