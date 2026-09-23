# 🐦 Cheatsheet de Swift

Este cheatsheet cubre los aspectos esenciales de Swift, el lenguaje de programación de Apple para iOS, macOS, watchOS, tvOS, y más.

## 🔍 Conceptos Básicos

*   **Xcode:** 🛠️ Entorno de desarrollo integrado (IDE) de Apple para crear aplicaciones para plataformas Apple.  Incluye el compilador de Swift, editor de código, simuladores, y otras herramientas.
*   **Playgrounds:** 📝 Entorno interactivo en Xcode para experimentar con código Swift y ver los resultados en tiempo real.
*   **Swift Package Manager (SPM):** 📦 Herramienta para gestionar dependencias en Swift (similar a npm en Node.js o CocoaPods).
*   **Comentarios:**
    *   `// Comentario de una línea`
    *   `/* Comentario de bloque */`
    *   `/// Comentario de documentación (Markdown)`

## 📦 Variables y Tipos de Datos

1.  **Declaración de variables:**

    ```swift
    var nombre = "Juan"  // Variable mutable (puede cambiar) - Inferencia de tipo (String)
    let apellido = "Pérez"  // Constante (no puede cambiar)
    var edad: Int = 30   // Tipo explícito
    ```

2.  **Tipos de datos básicos:**

    *   **`Int`:** Enteros (el tamaño depende de la plataforma, 32 o 64 bits).  También: `Int8`, `Int16`, `Int32`, `Int64`, `UInt`, `UInt8`, `UInt16`, `UInt32`, `UInt64`.
        ```swift
        let edad = 30  // Int
        let maximo = Int.max
        ```

    *   **`Double`:** Números de punto flotante (doble precisión - 64 bits).
    *   **`Float`:** Números de punto flotante (precisión simple - 32 bits).
        ```swift
        let precio = 99.99 // Double
        let pi: Float = 3.14159
        ```

    *   **`Bool`:** Booleanos (`true` o `false`).
        ```swift
        let esValido = true
        ```

    *   **`String`:** Cadenas de texto.
        ```swift
        let nombre = "Ana"
        let mensaje = "Hola, mundo!"
        let multilinea = """
          Este es un string
          multilinea.
        """ // Multilínea
        let interpolacion = "Mi nombre es \(nombre)" // Interpolación
        let concatenacion = "Hola" + " " + "Mundo"
        ```

    *   **`Character`:** Caracteres individuales.
        ```swift
        let letra: Character = "A"
        ```
    * **`Optional`:** Representa un valor que puede ser o no ser `nil`.
        ```swift
        var posibleEntero: Int? = 42
        posibleEntero = nil // Ok
        ```

3.  **Type Aliases:**

    ```swift
    typealias Coordenada = (Int, Int)
    let punto: Coordenada = (3, 5)
    ```
4. **Literales:**
    * `42` (Int)
    * `3.14` (Double)
    * `"Hola"` (String)
    * `true` (Bool)
    * `nil` (Optional)
    * `[1, 2, 3]` (Array)
    * `["a": 1, "b": 2]` (Dictionary)

## ➕ Operadores

1.  **Aritméticos:** `+`, `-`, `*`, `/`, `%`.
2.  **De asignación:** `=`, `+=`, `-=`, `*=`, `/=`, `%=`.
3.  **De comparación:** `==`, `!=`, `>`, `<`, `>=`, `<=`.
4.  **Lógicos:** `&&` (AND), `||` (OR), `!` (NOT).
5.  **De rango:**
    *   `...`: Rango cerrado (ej. `1...5` incluye 1, 2, 3, 4 y 5).
    *   `..<`: Rango semiabierto (ej. `1..<5` incluye 1, 2, 3 y 4).
6.  **Ternario:** `condicion ? valorSiTrue : valorSiFalse`.
7.  **Nil-Coalescing Operator (`??`):**  Desenvuelve un opcional o proporciona un valor por defecto.

    ```swift
    let nombre: String? = nil
    let nombreCompleto = nombre ?? "Invitado"  // "Invitado"
    ```

