# 🐦🔥 Cheatsheet de Flutter Avanzado: Lleva tus Apps al Siguiente Nivel 🚀

## 🎨 **Animaciones**

*   **`AnimationController`:**  Controla una animación.  Define la duración, el rango de valores (usando `Tween`) y permite iniciar, detener, revertir, etc.
    *   `vsync`:  *Muy importante*.  Necesita un `TickerProvider` (generalmente `SingleTickerProviderStateMixin` o `TickerProviderStateMixin` en tu `State`).  Evita que la animación consuma recursos cuando la pantalla no está visible.
    *   `forward()`, `reverse()`, `stop()`, `repeat()`.
    *   `value`:  El valor actual de la animación (entre 0.0 y 1.0 por defecto).
    *   `duration`:  Duración de la animación.
    *   `addListener()`:  Para escuchar los cambios en el valor de la animación y reconstruir el widget (generalmente se usa `AnimatedBuilder` en su lugar).
    * `addStatusListener()`: Para escuchar cambios en el *status* de la animación (started, completed, dismissed, forward, reverse).
*   **`Tween`:**  Define el rango de valores de una animación.  `Tween<double>(begin: 0.0, end: 1.0)` es el valor por defecto de un `AnimationController`.  Puedes usar `Tween` para animar otros tipos de datos (colores, tamaños, rectángulos, etc.).
*   **`AnimatedWidget`:**  Clase base para widgets que se reconstruyen automáticamente cuando cambia el valor de una animación.  Simplifica la creación de widgets animados (no necesitas llamar a `addListener()` y `setState()`).
*   **`AnimatedBuilder`:**  Widget que se reconstruye cuando cambia el valor de una animación.  Más flexible que `AnimatedWidget` (no es necesario crear una subclase).  *Muy recomendado*.
    ```dart
    AnimatedBuilder(
      animation: animationController, // o cualquier Listenable
      builder: (context, child) {
        return Transform.rotate(
          angle: animationController.value * 2 * pi, // Anima la rotación
          child: child,
        );
      },
      child: FlutterLogo(size: 100), // El child se pasa para optimización.
    )
    ```
*   **`CurvedAnimation`:**  Aplica una curva (easing curve) a una animación, modificando su velocidad a lo largo del tiempo (aceleración/desaceleración).  Ej: `Curves.easeIn`, `Curves.easeOut`, `Curves.easeInOut`, `Curves.bounceOut`, etc.
*   **`TweenSequence`:**  Encadena varias animaciones `Tween` para crear animaciones más complejas.
*   **Animaciones implícitas:**
    *   **`AnimatedContainer` (ya mencionado en el básico):**  La forma más sencilla de crear animaciones.
    *   **`AnimatedOpacity`:**  Anima la opacidad.
    *   **`AnimatedPadding`:**  Anima el padding.
    *   **`AnimatedPositioned`:**  Anima la posición de un widget dentro de un `Stack`.
    *   **`AnimatedCrossFade`:**  Realiza un cross-fade (fundido cruzado) entre dos widgets.
    *   **`AnimatedSize`:**  Anima el tamaño de un widget.
    *  `AnimatedDefaultTextStyle`: Anima el estilo por defecto de un texto.
    *  `AnimatedSwitcher`:  Anima la transición entre dos widgets.
*   **`Hero` (ya mencionado, pero es una animación importante):**  Recuerda que el `Hero` en la pantalla de origen y el `Hero` en la pantalla de destino deben tener el mismo `tag`.
* **Staggered Animations**: Anima una secuencia de widgets, con un desfase entre cada animación.

## 🌐 **Renderizado Personalizado (Custom Painting)**

