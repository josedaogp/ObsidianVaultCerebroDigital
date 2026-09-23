# 🤖 Cheatsheet de Android Studio

Este cheatsheet cubre los aspectos esenciales de Android Studio, el IDE oficial para el desarrollo de aplicaciones Android.

## 🔍 Conceptos Básicos

*   **Android Studio:** 💡 Entorno de desarrollo integrado (IDE) basado en IntelliJ IDEA, diseñado específicamente para el desarrollo de aplicaciones Android.
*   **SDK (Software Development Kit):** 📦 Conjunto de herramientas necesarias para desarrollar aplicaciones Android (compilador, emulador, bibliotecas, etc.).  Android Studio lo gestiona automáticamente (SDK Manager).
*   **AVD (Android Virtual Device):** 📱 Emulador de un dispositivo Android.
*   **Gradle:** 🛠️ Sistema de construcción (*build system*) utilizado en Android Studio.  Define la configuración del proyecto, dependencias, tareas de compilación, etc.
*   **ADB (Android Debug Bridge):** 🔌 Herramienta de línea de comandos para comunicarse con un dispositivo Android (real o emulado).
*   **Proyecto (Project):** 📂 Contenedor de todos los archivos de una aplicación Android (código fuente, recursos, manifiesto, etc.).
*   **Módulo (Module):** 🧩 Unidad de código independiente dentro de un proyecto (ej. una app, una biblioteca, un módulo de Wear OS).
*   **Activity:** 📝 Componente de una aplicación Android que representa una pantalla con una interfaz de usuario.
*   **Fragment:** 🧩 Parte modular de una Activity (se puede reutilizar en diferentes Activities).
*   **Intent:** 📩 Mensaje que se usa para comunicar entre componentes de una aplicación (ej. iniciar una Activity, enviar datos a un Service).
*   **Service:** ⚙️ Componente que se ejecuta en segundo plano (sin interfaz de usuario).
*   **Broadcast Receiver:** 📡 Componente que responde a eventos del sistema o de otras aplicaciones.
*   **Content Provider:** 🗄️ Componente que proporciona acceso a datos (ej. contactos, calendario).
*   **Layout:** 📐 Define la estructura visual de una Activity o Fragment (cómo se organizan los elementos de la UI).
*   **Resource:** 🖼️ Archivo externo (no código) que se utiliza en la aplicación (ej. imágenes, cadenas de texto, layouts, estilos).
*   **Manifest (AndroidManifest.xml):** 📄 Archivo XML que describe la aplicación (nombre, icono, permisos, componentes, etc.).
*   **APK (Android Package Kit):** 📦 Formato de archivo para distribuir e instalar aplicaciones Android.

## 🗂️ Estructura de un Proyecto Android

*   **`app/`:**  Contiene el código fuente principal de la aplicación.
    *   **`manifests/`:**  Contiene el archivo `AndroidManifest.xml`.
    *   **`java/` (o `kotlin/`):**  Contiene el código fuente Java o Kotlin.
        *   `com.example.mi_app` (o similar):  Paquete principal de la aplicación.  Contiene Activities, Fragments, clases de datos, etc.
        *   `com.example.mi_app (androidTest)`:  Contiene pruebas instrumentadas (se ejecutan en un dispositivo o emulador).
        *   `com.example.mi_app (test)`:  Contiene pruebas unitarias (se ejecutan en la JVM local).
    *   **`res/`:**  Contiene los recursos de la aplicación.
        *   **`drawable/`:**  Imágenes (PNG, JPG, GIF, SVG, etc.).
        *   **`layout/`:**  Archivos XML que definen los layouts.
        *   **`mipmap/`:**  Iconos de la aplicación (diferentes densidades de pantalla).
        *   **`values/`:**  Recursos de valores (cadenas de texto, colores, dimensiones, estilos, temas, etc.).
            *   `strings.xml`:  Cadenas de texto (para internacionalización).
            *   `colors.xml`:  Colores.
            *   `dimens.xml`:  Dimensiones (tamaños, márgenes, paddings).
            *   `styles.xml`:  Estilos (atributos visuales reutilizables).
            *   `themes.xml`:  Temas (estilos aplicados a toda la aplicación).
        *   **`menu/`: ** Menús
        *   **`raw/`:** Archivos en bruto (ej. audio, video).
        *   **`font/`:** Fuentes tipográficas.
        *   **`anim/`:** Animaciones.
        *   **`xml/`:** Otros archivos XML.