8.  **Operadores de identidad (`===` y `!==`):**  Comprueban si dos referencias apuntan al mismo objeto (solo para instancias de clases).
9. **Operador de un lado (`a...`, `...a`):** Para rangos
10. **Operadores bitwise:** `&`, `|`, `^`, `~`, `<<`, `>>`.

## 📝 Estructuras de Control

1.  **`if...else`:**

    ```swift
    if condicion {
      // ...
    } else if otraCondicion {
      // ...
    } else {
      // ...
    }
    ```

2.  **`switch`:**

    ```swift
    switch variable {
    case valor1:
      // ...
    case valor2, valor3:  // Múltiples valores
      // ...
    case let x where x > 10: // Con condición where
        //...
    case 0..<10: // Rangos
        // ...
    default:
      // ...
    }
    ```
    *   No hay *fallthrough* implícito (no es necesario `break`).
    *   Debe ser exhaustivo (cubrir todos los casos posibles, o usar `default`).

3.  **`for...in`:**

    ```swift
    for i in 1...5 {  // Rango cerrado
      print(i)
    }

    for i in 1..<5 { // Rango semiabierto
        print(i)
    }

    let numeros = [1, 2, 3]
    for numero in numeros {
      print(numero)
    }

    for (indice, valor) in numeros.enumerated() { // Con índice
        print("Índice: \(indice), Valor: \(valor)")
    }
    for _ in 1...3 { // Ignorar el valor
       print("Hola")
    }

    let diccionario = ["a": 1, "b": 2]
    for (clave, valor) in diccionario{}
    ```

4.  **`while`:**

    ```swift
    while condicion {
      // ...
    }
    ```

5.  **`repeat...while`:** (Similar a `do...while` en otros lenguajes)

    ```swift
    repeat {
      // ...
    } while condicion
    ```

6.  **`guard`:**  Salida temprana de una función/método si no se cumplen ciertas condiciones.

    ```swift
    func miFuncion(valor: Int?) {
      guard let valorSeguro = valor, valorSeguro > 0 else {
        print("Valor no válido")
        return // Debe salir
      }
      // 'valorSeguro' está disponible aquí
      print("Valor seguro: \(valorSeguro)")
    }
    ```

7.  **`defer`:**  Ejecuta código justo antes de que la ejecución salga del ámbito actual.

    ```swift
    func miFuncion() {
      defer {
        print("Esto se ejecuta al final (siempre)")
      }
      print("Haciendo algo...")
      // ...
    }
    ```
8. **`break`, `continue`, `fallthrough` (solo en `switch`)**

## ⚙️ Funciones

1.  **Declaración de función:**

    ```swift
    // Función con tipo de retorno y parámetros
    func sumar(a: Int, b: Int) -> Int {
      return a + b
    }

    // Función sin tipo de retorno (Void implícito)
    func saludar(nombre: String) {
      print("Hola, \(nombre)!")
    }

    // Función sin parámetros
       func decirHola() {
           print("Hola")
       }
    //Función con valor de retorno
    func obtenerSaludo() -> String {
        return "Hola"
    }
    ```

2.  **Parámetros y valores de retorno:**

    *   **Parámetros con nombres externos e internos:**

        ```swift
        func saludar(a persona: String, desde ciudad: String) {
          print("Hola, \(persona), desde \(ciudad)!")
        }
        saludar(a: "Juan", desde: "Madrid") // Usar nombres externos

        //Omitir nombre externo
        func sumar(_ a: Int, _ b: Int) -> Int {
           return a + b
        }
        ```

    *   **Parámetros con valores por defecto:**

        ```swift
        func saludar(nombre: String, saludo: String = "Hola") {
          print("\(saludo), \(nombre)!")
        }
        ```

    *   **Parámetros variádicos (varargs):**

        ```swift
        func sumar(numeros: Int...) -> Int {
          var total = 0
          for numero in numeros {
            total += numero
          }
          return total
        }
        ```

    *   **Parámetros inout:**  Permiten modificar el valor de un parámetro dentro de la función (paso por referencia).

        ```swift
        func intercambiar(_ a: inout Int, _ b: inout Int) {
          let temp = a
          a = b
          b = temp
        }

        var x = 5
        var y = 10
        intercambiar(&x, &y) // Pasar con &
        print(x, y) // 10, 5
        ```

    *   **Tuplas como valor de retorno (múltiples valores):**

        ```swift
        func minMax(array: [Int]) -> (min: Int, max: Int)? {
          if array.isEmpty { return nil }
          var currentMin = array[0]
          var currentMax = array[0]
          for value in array {
            if value < currentMin {
              currentMin = value
            } else if value > currentMax {
              currentMax = value
            }
          }
          return (currentMin, currentMax)
        }
        ```

