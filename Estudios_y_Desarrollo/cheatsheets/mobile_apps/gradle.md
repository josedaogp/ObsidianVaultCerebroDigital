# 🛠️ Cheatsheet de Gradle

Este cheatsheet cubre los aspectos esenciales de Gradle, el sistema de construcción (*build system*) de código abierto, flexible y potente, utilizado principalmente para proyectos Java, Kotlin, Android, y otros.

## 🔍 Conceptos Básicos

*   **Gradle:** Sistema de construcción que automatiza tareas como la compilación, pruebas, empaquetado y despliegue de software.
*   **Build Script:** 📄 Archivo que define la configuración de un proyecto Gradle (tareas, dependencias, plugins, etc.).  Normalmente se llama `build.gradle` (Groovy DSL) o `build.gradle.kts` (Kotlin DSL).
*   **Tarea (Task):** ⚙️ Unidad básica de trabajo en Gradle (ej. compilar código, ejecutar pruebas, generar Javadoc).
*   **Plugin:** 🧩 Extensión que añade funcionalidad a Gradle (ej. soporte para un lenguaje específico, integración con una herramienta, etc.).
*   **Dependencia (Dependency):** 📦 Biblioteca o módulo externo del que depende tu proyecto.
*   **Repositorio (Repository):** 🗄️ Lugar donde se almacenan las dependencias (ej. Maven Central, JCenter, repositorios locales).
*   **Proyecto (Project):** 📂 Representa un componente de software que se va a construir (ej. una aplicación, una biblioteca).  Un proyecto Gradle puede tener múltiples subproyectos.
*   **Ciclo de Vida de la Construcción (Build Lifecycle):** 🔄 Secuencia de fases por las que pasa un proyecto Gradle (inicialización, configuración, ejecución).
*   **Wrapper (Gradle Wrapper):** 📜 Script (`gradlew` en Linux/macOS, `gradlew.bat` en Windows) que permite ejecutar Gradle sin necesidad de instalarlo manualmente.  Asegura que se utiliza la versión correcta de Gradle.
*   **Daemon:** Proceso en segundo plano que acelera las builds.
* **DSL (Domain Specific Language):** Lenguaje específico para configurar los proyectos. Puede ser Groovy o Kotlin.

## 🚀 Sintaxis Básica (Groovy DSL vs. Kotlin DSL)

Gradle ofrece dos DSLs (Domain-Specific Languages) para definir los scripts de construcción: Groovy DSL (tradicional) y Kotlin DSL (más moderno).

**Groovy DSL (`build.gradle`)**

```groovy
plugins {
    id 'java' // Aplicar el plugin de Java
}

repositories {
    mavenCentral() // Usar Maven Central como repositorio
}

dependencies {
    implementation 'com.google.guava:guava:30.1-jre' // Dependencia
    testImplementation 'junit:junit:4.13.2' // Dependencia de prueba
}

tasks.register('miTarea') { // Definir una tarea
    doLast {
        println 'Hola desde miTarea!'
    }
}
```

**Kotlin DSL (`build.gradle.kts`)**

```kotlin
plugins {
    id("java") // Aplicar el plugin de Java
}

repositories {
    mavenCentral() // Usar Maven Central como repositorio
}

dependencies {
    implementation("com.google.guava:guava:30.1-jre") // Dependencia
    testImplementation("junit:junit:4.13.2") // Dependencia de prueba
}

tasks.register("miTarea") { // Definir una tarea
    doLast {
        println("Hola desde miTarea!")
    }
}
```

*   **Kotlin DSL:**  Ofrece mejor autocompletado, refactorización, y comprobación de tipos en el IDE.  Es el DSL recomendado para nuevos proyectos.
*   **Groovy DSL:**  Más tradicional, sintaxis más flexible (pero a veces menos clara).

## ⚙️ Tareas (Tasks)

*   Las tareas son las unidades básicas de trabajo en Gradle.
*   Se definen en el `build.gradle` (o `build.gradle.kts`).