*   **`Gradle Scripts/`:**  Contiene los archivos de configuración de Gradle.
    *   **`build.gradle (Project)`:**  Configuración global del proyecto (repositorios, plugins, etc.).
    *   **`build.gradle (Module: app)`:**  Configuración específica del módulo `app` (dependencias, versión de la aplicación, opciones de compilación, etc.).
    *   `settings.gradle`: Define los módulos del proyecto.
    *  `gradle.properties`: Propiedades globales de Gradle.
    *  `local.properties`: Propiedades locales (normalmente no se versiona).

## 💻 El Editor de Android Studio

*   **Editor de código:**  Edición de archivos Java, Kotlin, XML, etc.  Ofrece autocompletado, resaltado de sintaxis, refactorización, etc.
*   **Editor de layouts:**  Editor visual para diseñar layouts (arrastrar y soltar componentes, previsualización, etc.).
*   **Navigation Editor:**  Editor visual para definir la navegación entre pantallas (Activities, Fragments) usando Navigation Component.
*   **Design tools:**  Editores visuales para otros recursos (imágenes, vectores, etc.).
*   **Ventanas de herramientas (Tool Windows):**
    *   **Project:**  Muestra la estructura del proyecto.
    *   **Structure:**  Muestra la estructura del archivo actual (clases, métodos, etc.).
    *   **Build:**  Muestra los resultados de la compilación.
    *   **Logcat:**  Muestra los logs del dispositivo/emulador.
    *   **TODO:**  Muestra las tareas pendientes (comentarios TODO).
    *   **Version Control:**  Integración con Git (u otros VCS).
    *   **Terminal:**  Acceso a la línea de comandos.
    *   **Profiler:**  Herramientas para analizar el rendimiento de la aplicación (CPU, memoria, red, energía).
    *   **Database Inspector:** Permite inspeccionar, consultar y modificar bases de datos SQLite en tu aplicación.
    *  **Device File Explorer:** Permite explorar el sistema de archivos del dispositivo/emulador.
    * **Resource Manager:** Gestiona los recursos.
    * **App Inspection:** Inspecciona la app en ejecución.

## 📱 Emulador (AVD Manager)

*   **AVD Manager:**  Herramienta para crear y gestionar dispositivos virtuales Android (AVDs).
    *   Acceso:  Tools > AVD Manager (o desde la barra de herramientas).
    *   Permite definir:
        *   Dispositivo (teléfono, tablet, Wear OS, Android TV).
        *   Versión de Android (API level).
        *   Orientación (portrait, landscape).
        *   Tamaño de la pantalla y densidad.
        *   Memoria RAM, almacenamiento, etc.
*   **Ejecutar el emulador:**  Selecciona un AVD y haz clic en el botón "Play".
* **Controles del emulador:**
    * Botones de navegación (atrás, inicio, recientes).
    * Rotación.
    * Volumen.
    * Zoom.
    * Captura de pantalla.
    * Ubicación simulada.
    * ...

## 🐞 Depuración (Debugging)

1.  **Puntos de interrupción (Breakpoints):**  Haz clic en el margen izquierdo del editor de código para establecer un punto de interrupción.
2.  **Iniciar la depuración:**  Haz clic en el icono "Debug" (o Run > Debug).
3.  **Controles de depuración:**
    *   **Step Over:**  Ejecuta la línea actual (sin entrar en funciones/métodos).
    *   **Step Into:**  Entra en la función/método actual.
    *   **Step Out:**  Sale de la función/método actual.
    *   **Run to Cursor:**  Ejecuta hasta la posición del cursor.
    *   **Evaluate Expression:**  Evalúa una expresión en el contexto actual.
    *   **Resume Program:**  Continúa la ejecución normal.
    * **Stop**: Detiene.
    * **View Breakpoints:** Ver y gestionar todos los puntos.
4.  **Ventana de depuración:**
    *   **Frames:**  Muestra la pila de llamadas.
    *   **Variables:**  Muestra las variables locales y sus valores.
    *   **Watches:**  Permite evaluar expresiones personalizadas.

## ⌨️ Atajos de Teclado (Shortcuts) - Windows/Linux (macOS)