3.  **Tipos de función:**  Las funciones son tipos de primera clase.

    ```swift
    var miFuncion: (Int, Int) -> Int = sumar
    func operar(a: Int, b: Int, operacion: (Int, Int) -> Int) -> Int {
        return operacion(a, b)
    }
    ```

4.  **Closures (bloques de código autocontenidos, similares a lambdas):**

    ```swift
    let multiplicar: (Int, Int) -> Int = { (a, b) in
      return a * b
    }
    // Sintaxis simplificada
    let dividir: (Int, Int) -> Int = { $0 / $1 } // $0, $1, etc. son los argumentos

    // Closures como parámetros (trailing closure syntax)
      numeros.sorted { $0 < $1 }
      numeros.sorted(by: {a, b in return a < b})
    ```
    * **Capturing values:** Los closures pueden "capturar" variables del contexto circundante.
    * `@escaping`: Se usa en parámetros de closures que se ejecutan *después* de que la función retorna.

## 📦 Clases y Estructuras

*   **Clases (Classes):**  Tipos por *referencia*.  Soportan herencia.
*   **Estructuras (Structures):**  Tipos por *valor*.  No soportan herencia (pero sí protocolos).

1.  **Definición:**

    ```swift
    // Clase
    class Persona {
      var nombre: String
      var edad: Int

      // Inicializador (constructor)
      init(nombre: String, edad: Int) {
        self.nombre = nombre
        self.edad = edad
      }

      // Método
      func saludar() {
        print("Hola, soy \(nombre)")
      }

        deinit { // Desinicializador (solo clases)
            print("Persona \(nombre) eliminada")
        }
    }

    // Estructura
    struct Punto {
      var x: Double
      var y: Double

      // Inicializador generado automáticamente (memberwise initializer)

      // Método (las estructuras pueden tener métodos)
      func distanciaAlOrigen() -> Double {
        return (x * x + y * y).squareRoot()
      }

      // Métodos que modifican la estructura deben ser 'mutating'
        mutating func mover(dx: Double, dy: Double) {
            x += dx
            y += dy
        }
    }
    ```

2.  **Instanciación:**

    ```swift
    let persona = Persona(nombre: "Ana", edad: 25)
    let punto = Punto(x: 3.0, y: 4.0)
    ```

3.  **Propiedades:**

    *   **Stored properties:**  Almacenan valores.
    *   **Computed properties:**  Calculan valores (getters y setters).
    *   **Property observers:**  `willSet` y `didSet` (se ejecutan antes y después de que una propiedad cambia).
    *   **Type properties (propiedades de tipo):**  Propiedades asociadas a la clase/estructura en sí, no a las instancias (similares a propiedades estáticas).  Se definen con `static` (para clases y estructuras) o `class` (solo para clases, y permite *override* en subclases).
    * **Lazy Stored Properties:** Se inicializan la primera vez que se usan.

    ```swift
        var radio: Double = 0 {
            willSet { // Antes de cambiar el valor
                print("Nuevo valor: \(newValue)")
            }
            didSet { // Después de cambiar el valor
              print("Valor antiguo: \(oldValue)")
            }
        }

         var diametro: Double { // Computed property
            get {
                return radio * 2
            }
            set {
                radio = newValue / 2
            }
        }
    ```