*   **`CustomPaint`:**  Widget que permite dibujar directamente en el lienzo (canvas).  Necesitas un `CustomPainter`.
*   **`CustomPainter`:**  Clase abstracta que debes extender para implementar la lógica de dibujo.
    *   `paint(Canvas canvas, Size size)`:  Método donde realizas el dibujo.  Recibes un objeto `Canvas` (para dibujar líneas, formas, texto, imágenes, etc.) y un objeto `Size` (el tamaño disponible para dibujar).
    *   `shouldRepaint(covariant CustomPainter oldDelegate)`:  Método que determina si el `CustomPainter` debe volver a dibujar (si ha cambiado algo que afecte al dibujo).
    ```dart
    class MyPainter extends CustomPainter {
      @override
      void paint(Canvas canvas, Size size) {
        final paint = Paint()
          ..color = Colors.blue
          ..strokeWidth = 5
          ..style = PaintingStyle.stroke; // O PaintingStyle.fill

        canvas.drawLine(Offset(0, 0), Offset(size.width, size.height), paint);

        final rect = Rect.fromLTWH(50, 50, 100, 100);
        canvas.drawRect(rect, paint);

        canvas.drawCircle(Offset(200, 200), 50, paint);

        final textStyle = TextStyle(color: Colors.black, fontSize: 20);
        final textSpan = TextSpan(text: 'Hola, mundo!', style: textStyle);
        final textPainter = TextPainter(text: textSpan, textDirection: TextDirection.ltr);
        textPainter.layout(); // Importante llamar a layout() antes de pintar.
        textPainter.paint(canvas, Offset(100, 300));
      }

      @override
      bool shouldRepaint(covariant CustomPainter oldDelegate) {
        return false; // Si no hay cambios, no es necesario volver a pintar.
      }
    }
    ```

## ⚙️ **Gestión del Estado Avanzada**

*   **`InheritedWidget`:**  Clase base para widgets que propagan información *hacia abajo* en el árbol de widgets de forma *eficiente*.  Los widgets descendientes pueden acceder a la información sin necesidad de pasarla explícitamente a través de constructores.  Es la base de muchos patrones de gestión del estado (como `Provider`).
    *   `updateShouldNotify(covariant InheritedWidget oldWidget)`:  Determina si los widgets descendientes deben reconstruirse cuando el `InheritedWidget` cambia.

*   **`Provider` (ya mencionado en el básico, pero profundicemos):**
    *   **`ProxyProvider`:**  Combina varios `Provider`s.  Útil cuando un `Provider` depende de otros.
    *   **`MultiProvider`:**  Proporciona varios `Provider`s a la vez.
    *   `Provider.of<T>(context, listen: false)`:  Obtiene una instancia de un `Provider` *sin* escuchar los cambios (útil para llamar a métodos del `Provider` sin reconstruir el widget).
* **`Riverpod` (ya mencionado en el básico).  Algunas ventajas sobre `Provider`:**
    * No depende del `BuildContext`.
    * Es más seguro a la hora de compilar.
    * Facilita el testing
*   **`Bloc` / `Cubit` (ya mencionados, pero son importantes):**
    *   **`Bloc`:**  Recibe eventos y emite estados.  Usa streams.  Más potente y flexible, pero más complejo.
    *   **`Cubit`:**  Una versión más simple de `Bloc`.  Expone funciones que modifican el estado y emiten nuevos estados.  No usa streams directamente (aunque internamente sí).  Más fácil de aprender y usar.
    *   **`flutter_bloc` package:**  Proporciona widgets para integrar `Bloc` y `Cubit` en la UI de Flutter (como `BlocProvider`, `BlocBuilder`, `BlocListener`, `BlocConsumer`).
* **`GetX` (ya mencionado):**
  * `Get.put()`: Inyecta una dependencia.
  * `Get.find()`: Recupera una dependencia.
  * `Obx(() => ...)`: Reconstruye un widget cuando una variable reactiva cambia.
  * `GetBuilder`: Otra forma de reconstruir widgets basado en cambios.
  * `Get.toNamed('/ruta')`, `Get.back()`: Navegación.

## 📱 **Integración con Plataforma Nativa (Platform Channels)**