*   **General:**
    *   `Ctrl+Shift+A` (`Cmd+Shift+A`):  Find Action (buscar cualquier acción).
    *   `Ctrl+N` (`Cmd+O`):  Go to Class (buscar una clase).
    *   `Ctrl+Shift+N` (`Cmd+Shift+O`):  Go to File (buscar un archivo).
    *   `Ctrl+Alt+Shift+N` (`Cmd+Option+O`): Go to Symbol
    *   `Ctrl+F12` : Go to implementation
    *   `Ctrl+P`: Parameter Info
    *   `Ctrl+Q` : Quick documentation
    *   `Ctrl+B` (`Cmd+B`):  Go to Declaration (ir a la declaración).
    *   `Ctrl+Alt+B` (`Cmd+Option+B`):  Go to Implementation(s)
    *   `Ctrl+U` : Go to super
    *   `Alt+F7` (`Option+F7`): Find Usages
    *   `Ctrl+Alt+F7` : Show Usages
    *   `Ctrl+Alt+L` (`Cmd+Option+L`):  Reformat Code (formatear código).
    *   `Ctrl+Alt+O`: Optimize imports
    *   `Ctrl+/` (`Cmd+/`):  Comentar/descomentar línea.
    *   `Ctrl+Shift+/` (`Cmd+Shift+/`):  Comentar/descomentar bloque.
    *   `Ctrl+W` (`Option+Up`):  Extend Selection (selección inteligente).
    *   `Ctrl+Shift+W`: Shrink selection
    *   `Ctrl+D` (`Cmd+D`):  Duplicar línea o selección.
    *   `Ctrl+Y` (`Cmd+Delete`):  Eliminar línea.
    *   `Ctrl+F` (`Cmd+F`):  Find (buscar).
    *   `Ctrl+R` (`Cmd+R`):  Replace (reemplazar).
    *   `Ctrl+Shift+F` (`Cmd+Shift+F`):  Find in Path (buscar en todo el proyecto).
    *   `Ctrl+Shift+R`: Replace in Path
    *   `Alt+Enter`:  Show Intention Actions (mostrar acciones rápidas, correcciones, etc.).
    *   `Ctrl+Space` (`Cmd+Space`):  Basic Code Completion (autocompletado básico).
    *   `Ctrl+Shift+Enter`: Complete Statement
    *   `Shift+F6` (`Shift+F6`):  Rename (refactorizar nombre).
    *   `Ctrl+Alt+M` (`Cmd+Option+M`):  Extract Method (refactorizar a método).
    *   `F2`: Next highlighted error.
    *   `Ctrl+E`: Recent Files
    *   `Ctrl+Shift+E`: Recent Locations
    *   `Ctrl+J` : Insert Live Template
    *   `Tab`: Indent
    *   `Shift+Tab`: Unindent

*   **Ejecución/Depuración:**
    *   `Shift+F10` (`Ctrl+R`):  Run (ejecutar).
    *   `Shift+F9` (`Ctrl+D`):  Debug (depurar).
    *   `F8`:  Step Over (paso a paso).
    *   `F7`:  Step Into (entrar en función).
    *   `Shift+F8`:  Step Out (salir de función).
    *   `Alt+F9` (`Option+F9`): Run to Cursor
    *   `Alt+F8`: Evaluate Expression.
    *   `F9`: Resume Program.

* **Navegación:**
   * `Ctrl+Tab`: Switcher
   * `Ctrl+G`: Go to line
   * `Ctrl+[` / `Ctrl+]`: Ir al inicio o final del bloque de código.
   * `Alt+Left` / `Alt+Right`: Navegar atrás y adelante.

*   **Ventanas de herramientas:**
    *   `Alt+1` (`Cmd+1`):  Project.
    *   `Alt+4` (`Cmd+4`):  Run.
    *   `Alt+5` (`Cmd+5`):  Debug.
    *   `Alt+6` (`Cmd+6`):  Logcat.
    *   `Alt+9` (`Cmd+9`):  Version Control.
    *   `Ctrl+Shift+F12`:  Toggle Full Screen Mode.

* **Refactoring:**
  * `Ctrl+Alt+Shift+T`: Refactor This

*Nota:*  Estos son solo algunos de los atajos más comunes.  Android Studio tiene muchos más.  Puedes personalizarlos en File > Settings > Keymap (Android Studio > Preferences > Keymap en macOS).

## 🛠️ Gradle (Build System)