4.  **Métodos:**

    *   **Instance methods:**  Métodos asociados a instancias.
    *   **Type methods (métodos de tipo):**  Métodos asociados a la clase/estructura en sí.  Se definen con `static` (o `class` en clases).
    *   **Mutating methods (solo en estructuras y enumeraciones):**  Métodos que modifican la instancia.

5.  **Herencia (solo clases):**

    ```swift
    class Empleado: Persona { // Hereda de Persona
      var puesto: String

      // Inicializador
      init(nombre: String, edad: Int, puesto: String) {
        self.puesto = puesto
        super.init(nombre: nombre, edad: edad) // Llama al inicializador de la clase base
      }

      // Sobrescribir un método
      override func saludar() {
        super.saludar() // Llama al método de la clase base (opcional)
        print("Soy un empleado con puesto \(puesto)")
      }
    }
    ```

    *   `override`:  Se usa para sobrescribir métodos, propiedades e inicializadores.
    *   `final`:  Evita que una clase sea heredada, o que un método/propiedad sea sobrescrito.
    *   `super`:  Accede a miembros de la clase base.

6. **Inicializadores:**
    * **Designated initializers:** Inicializadores principales.
    * **Convenience initializers:** Inicializadores secundarios que llaman a un inicializador designado.
    * **Failable initializers:** Inicializadores que pueden fallar (devuelven `nil`). Se definen con `init?`.
    * **Required initializers:** Inicializadores que deben ser implementados por todas las subclases.
    * **Inicialización en dos fases:** Swift tiene reglas estrictas para inicializar propiedades.

7. **Desinicialización (deinit):** Solo para clases. Se llama justo antes de que una instancia se desasigna de memoria.

## 📦 Enumeraciones (Enums)

```swift
enum Color {
  case rojo
  case verde
  case azul
}

// Con valores asociados
enum Resultado {
  case exito(String)
  case error(Int, String) // Código de error y mensaje
}

// Con valores raw (deben ser únicos y del mismo tipo)
enum DiaSemana: Int {
  case lunes = 1
  case martes, miercoles, jueves, viernes, sabado, domingo
}

// Con métodos y propiedades
enum Operacion {
    case suma, resta, multiplicacion, division

    func calcular(a: Double, b: Double) -> Double {
        switch self {
        case .suma:
            return a + b
        case .resta:
            return a - b
        case .multiplicacion:
            return a * b
        case .division:
            return a / b
        }
    }
}

// Enumeraciones recursivas (indirect)
indirect enum ExpresionAritmetica {
    case numero(Int)
    case suma(ExpresionAritmetica, ExpresionAritmetica)
}

var color = Color.rojo
```

*   Los casos de enumeración son valores en sí mismos (no son enteros por defecto como en C/C++).
*   Pueden tener valores asociados (de diferentes tipos para cada caso).
*   Pueden tener valores *raw* (del mismo tipo para todos los casos).
*   Pueden tener métodos y propiedades (computed).

## 📝 Protocolos

*   Definen un *contrato* de métodos, propiedades, y otros requisitos que un tipo (clase, estructura, enumeración) debe cumplir.
*   Similares a interfaces en otros lenguajes.

```swift
protocol Volador {
  var velocidadMaxima: Int { get }  // Propiedad (solo getter)
  func volar()  // Método
}

// Conformar a un protocolo (clase, estructura o enumeración)
struct Avion: Volador {
  var velocidadMaxima: Int = 800

  func volar() {
    print("Avión volando a \(velocidadMaxima) km/h")
  }
}

// Protocolos como tipos
func despegar(objeto: Volador) {
  objeto.volar()
}
```

*   Un tipo puede conformar a múltiples protocolos.
*   **Protocol extensions:**  Proporcionan implementaciones por defecto de métodos y propiedades de un protocolo.
*  **Protocol composition:** Combina varios protocolos. `Protocolo1 & Protocolo2`
*  **`Self`:** Se refiere al tipo que conforma al protocolo.
* **Optional Protocol Requirements**: Se definen con `@objc`.
* **Protocol Associated Types:** Permite definir tipos genéricos dentro de un protocolo.