```kotlin
// Kotlin DSL
tasks.register("miTarea") { // Define una tarea llamada "miTarea"
    doLast { // Acción a ejecutar (al final)
        println("Hola desde miTarea!")
    }
}

tasks.register("otraTarea") {
    doFirst { // Acción a ejecutar al principio
        println("Antes de otraTarea")
    }
    doLast {
        println("Después de otraTarea")
    }
}

tasks.register("tareaConDependencia") {
    dependsOn("miTarea") // Depende de que 'miTarea' se ejecute primero
      doLast{
        println("Tarea con dependencia")
    }
}

//Tareas con tipos
tasks.register<Copy>("copiarArchivos") {
    from("src/main/resources")
    into("build/output")
}

// Configurar una tarea existente
tasks.named<Test>("test") {
    useJUnitPlatform() // Usar JUnit 5
}

//Crear una tarea custom
abstract class MyTask : DefaultTask() {
    @get:Input
    abstract val myProperty: Property<String>

    @TaskAction
    fun doSomething() {
        println("Haciendo algo con ${myProperty.get()}")
    }
}

tasks.register<MyTask>("myCustomTask") {
    myProperty.set("Valor")
}
```

```groovy
// Groovy DSL - Equivalente
task miTarea {
    doLast {
        println 'Hola desde miTarea!'
    }
}
```

*   **Ejecutar una tarea:**  `./gradlew miTarea` (o `gradle miTarea` si tienes Gradle instalado).
*   **`doFirst` y `doLast`:**  Bloques para añadir acciones a una tarea.  `doFirst` se ejecuta antes que las acciones principales de la tarea, y `doLast` se ejecuta después.
*   **Dependencias entre tareas (`dependsOn`):**  Una tarea puede depender de otra(s).
* **Tareas por defecto:** Se definen con `defaultTasks 'tarea1', 'tarea2'`
* **Tipos de tareas predefinidas:** `Copy`, `Delete`, `Exec`, `JavaCompile`, `Test`, etc.
* **Tareas incrementales:** Solo se ejecutan si sus entradas o salidas han cambiado.
* **Reglas:** Permiten crear tareas dinámicamente.

## 🧩 Plugins

*   Los plugins añaden funcionalidad a Gradle.
*   Hay plugins para:
    *   Soporte de lenguajes (Java, Kotlin, Groovy, Scala, etc.).
    *   Integración con herramientas (ej. IDEs, servidores de CI).
    *   Tareas específicas (ej. empaquetado, despliegue, análisis de código).

```kotlin
// Kotlin DSL
plugins {
    id("java") // Plugin de Java
    id("org.jetbrains.kotlin.jvm") version "1.9.22" // Plugin de Kotlin
    id("com.android.application") version "8.2.2" apply false // Plugin de Android (sin aplicarlo)
    id("mi.plugin.personalizado") version "1.0" // Plugin personalizado
}
```

```groovy
// Groovy DSL
plugins {
    id 'java'
    id 'org.jetbrains.kotlin.jvm' version '1.9.22'
     id 'com.android.application' version '8.2.2' apply false
}

// Otra forma (más antigua)
apply plugin: 'java'
```