*   **`build.gradle (Project)`:**  Configuración global del proyecto.

    ```groovy
    buildscript {
        repositories {
            google()
            mavenCentral()
        }
        dependencies {
            classpath "com.android.tools.build:gradle:8.x.x" // Versión del plugin de Gradle para Android
            classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:1.x.x" // Plugin de Kotlin
            // Otros plugins
        }
    }

    plugins {
        id 'com.android.application' version '8.x.x' apply false
        id 'org.jetbrains.kotlin.android' version '1.x.x' apply false
        //Otros plugins
    }
    allprojects {
        repositories {
            google()
            mavenCentral()
        }
    }
    ```

*   **`build.gradle (Module: app)`:**  Configuración específica del módulo (normalmente la app).

    ```groovy
    plugins {
        id 'com.android.application'
        id 'org.jetbrains.kotlin.android'
        // Otros plugins (ej. para Firebase, Hilt, etc.)
    }

    android {
        compileSdk 34 // Versión del SDK de compilación

        defaultConfig {
            applicationId "com.example.mi_app" // ID de la aplicación
            minSdk 21 // Versión mínima de Android soportada
            targetSdk 34 // Versión de Android a la que se dirige la aplicación
            versionCode 1 // Código de versión (entero)
            versionName "1.0" // Nombre de versión (cadena)

            testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
        }

        buildTypes { // Configuraciones de compilación (debug, release, etc.)
            release {
                minifyEnabled true // Habilitar ProGuard (ofuscación y optimización)
                proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
                signingConfig signingConfigs.release // Configuración de firma (para release)
            }
        }

        compileOptions {
            sourceCompatibility JavaVersion.VERSION_17
            targetCompatibility JavaVersion.VERSION_17
        }
        kotlinOptions {
            jvmTarget = "17"
        }

        buildFeatures{ //Habilitar características
            viewBinding true // View Binding
            //dataBinding true // Data Binding
            //compose true // Jetpack Compose
        }
        // composeOptions { ... } // Configuración de Jetpack Compose (si se usa)
    }

    dependencies {
        implementation "androidx.core:core-ktx:1.x.x"
        implementation "androidx.appcompat:appcompat:1.x.x"
        implementation "com.google.android.material:material:1.x.x"
        // Otras dependencias (librerías, módulos locales, etc.)

        testImplementation "junit:junit:4.x.x" // Pruebas unitarias
        androidTestImplementation "androidx.test.ext:junit:1.x.x" // Pruebas instrumentadas
        androidTestImplementation "androidx.test.espresso:espresso-core:3.x.x"
    }
    ```

*   **Tareas de Gradle:**  Se ejecutan desde la ventana de Gradle (View > Tool Windows > Gradle) o desde la línea de comandos (`./gradlew <tarea>`).
    *   `assembleDebug`:  Compila la versión de depuración de la aplicación.
    *   `assembleRelease`:  Compila la versión de lanzamiento.
    *   `build`:  Compila todas las variantes de la aplicación.
    *   `clean`:  Limpia el directorio de compilación.
    *   `test`:  Ejecuta las pruebas unitarias.
    *   `connectedAndroidTest`:  Ejecuta las pruebas instrumentadas.
    *   `installDebug`:  Instala la versión de depuración en un dispositivo/emulador.

## ✨ Otros Aspectos Importantes

*   **Logcat:**  Herramienta para ver los logs del sistema y de la aplicación.
    *   Niveles de log:  Verbose, Debug, Info, Warn, Error, Assert.
    *   Filtrado por nivel, tag, mensaje, etc.

*   **SDK Manager:**  Herramienta para gestionar las versiones del SDK de Android, las herramientas de build, las imágenes del sistema (para el emulador), etc.
    *   Acceso:  Tools > SDK Manager (o desde la barra de herramientas).

*   **Layout Inspector:**  Herramienta para inspeccionar la jerarquía de vistas de una aplicación en ejecución.
*   **Profiler:**  Herramientas para analizar el rendimiento de la aplicación.
*   **Firebase Assistant:**  Herramienta para integrar servicios de Firebase en tu aplicación.

Este cheatsheet cubre una amplia variedad de aspectos de Android Studio, desde la estructura del proyecto y el editor hasta la depuración, los atajos de teclado y Gradle. ¡Guárdalo y consúltalo a menudo!