## ➕ Extensiones

*   Añaden funcionalidad a clases, estructuras, enumeraciones o protocolos existentes (incluso de bibliotecas externas).
*   No pueden añadir *stored properties*, pero sí *computed properties*.

```swift
extension Int {
  func cuadrado() -> Int {
    return self * self
  }

  var esPar: Bool { // Computed property
    return self % 2 == 0
  }
}

let numero = 5
print(numero.cuadrado())  // 25
print(10.esPar)  // true
```

## ⚙️ Genéricos

*   Permiten escribir código que funciona con diferentes tipos de datos, de forma segura.

```swift
func intercambiar<T>(_ a: inout T, _ b: inout T) {  // T es un tipo genérico
  let temp = a
  a = b
  b = temp
}

var x = 5
var y = 10
intercambiar(&x, &y)

var s1 = "Hola"
var s2 = "Mundo"
intercambiar(&s1, &s2)

// Estructura genérica
struct Pila<Elemento> {
  var elementos: [Elemento] = []

  mutating func push(_ elemento: Elemento) {
    elementos.append(elemento)
  }

  mutating func pop() -> Elemento? {
    return elementos.popLast()
  }
}

var pilaEnteros = Pila<Int>()
```

*   **Type constraints (restricciones de tipo):**  Especifican requisitos para los tipos genéricos (ej. que conformen a un protocolo).

    ```swift
    func encontrarIndice<T: Equatable>(de valor: T, en array: [T]) -> Int? {
        // ...
    }
    // T debe conformar al protocolo Equatable (para poder usar ==)
    ```

## ❌ Manejo de Errores

1.  **`Error` protocol:**  Representa un error que se puede lanzar.  Normalmente se usan enumeraciones.

    ```swift
    enum ErrorDivision: Error {
      case divisionPorCero
    }
    ```

2.  **`throw`:**  Lanza un error.

    ```swift
    func dividir(a: Int, b: Int) throws -> Int {
      if b == 0 {
        throw ErrorDivision.divisionPorCero
      }
      return a / b
    }
    ```

3.  **`do...catch`:**  Maneja errores.

    ```swift
    do {
      let resultado = try dividir(a: 10, b: 0)
      print(resultado)
    } catch ErrorDivision.divisionPorCero {
      print("Error: División por cero")
    } catch {  // Captura cualquier otro error
      print("Error desconocido: \(error)")
    }
    ```

4.  **`try?`:**  Convierte un error en un valor opcional (`nil` si hay error).

    ```swift
    let resultado = try? dividir(a: 10, b: 0)  // resultado es un Int? (nil en este caso)
    ```

5.  **`try!`:**  Asegura que no habrá error (lanza un *runtime error* si lo hay).  *Usar con cuidado.*

    ```swift
    let resultado = try! dividir(a: 10, b: 2) // Si hubiera error, el programa se detiene
    ```
6. **Propagación de errores:** Las funciones que llaman a otras que lanzan errores, deben manejarlos o usar `throws` para propagarlos.

## 🧵 Concurrencia (async/await, Task, etc)

*   Swift 5.5 introdujo `async/await` para programación concurrente y asíncrona estructurada.
*  Similar a las corrutinas de Kotlin o async/await en Javascript

```swift
    func obtenerDatos() async throws -> String {
        //Simular una operación larga
        try await Task.sleep(nanoseconds: 2_000_000_000) // 2 segundos
        return "Datos recibidos"
    }

    func miFuncion() async {
        do {
            let datos = try await obtenerDatos()
            print(datos)
        } catch {
            print("Error: \(error)")
        }
    }

    // Llamar la función async
    Task {
        await miFuncion()
    }
    // O, si estás en un contexto asíncrono, simplemente: await miFuncion()
```