*   **`apply false`:**  Declara el plugin pero no lo aplica (útil en proyectos multi-módulo).
*   **Plugins de la comunidad:**  Disponibles en el Gradle Plugin Portal ([https://plugins.gradle.org/](https://plugins.gradle.org/)).

## 📦 Dependencias

*   Las dependencias son bibliotecas o módulos externos que tu proyecto necesita.
*   Se declaran en el bloque `dependencies`.

```kotlin
// Kotlin DSL
dependencies {
    implementation("com.google.guava:guava:30.1-jre") // Dependencia en tiempo de compilación y ejecución
    testImplementation("junit:junit:4.13.2") // Dependencia solo para pruebas
    compileOnly("javax.servlet:javax.servlet-api:4.0.1") // Dependencia solo en tiempo de compilación
    runtimeOnly("org.postgresql:postgresql:42.3.1")// Dependencia solo en tiempo de ejecución.
    annotationProcessor("com.google.dagger:dagger-compiler:2.40.5") // Procesador de anotaciones

    // Dependencia de un proyecto local
    implementation(project(":mi-modulo"))

    // Dependencia de un archivo local
    implementation(files("libs/mi-libreria.jar"))

    // Dependencia transitiva (se incluyen las dependencias de la dependencia)
     implementation("org.apache.commons:commons-lang3:3.12.0") {
        isTransitive = true // Por defecto es true
    }
    // Excluir dependencias
    implementation("org.apache.commons:commons-collections4:4.4"){
        exclude(group = "commons-logging", module = "commons-logging")
    }
}
```

```groovy
// Groovy DSL - Equivalente
dependencies {
    implementation 'com.google.guava:guava:30.1-jre'
    testImplementation 'junit:junit:4.13.2'
}
```

*   **Configuraciones de dependencias:**
    *   `implementation`:  Dependencia necesaria para compilar y ejecutar el proyecto (y sus dependencias transitivas están disponibles para los consumidores).
    *   `api`: Similar a `implementation` pero expone la dependencia a los consumidores del módulo (como si fuera parte de su API pública).  Usar con moderación.
    *   `compileOnly`:  Dependencia necesaria solo para compilar (no se incluye en el artefacto final).
    *   `runtimeOnly`:  Dependencia necesaria solo para ejecutar (no para compilar).
    *   `testImplementation`:  Dependencia necesaria para compilar y ejecutar las pruebas.
    *   `testCompileOnly`:  Dependencia necesaria solo para compilar las pruebas.
    *   `testRuntimeOnly`: Dependencia necesaria solo para ejecutar pruebas.
    * `annotationProcessor`: Para procesadores de anotaciones.
*   **Notación de dependencias:**  `grupo:nombre:version` (ej. `com.google.guava:guava:30.1-jre`).
*  **Rangos de versiones, versiones dinámicas y exclusiones:**
   ```kotlin
    implementation("com.example:mi-libreria:1.+") // Cualquier versión 1.x
    implementation("com.example:mi-libreria:[1.0,2.0)") // Versión entre 1.0 (incluido) y 2.0 (excluido)
   ```
## 🗄️ Repositorios

*   Los repositorios son lugares donde Gradle busca las dependencias.

```kotlin
// Kotlin DSL
repositories {
    mavenCentral()  // Maven Central (repositorio público)
    google() // Repositorio de Google (para Android)
    mavenLocal() // Repositorio local Maven (~/.m2/repository)
    jcenter() // JCenter (otro repositorio público, ya no recomendado)
    maven {  // Repositorio personalizado
        url = uri("https://mi.repositorio.com/maven")
         credentials { // Si requiere autenticación
            username = "usuario"
            password = "contraseña"
        }
    }
    gradlePluginPortal() //Para plugins
}
```

```groovy
// Groovy DSL - Equivalente
repositories {
    mavenCentral()
    google()
}
```

## 🗂️ Configuración de Proyectos Multi-Módulo

*   Proyectos con múltiples subproyectos (módulos).
*   **`settings.gradle.kts` (o `settings.gradle`):**  Define los subproyectos.

    ```kotlin
    // settings.gradle.kts
    rootProject.name = "mi-proyecto-multimodulo"

    include("modulo1")
    include("modulo2")
    include("libreria:submodulo") // Submódulo anidado
    ```

*   Cada subproyecto tiene su propio `build.gradle.kts` (o `build.gradle`).
*  Se puede acceder a propiedades y métodos del proyecto padre.

## ⚙️ Ciclo de Vida de la Construcción

1.  **Initialization:**  Gradle determina qué proyectos participan en la construcción.  Lee el archivo `settings.gradle.kts` (o `settings.gradle`).
2.  **Configuration:**  Gradle configura los proyectos.  Evalúa los scripts de construcción (`build.gradle.kts` o `build.gradle`) y crea los objetos `Project` y `Task`.
3.  **Execution:**  Gradle ejecuta las tareas seleccionadas.

## ✨ Otros Conceptos y Características

*   **Gradle Wrapper:**  Permite ejecutar Gradle sin necesidad de instalarlo.  Se usa con `./gradlew` (Linux/macOS) o `gradlew.bat` (Windows).
*   **`gradle.properties`:**  Archivo para definir propiedades del proyecto (ej. versiones de dependencias).
*   **`local.properties`:** Archivo para propiedades específicas del entorno local (normalmente no se versiona).
*   **Perfiles de construcción (Build Profiles):**  Permiten definir diferentes configuraciones para diferentes entornos (ej. desarrollo, producción).
*   **Inicialización de Gradle (init scripts):**  Scripts que se ejecutan antes de que se evalúe el proyecto.
* **Testing:** Gradle tiene soporte integrado para testing.
* **Continuous Integration:** Gradle se integra con servidores de CI.
* **Variables de entorno:** Se pueden usar variables.
* **Línea de comandos:** Gradle ofrece muchas opciones.

Este cheatsheet cubre los aspectos más importantes de Gradle. ¡Guárdalo y consúltalo a menudo!