*   **Platform Channels:**  Permiten la comunicación entre el código Dart de Flutter y el código nativo (Java/Kotlin en Android, Objective-C/Swift en iOS).  Útil para acceder a APIs nativas que no están disponibles directamente en Flutter.
    *   **`MethodChannel`:**  Canal para llamar a métodos en el lado nativo y recibir resultados.
    *   **`EventChannel`:**  Canal para recibir streams de eventos desde el lado nativo.
    *   **`BasicMessageChannel`:** Canal bidireccional para enviar mensajes.
*   **`Pigeon`:**  (Recomendado) Genera código para platform channels de forma segura (type-safe).  Evita errores comunes y simplifica la comunicación.  Define la interfaz en Dart y Pigeon genera el código nativo correspondiente.

## 📐 **Layouts Personalizados**

*   **`CustomMultiChildLayout`:**  Widget que permite controlar el layout de varios widgets hijos de forma personalizada.  Necesitas un `MultiChildLayoutDelegate`.
*   **`CustomSingleChildLayout`:** Similar, pero para un único widget hijo. Necesitas un `SingleChildLayoutDelegate`.
* **`LayoutBuilder`**: Permite construir un widget en base a las restricciones que le da su padre.

## 🧪 **Testing Avanzado**

*   **Golden tests:**  Comparan la salida visual de un widget con una imagen de referencia ("golden file").  Detectan cambios visuales no deseados.
*   **Mocking:**  Crear objetos simulados (mocks) para reemplazar dependencias reales en las pruebas (bases de datos, servicios web, etc.).  Permite aislar la unidad que se está probando.  Paquetes como `mockito` facilitan la creación de mocks.
*   **Test Doubles:**  Término genérico para objetos que reemplazan a objetos reales en las pruebas (mocks, stubs, fakes, spies, etc.).
*  **BDD (Behavior-Driven Development)**: Con paquetes como `flutter_gherkin`.

## ⚙️ **Optimización del Rendimiento**

*   **Evita reconstrucciones innecesarias de widgets:**
    *   Usa `const` widgets siempre que sea posible.
    *   Usa `RepaintBoundary` para aislar partes de la UI que se redibujan con frecuencia.
    *   Usa `keys` con cuidado (solo cuando sea necesario).
    *   Usa `ListView.builder` o `GridView.builder` para listas y cuadrículas largas.
*   **Perfilado (Profiling):**  Usa las herramientas de desarrollo de Flutter (DevTools) para identificar cuellos de botella en el rendimiento (CPU, memoria, GPU, red).
*   **`dart:ui`:**  Acceso de bajo nivel a las APIs de renderizado de Flutter (para optimizaciones avanzadas).
* **Isolates**: Ejecuta código en hilos separados para evitar bloquear el hilo principal de la UI.

## 📦 **Otros Temas Avanzados**

*   **Generación de código:**  Usar herramientas como `build_runner` y paquetes como `json_serializable`, `freezed`, `dart_mappable` para generar código repetitivo (serialización/deserialización de JSON, clases inmutables, etc.).
*   **Internacionalización (i18n) y Localización (l10n) avanzadas:**  Gestión de plurales, género, formatos de números y fechas, etc.  Paquetes como `intl`.
*   **Deep linking:**  Permite que los usuarios accedan a pantallas específicas de la aplicación desde URLs.
*  **Background processes**: Ejecutar tareas en segundo plano.
* **Acceso a hardware**: Cámaras, GPS, Bluetooth, sensores, etc.  Hay paquetes para la mayoría de las funcionalidades de hardware.
* **WebSockets**: Comunicación bidireccional en tiempo real.
* **FFI (Foreign Function Interface)**: Llamar a funciones nativas escritas en C/C++.
*   **Trabajar con código nativo:**  Crear plugins de Flutter que expongan APIs nativas a Dart.
* **Firebase**: Usar servicios de Firebase (Authentication, Cloud Firestore, Cloud Storage, Cloud Functions, etc.)