* `async`: Se usa para declarar funciones asíncronas.
* `await`: Se usa para esperar al resultado de una función asíncrona, *suspendiendo* la ejecución de la corrutina actual (sin bloquear el hilo).
* `Task`: Representa una unidad de trabajo asíncrono.
* `TaskGroup`: Permite crear grupos de tareas.
* Actores (`actor`): Tipo de dato que protege su estado mutable del acceso concurrente.

## 🧺 Colecciones

1.  **Arrays (arreglos):**

    ```swift
    var numeros = [1, 2, 3, 4, 5]  // Array literal
    var frutas: [String] = ["manzana", "plátano"]
    var vacio = [Int]()
    var otroVacio = Array<Int>()

    numeros.append(6)
    numeros += [7, 8]
    numeros.insert(0, at: 0) // Insertar
    numeros.remove(at: 2)
    numeros[1] = 10
    print(numeros.count)
    print(numeros.isEmpty)
    print(numeros.first)  // Opcional
    print(numeros.last)  // Opcional
    print(numeros.contains(3))
    print(numeros[2])

    // Iterar
    for numero in numeros {
      print(numero)
    }
    ```

2.  **Dictionaries (diccionarios):**

    ```swift
    var miDiccionario = [  // Diccionario literal
      "nombre": "Juan",
      "edad": 30
    ]

    var otroDiccionario: [String: Any] = [
        "ciudad": "Madrid"
    ]
    var vacio = [String: Int]()
    var otroVacio = Dictionary<String, Int>()

    print(miDiccionario["nombre"])  // Acceder a un valor (devuelve un opcional)
    miDiccionario["pais"] = "España"  // Agregar o modificar
    miDiccionario.updateValue("Otro", forKey: "nombre")
    miDiccionario.removeValue(forKey: "edad")
    print(miDiccionario.count)
    print(miDiccionario.isEmpty)
    print(miDiccionario.keys)
    print(miDiccionario.values)

    // Iterar
    for (clave, valor) in miDiccionario {
      print("\(clave): \(valor)")
    }
    ```

3.  **Sets (conjuntos):**

    ```swift
    var miSet: Set = [1, 2, 3, 2, 1]  // {1, 2, 3} - Elementos únicos, no ordenados
    var otroSet: Set<String> = ["rojo", "verde"]
    var setVacio = Set<Int>()

    miSet.insert(4)
    miSet.remove(1)
    print(miSet.count)
    print(miSet.contains(3)) // true

    // Iterar
      for elemento in miSet {
        print(elemento)
      }

    // Operaciones de conjuntos
      let a: Set = [1, 2, 3]
      let b: Set = [3, 4, 5]
      print(a.union(b))  // Unión
      print(a.intersection(b))  // Intersección
      print(a.subtracting(b))  // Diferencia
      print(a.symmetricDifference(b))  // Diferencia simétrica
    ```

## ✨ Otros Conceptos y Características

*   **Optional Chaining:**  Acceder a propiedades, métodos y subíndices de un opcional de forma segura.
*   **Type Casting:**  Comprobar el tipo de una instancia (`is`) y convertir tipos (`as`, `as?`, `as!`).
*   **Automatic Reference Counting (ARC):**  Gestión de memoria automática en Swift.
*   **Guard let:**  Desenvuelve opcionales y verifica condiciones, saliendo del ámbito actual si no se cumplen.
*   **Programación Orientada a Protocolos (POP):**  Enfoque de diseño que enfatiza el uso de protocolos y extensiones.
*   **Result Type:**  Tipo genérico que representa el resultado de una operación (éxito o error).
* **Property Wrappers:** Permiten reutilizar lógica de get/set de propiedades.
*  **Opaque Types**: Permiten ocultar el tipo concreto que devuelve una función o almacena una propiedad.
* **Key Paths:** Permiten referirse a propiedades de forma dinámica.

Este cheatsheet cubre una amplia gama de características de Swift, desde los fundamentos hasta conceptos más avanzados. ¡Guárdalo y consúltalo a menudo!