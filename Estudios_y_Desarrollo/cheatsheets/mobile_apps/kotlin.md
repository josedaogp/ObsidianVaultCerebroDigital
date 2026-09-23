# ⚡ Cheatsheet de Kotlin

Este cheatsheet cubre los aspectos esenciales de Kotlin, el lenguaje de programación moderno, conciso y seguro de JetBrains, que se ejecuta en la JVM (y otras plataformas).

## 🔍 Conceptos Básicos

*   **Kotlin Playground:** 📝 Entorno de desarrollo online para Kotlin: [https://play.kotlinlang.org/](https://play.kotlinlang.org/)
*   **SDK de Kotlin:** 📦 Incluye el compilador de Kotlin, bibliotecas, y otras herramientas.
*   **`kotlinc` (compilador):** ⚙️ Compila código Kotlin a bytecode de la JVM (archivos `.class` o `.jar`).
*   **`kotlin` (intérprete):** ▶️ Ejecuta código Kotlin directamente (REPL - Read-Eval-Print Loop).
*   **`kotlin-jvm`, `kotlin-js`, `kotlin-native`:** 💻 Diferentes *targets* de compilación (JVM, JavaScript, nativo).
*   **`build.gradle.kts` (Gradle) / `pom.xml` (Maven):** 📄 Archivos de configuración de proyectos Kotlin (gestión de dependencias, compilación, etc.).
*   **Comentarios:**
    *   `// Comentario de una línea`
    *   `/* Comentario de bloque */`
    *   `/** Comentario de documentación (KDoc) */`

## 📦 Variables y Tipos de Datos

1.  **Declaración de variables:**

    ```kotlin
    var nombre = "Juan"  // Variable mutable (puede cambiar) - Inferencia de tipo (String)
    val apellido = "Pérez"  // Variable inmutable (no puede cambiar) - Similar a final en Java
    val edad: Int = 30   // Tipo explícito
    lateinit var direccion: String // Inicialización tardía (solo var)
    ```

2.  **Tipos de datos básicos:**

    *   **`Int`:** Enteros (32 bits).
    *   **`Long`:** Enteros largos (64 bits).
    *   **`Short`:** Enteros cortos (16 bits).
    *   **`Byte`:** Enteros muy pequeños (8 bits).
    *   **`Double`:** Números de punto flotante (doble precisión - 64 bits).
    *   **`Float`:** Números de punto flotante (precisión simple - 32 bits).
    *   **`Boolean`:** `true` o `false`.
    *   **`String`:** Cadenas de texto.
    *   **`Char`:** Caracteres individuales.
    * **`Array`**: Arrays
    * **`Any`**: Supertipo de todos los tipos.
    * **`Unit`**: Tipo que representa "ningún valor" (similar a `void` en Java).
    * **`Nothing`**: Tipo que representa "ningún valor posible" (para funciones que nunca retornan).

    ```kotlin
    val edad = 30  // Int
    val precio = 99.99 // Double
    val letra = 'A'  // Char
    val nombre = "Ana"  // String
    val esValido = true  // Boolean
    val hexadecimal = 0x1A // Int (hexadecimal)
    val binario = 0b1010 // Int (binario)
    val largo = 100_000_000L // Long (con separador de miles)
    val flotante = 3.14f // Float
    ```

3.  **Nullable types (tipos anulables):**  Por defecto, las variables no pueden ser `null`.  Para permitir `null`, se usa el operador `?`.

    ```kotlin
    var nombre: String? = null  // 'nombre' puede ser null
    val longitud = nombre?.length  // Acceso seguro (devuelve null si nombre es null)
    val longitud2 = nombre?.length ?: 0  // Operador Elvis (si es null, usa 0)
    val longitud3 = nombre!!.length // !! (lanza NullPointerException si nombre es null)
    ```

4. **Smart Casts (conversiones inteligentes):**
    ```kotlin
        val x: Any = "Hola"
        if (x is String) {
            println(x.length) // x es automáticamente convertido a String
        }
    ```
5. **Literales:**
    * `42` (Int)
    * `3.14` (Double)
    * `'A'` (Char)
    * `"Hola"` (String)
    * `true` (Boolean)
    * `null`

## ➕ Operadores

1.  **Aritméticos:** `+`, `-`, `*`, `/`, `%`.
2.  **De asignación:** `=`, `+=`, `-=`, `*=`, `/=`, `%=`.
3.  **Incremento y decremento:** `++`, `--` (pre y post).
4.  **De comparación:** `==` (igualdad estructural), `!=`, `>` , `<`, `>=`, `<=`, `===` (igualdad referencial), `!==`.
5.  **Lógicos:** `&&` (AND), `||` (OR), `!` (NOT).
6. **Rangos**
    * `..`: Crea un rango inclusivo.
    * `until`: Crea un rango exclusivo.
    * `downTo`: Crea un rango descendente.
    * `step`: Especifica el incremento.
    ```kotlin
      for (i in 1..5) print(i) // 12345
      for (i in 5 downTo 1) print(i) // 54321
      for (i in 1..10 step 2) print(i) // 13579
      for (i in 1 until 5) print(i) // 1234 (no incluye 5)
    ```
7. **Operador `in` y `!in`:** Para comprobar si un valor está dentro de un rango o colección.
8. **Operador `is` y `!is`:** Para comprobar el tipo.
9. **Operadores bitwise** (solo para Int y Long): `shl`, `shr`, `ushr`, `and`, `or`, `xor`, `inv`.

## 📝 Estructuras de Control

1.  **`if...else`:**

    ```kotlin
    if (condicion) {
      // ...
    } else if (otraCondicion) {
      // ...
    } else {
      // ...
    }

    // if como expresión (devuelve un valor)
    val mayor = if (a > b) a else b
    ```

2.  **`when`:** (Similar a `switch` en otros lenguajes, pero más potente)

    ```kotlin
    when (variable) {
      valor1 -> { /* ... */ }
      valor2, valor3 -> { /* ... */ }  // Múltiples valores
      in rango -> { /* ... */ }  // Comprobar si está en un rango
      is Tipo -> { /* ... */ }  // Comprobar el tipo
      else -> { /* ... */ }
    }

    // when sin argumento (como if-else if)
     when {
         x.isOdd() -> print("x is odd")
         else -> print("x is even")
     }

     // when como expresión
     val resultado = when (x) {
         1 -> "Uno"
         2 -> "Dos"
         else -> "Otro"
     }
    ```

3.  **`for`:**

    ```kotlin
    for (elemento in coleccion) {
      // ...
    }

    for (i in 1..5) { // Rango inclusivo
      print(i) // 12345
    }

    for (i in 5 downTo 1) {
      print(i)
    }

    for (i in 1 until 5) { // Rango exclusivo (no incluye 5)
        print(i)
    }

    for (i in 1..10 step 2) {
      print(i)
    }
    for ((index, value) in array.withIndex()){} // Con índice
    ```

4.  **`while`:**

    ```kotlin
    while (condicion) {
      // ...
    }
    ```

5.  **`do...while`:**

    ```kotlin
    do {
      // ...
    } while (condicion)
    ```

6.  **`break` y `continue`:**
7. **`return`**

## ⚙️ Funciones

1.  **Declaración de función:**

    ```kotlin
    // Función con tipo de retorno y parámetros
    fun sumar(a: Int, b: Int): Int {
      return a + b
    }

    // Función con cuerpo de expresión (retorno inferido)
    fun multiplicar(a: Int, b: Int) = a * b

    // Función sin tipo de retorno (Unit implícito)
    fun saludar(nombre: String) {
      println("Hola, $nombre!")
    }

    // Parámetros con valores por defecto
    fun saludar(nombre: String, saludo: String = "Hola") {
      println("$saludo, $nombre!")
    }

    // Parámetros nombrados
     saludar(saludo = "Buenos días", nombre = "Ana")

    ```

2.  **Funciones de una sola línea (single-expression functions):**

    ```kotlin
    fun cuadrado(x: Int): Int = x * x
    ```

3.  **Funciones vararg (número variable de argumentos):**

    ```kotlin
    fun imprimirNombres(vararg nombres: String) {
      for (nombre in nombres) {
        println(nombre)
      }
    }
    imprimirNombres("Juan", "Ana", "Pedro")
    ```

4.  **Funciones de extensión:**  Añaden funcionalidad a clases existentes sin herencia.

    ```kotlin
    fun String.aMayusculas(): String {
      return this.uppercase()
    }

    fun main() {
      println("hola".aMayusculas())  // HOLA
    }
    ```

5.  **Funciones infix:**  Se pueden llamar sin punto ni paréntesis (para un solo parámetro).

    ```kotlin
    infix fun Int.sumarCon(otro: Int): Int {
      return this + otro
    }

    fun main() {
      val resultado = 5 sumarCon 3 // Equivalente a 5.sumarCon(3)
      println(resultado) // 8
    }
    ```

6.  **Funciones de orden superior:**  Toman funciones como parámetros o devuelven funciones.

7. **Lambdas (funciones anónimas):**
    ```kotlin
    val suma = { a: Int, b: Int -> a + b }
    println(suma(2, 3))
    ```

8. **Funciones locales:** Funciones dentro de otras funciones.

## 📦 Clases y Objetos

1.  **Declaración de clase:**

    ```kotlin
    class Persona(val nombre: String, var edad: Int) { // Constructor primario
      // Propiedades (nombre es val, edad es var)

      // Bloque init (se ejecuta al crear la instancia)
      init {
        println("Persona creada: $nombre")
      }

      // Método
      fun saludar() {
        println("Hola, soy $nombre y tengo $edad años.")
      }

      // Segundo constructor
      constructor(nombre: String) : this(nombre, 0) { // Llama al constructor primario

      }
    }
    ```

2.  **Creación de objetos:**

    ```kotlin
    val persona = Persona("Juan", 30)
    persona.saludar()
    ```

3.  **Propiedades (getters y setters implícitos):**  Kotlin genera automáticamente getters y setters para las propiedades `var`.  Para `val`, solo genera el getter.

    ```kotlin
        var edad: Int = 30
        get() = field // Getter implícito
        set(value) {  // Setter implícito
            if(value >= 0) {
                field = value // "field" es la variable de respaldo
            }
        }
    ```

4.  **Herencia:**

    ```kotlin
    open class Animal(val nombre: String) {  // 'open' permite que la clase sea heredada
      open fun hacerSonido() {  // 'open' permite que el método sea sobrescrito
        println("Sonido genérico")
      }
    }

    class Perro(nombre: String, val raza: String) : Animal(nombre) {

      override fun hacerSonido() {  // Sobrescribe el método
        println("Guau! Soy un perro de raza $raza")
      }
    }
    ```

5.  **Clases abstractas:**

    ```kotlin
    abstract class Figura {
      abstract fun calcularArea(): Double
    }

    class Circulo(val radio: Double) : Figura() {
      override fun calcularArea(): Double {
        return Math.PI * radio * radio
      }
    }
    ```

6.  **Interfaces:**

    ```kotlin
    interface Volador {
      fun volar()  // Método abstracto (sin implementación)
      val velocidadMaxima: Int // Propiedad abstracta
        get() = 100 // Con implementación por defecto.
    }

    class Pato : Volador {
      override val velocidadMaxima = 50
      override fun volar() {
        println("Pato volando...")
      }
    }
    ```

7.  **Data classes:**  Clases diseñadas principalmente para almacenar datos.  Generan automáticamente `equals()`, `hashCode()`, `toString()`, `copy()`, y *componentN* functions (para destructuring).

    ```kotlin
    data class Usuario(val nombre: String, val edad: Int)

    val usuario1 = Usuario("Juan", 30)
    val usuario2 = usuario1.copy(edad = 31) // Copia con modificación
    println(usuario1)  // Imprime una representación legible
    val (nombre, edad) = usuario1 // Destructuring
    ```

8.  **Enum classes:**

    ```kotlin
    enum class Color {
      ROJO, VERDE, AZUL
    }
    // Con propiedades y métodos
      enum class DiaSemana(val esFinDeSemana: Boolean) {
        LUNES(false),
        MARTES(false),
        MIERCOLES(false),
        JUEVES(false),
        VIERNES(false),
        SABADO(true),
        DOMINGO(true);

        fun mensaje(): String {
            return if (esFinDeSemana) "¡Es fin de semana!" else "A trabajar..."
        }
    }
    ```

9.  **Sealed classes:**  Representan jerarquías de clases restringidas (todos los subtipos deben estar definidos en el mismo archivo).

    ```kotlin
    sealed class Resultado {
      data class Exito(val valor: String) : Resultado()
      data class Error(val mensaje: String) : Resultado()
      object Cargando : Resultado() // Singleton
    }
    ```

10. **Object declarations (singletons):**  Define una clase con una sola instancia.

    ```kotlin
    object Configuracion {
      val version = "1.0"
      fun mostrarVersion() {
        println("Versión: $version")
      }
    }
    ```

11. **Companion objects:**  Similares a miembros estáticos en Java.

    ```kotlin
    class MiClase {
      companion object {
        val constante = "Valor"
        fun metodoEstatico() { /* ... */ }
      }
    }

    MiClase.metodoEstatico()
    ```

12. **Extension functions (funciones de extensión):**

13. **Extension properties (propiedades de extensión):**

14. **Nested classes:** Clases dentro de otras clases.

15. **Inner classes:** Clases anidadas que tienen acceso a los miembros de la clase externa.  Se declaran con `inner`.

16. **Delegation:** Permite delegar la implementación de una interfaz a otro objeto.

17. **Delegated properties:** Permiten delegar la lógica de get/set de una propiedad a otro objeto.

18. **Visibility modifiers (modificadores de visibilidad):**
    * `public`: Visible en todas partes (por defecto).
    * `private`: Visible solo dentro de la clase o archivo.
    * `protected`: Visible dentro de la clase y sus subclases.
    * `internal`: Visible dentro del mismo módulo.

## 🧺 Colecciones

1.  **Listas (List):**

    ```kotlin
    val numeros = listOf(1, 2, 3, 4, 5)  // Lista inmutable
    val frutas = mutableListOf("manzana", "plátano")  // Lista mutable

    println(numeros[0])  // Acceder a un elemento por índice
    println(numeros.size)
    println(numeros.first())
    println(numeros.last())
    println(numeros.isEmpty())
    println(numeros.contains(3))
    println(numeros.indexOf(4))

    frutas.add("naranja")
    frutas.removeAt(0)
    ```

2.  **Conjuntos (Set):**

    ```kotlin
    val miSet = setOf(1, 2, 3, 2, 1)  // {1, 2, 3} - Conjunto inmutable (elementos únicos)
    val otroSet = mutableSetOf("rojo", "verde")  // Conjunto mutable

    println(miSet.size)
    println(miSet.contains(3))
    ```

3.  **Mapas (Map):**

    ```kotlin
    val miMapa = mapOf(  // Mapa inmutable
      "nombre" to "Juan",
      "edad" to 30
    )

    val otroMapa = mutableMapOf(  // Mapa mutable
      "ciudad" to "Madrid"
    )

    println(miMapa["nombre"])
    println(miMapa.size)
    println(miMapa.keys)
    println(miMapa.values)
    println(miMapa.containsKey("edad"))

    otroMapa["pais"] = "España" // Agregar o modificar
    otroMapa.remove("ciudad")
    ```
4. **Operaciones con colecciones (estilo funcional):**

   ```kotlin
   val numeros = listOf(1, 2, 3, 4, 5)
   val cuadrados = numeros.map { it * it } // Transformar (it es el elemento actual)
   val pares = numeros.filter { it % 2 == 0 } // Filtrar
   val suma = numeros.reduce { acc, i -> acc + i } // Reducir
   val existePar = numeros.any { it % 2 == 0 }  // Comprobar si existe algún elemento que cumpla la condición
   val todosPares = numeros.all { it % 2 == 0 }  // Comprobar si todos los elementos cumplen la condición
   val primeroPar = numeros.find { it % 2 == 0 } //Encontrar el primer elemento
   val agrupados = numeros.groupBy {if (it%2 == 0) "Par" else "Impar"}
   val aplanados = listOf(listOf(1,2), listOf(3,4)).flatten()
   val ordenados = numeros.sorted() //Ordenar
   numeros.forEach{ println(it)} //Iterar
   ```

## 🧵 Corrutinas (Coroutines) - Programación Asíncrona

*   Permiten escribir código asíncrono de forma secuencial (más legible).
*   Son *ligeras* (no bloquean hilos).
*   Se necesita la dependencia `kotlinx-coroutines-core`.

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking { // Bloquea el hilo principal hasta que todas las corrutinas terminan
  launch { // Inicia una nueva corrutina
    delay(1000)  // Espera no bloqueante (simula una operación larga)
    println("Mundo!")
  }
  println("Hola,") // Se ejecuta antes que "Mundo!"
  //delay(2000) // Para que de tiempo a ejecutarse
}
```

*   **`launch`:**  Inicia una corrutina y no devuelve un resultado.
*   **`async`:**  Inicia una corrutina y devuelve un `Deferred` (un resultado futuro).
*   **`runBlocking`:**  Bloquea el hilo actual hasta que la corrutina termina (usar principalmente en `main` y pruebas).
*   **`delay`:**  Espera (suspende la corrutina) sin bloquear el hilo.
*   **`withContext`:**  Cambia el contexto de ejecución de la corrutina (ej. `Dispatchers.IO` para operaciones de E/S).
*  **`CoroutineScope`**: Define el alcance de las corrutinas.
* **`Dispatchers`**: Controlan en qué hilo se ejecutan.
    *  `Dispatchers.Default`: Para tareas intensivas en CPU.
    *  `Dispatchers.IO`: Para operaciones de entrada/salida.
    * `Dispatchers.Main`: Para el hilo principal (UI).
    * `Dispatchers.Unconfined`: Sin un hilo específico.

```kotlin
    //Ejemplo con async/await
     val deferredResult = async {
        delay(1000)
        "Resultado"
    }
    val result = deferredResult.await() // Espera el resultado

    //Ejemplo con withContext
    val result = withContext(Dispatchers.IO) {
        // Operaciones de E/S
        "Resultado"
    }
```

## ✨ Otros Conceptos y Características

*   **Expresiones `try...catch...finally`:**

    ```kotlin
    try {
      // ...
    } catch (e: Exception) {
      // ...
    } finally {
      // ...
    }

    // try como expresión
    val resultado = try {
        // ...
    } catch (e: Exception) {
        null
    }
    ```

*   **Paquetes e importaciones:**

    ```kotlin
    package mi.paquete

    import kotlin.math.sqrt  // Importar una función específica
    import kotlin.math.*  // Importar todo

    // Alias
    import kotlin.collections.List as KList
    ```
* **Strings:**
    *  `"""` (raw strings): Para cadenas multilínea y sin escape de caracteres.
    *  Interpolación de strings: `$variable` o `${expresion}`.
*   **Smart casts:**  El compilador de Kotlin rastrea las comprobaciones de tipo y realiza conversiones automáticas cuando es seguro.
* **Destructuring declarations:** Permite extraer múltiples valores de un objeto.

    ```kotlin
        val (nombre, edad) = persona
    ```

* **Type aliases:** Permite crear alias para tipos.
* **Inline classes:** Clases que envuelven un valor, para mejorar el rendimiento y la seguridad de tipos.

Este cheatsheet cubre una amplia gama de características de Kotlin, desde lo más básico hasta conceptos más avanzados. ¡Guárdalo y consúltalo a menudo!