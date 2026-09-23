# 🎯 Cheatsheet de Dart

Este cheatsheet cubre los aspectos esenciales de Dart, el lenguaje de programación de Google optimizado para construir aplicaciones rápidas en cualquier plataforma.

## 🔍 Conceptos Básicos

*   **DartPad:** 📝 Entorno de desarrollo online para Dart: [https://dartpad.dev/](https://dartpad.dev/)
*   **SDK de Dart:** 📦 Incluye la máquina virtual de Dart (Dart VM), bibliotecas, el compilador, y otras herramientas.
*   **`dart` (línea de comandos):** ⚙️ Herramienta principal del SDK.  Se usa para ejecutar, compilar, analizar, y formatear código Dart.
*   **`pub` (gestor de paquetes):** 📦 Herramienta para gestionar dependencias en Dart (similar a npm en Node.js).
*   **`pubspec.yaml`:** 📄 Archivo que define las dependencias y metadatos de un proyecto Dart.
*   **Comentarios:**
    *   `// Comentario de una línea`
    *   `/* Comentario de bloque */`
    *   `/// Comentario de documentación`

## 📦 Variables y Tipos de Datos

1.  **Declaración de variables:**

    ```dart
    var nombre = "Juan";  // Inferencia de tipo (String)
    String apellido = "Pérez";  // Tipo explícito
    dynamic edad = 30;  // Tipo dinámico (puede cambiar) - Evitar en lo posible
    Object miObjeto = "Hola"; // Object es la clase base de todos los objetos.
    ```

2.  **Tipos de datos básicos:**

    *   **`int`:** Enteros.
        ```dart
        int edad = 30;
        int hex = 0x1A; // Hexadecimal
        ```

    *   **`double`:** Números de punto flotante (doble precisión).
        ```dart
        double precio = 99.99;
        double exponente = 1.42e5; // Notación científica
        ```

    *   **`String`:** Cadenas de texto.
        ```dart
        String nombre = "Ana";
        String mensaje = 'Hola, mundo!';
        String multilinea = '''
          Este es un string
          multilinea.
        ''';
        String interpolacion = "Mi nombre es $nombre"; // Interpolación
        String concatenacion = "Hola" + " " + "Mundo";
        ```

    *   **`bool`:** Booleanos (`true` o `false`).
        ```dart
        bool esValido = true;
        ```

    *   **`num`:** Superclase de `int` y `double`.

3.  **`final` y `const`:**

    *   `final`:  La variable solo se puede asignar una vez (similar a `const` en JavaScript).
        ```dart
        final nombre = "Pedro";
        // nombre = "Otro";  // Error: no se puede reasignar
        ```

    *   `const`:  Constante en *tiempo de compilación*.  Debe inicializarse con un valor que se conozca en tiempo de compilación.  Implícitamente `final`.
        ```dart
        const PI = 3.14159;
        const colores = ['rojo', 'verde', 'azul']; // Lista constante
        ```

4.  **Valores por defecto:**  Las variables no inicializadas tienen el valor `null` por defecto (incluso las numéricas).

5. **Literales:**
   * `42` (int)
   * `3.14` (double)
   * `'Hola'` (String)
   * `true` (bool)
   * `[1, 2, 3]` (List)
   * `{'a': 1, 'b': 2}` (Map)
   * `#miSimbolo` (Symbol)
   * `null`

## ➕ Operadores

1.  **Aritméticos:** `+`, `-`, `*`, `/`, `~/` (división entera), `%` (módulo).
2.  **De igualdad y relacionales:** `==`, `!=`, `>`, `<`, `>=`, `<=`.
3.  **Lógicos:** `&&` (AND), `||` (OR), `!` (NOT).
4.  **De asignación:** `=`, `+=`, `-=`, `*=`, `/=`, `~/=`, `%=`.
5.  **Incremento y decremento:** `++`, `--` (pre y post).
6.  **Condicional (ternario):** `condicion ? expresion1 : expresion2`.
7.  **De comprobación de tipo:** `is`, `is!`.
    ```dart
     if (variable is String) { /* ... */ }
    ```
8.  **Operador de cascada (`..`):** Permite realizar múltiples operaciones sobre el mismo objeto.

    ```dart
    var lista = []
      ..add(1)
      ..add(2)
      ..add(3);
    ```

9.  **Operador de acceso condicional (`?.`):**  Accede a una propiedad o método solo si el objeto no es `null`.

    ```dart
    String? nombre;
    print(nombre?.length);  // Imprime null (no da error)
    ```
10. **Operador de propagación (`...` y `...?`):**

    ```dart
    var lista1 = [1, 2, 3];
    var lista2 = [0, ...lista1, 4]; // [0, 1, 2, 3, 4]

    List<int>? lista3 = null;
    var lista4 = [0, ...?lista3, 4]; //Con ? para evitar error si es null.
    ```
11. **Operador de coalescencia nula (`??`):**

   ```dart
      String? nombre;
      String nombreCompleto = nombre ?? "Invitado"; // Si nombre es null, usa "Invitado"
   ```

12. **Operador de asignación si es nulo (`??=`):**
    ```dart
      int? a;
      a ??= 5; // Asigna 5 a 'a' solo si 'a' es null.
    ```
13. **Operadores bitwise:** `&`, `|`, `^`, `~`, `<<`, `>>`, `>>>`.
14. **Llamada a función con !:** `funcion!()` Llama una función que puede ser null, lanzando error si lo es.

## 📝 Estructuras de Control

1.  **`if...else`:**

    ```dart
    if (condicion) {
      // ...
    } else if (otraCondicion) {
      // ...
    } else {
      // ...
    }
    ```

2.  **`switch`:**

    ```dart
    switch (variable) {
      case valor1:
        // ...
        break;
      case valor2:
        // ...
        break;
      default:
        // ...
    }
    ```

3.  **`for`:**

    ```dart
    for (var i = 0; i < 5; i++) {
      print(i);
    }

    // for...in (para iterar sobre elementos de una colección)
    var numeros = [1, 2, 3];
    for (var numero in numeros) {
      print(numero);
    }
    ```

4.  **`while`:**

    ```dart
    var i = 0;
    while (i < 5) {
      print(i);
      i++;
    }
    ```

5.  **`do...while`:**

    ```dart
    var i = 0;
    do {
      print(i);
      i++;
    } while (i < 5);
    ```

6.  **`break` y `continue`:**

## ⚙️ Funciones

1.  **Declaración de función:**

    ```dart
    // Función con tipo de retorno y parámetros
    int sumar(int a, int b) {
      return a + b;
    }

    // Función sin tipo de retorno explícito (void implícito)
    void saludar(String nombre) {
      print("Hola, $nombre!");
    }

    // Función con retorno inferido (=>)
    int multiplicar(int a, int b) => a * b;
    ```

2.  **Parámetros opcionales:**

    *   **Posicionales opcionales (`[]`):**

        ```dart
        void saludar(String nombre, [String? saludo]) {
          print("${saludo ?? 'Hola'}, $nombre!");
        }

        saludar("Juan"); // Hola, Juan!
        saludar("Juan", "Buenos días"); // Buenos días, Juan!
        ```

    *   **Nombrados opcionales (`{}`):**

        ```dart
        void presentar({String? nombre, int? edad}) {
          print("Nombre: ${nombre ?? 'Desconocido'}, Edad: ${edad ?? 'Desconocida'}");
        }

        presentar(nombre: "Ana", edad: 30);
        presentar(edad: 25);
        presentar();
        ```

    *   **Parámetros con valores por defecto:**

        ```dart
        void saludar(String nombre, {String saludo = "Hola"}) {
          print("$saludo, $nombre!");
        }

        saludar("Juan"); // Hola, Juan!
        saludar("Juan", saludo: "Buenos días"); // Buenos días, Juan!
        ```
    *  **Combinar parámetros obligatorios con opcionales**

3.  **Funciones anónimas (lambdas):**

    ```dart
    var miFuncion = (int a, int b) {
      return a + b;
    };

    // Función anónima con retorno inferido
    var otraFuncion = (int x, int y) => x * y;
    ```

4.  **Funciones como objetos de primera clase:**  Las funciones se pueden asignar a variables, pasar como argumentos a otras funciones, y retornar desde funciones.

5. **Función main():** Punto de entrada de la aplicación.

    ```dart
    void main() {
      print("Hola, mundo!");
    }
    ```
6. **Funciones de orden superior:** Funciones que toman otras como parámetro, o las retornan.

## 📦 Clases y Objetos

1.  **Declaración de clase:**

    ```dart
    class Persona {
      String? nombre;  // Propiedades (campos)
      int? edad;

      // Constructor
      Persona(this.nombre, this.edad);

      // Método
      void saludar() {
        print("Hola, soy $nombre y tengo $edad años.");
      }

       // Constructor con parámetros nombrados
        Persona({this.nombre, this.edad});

      // Constructor con valores por defecto
      // Persona({this.nombre = "Invitado", this.edad = 0});

        // Named constructor
        Persona.invitado() {
            nombre = 'Invitado';
            edad = 0;
        }

         // Factory constructor
        factory Persona.fromJson(Map<String, dynamic> json) {
            return Persona(json['nombre'], json['edad']);
        }
    }
    ```

2.  **Creación de objetos:**

    ```dart
    var persona1 = Persona("Juan", 30);
    var persona2 = Persona(nombre: "Ana", edad: 25); // Usando parámetros nombrados
    persona1.saludar();
    ```

3.  **Getters y setters:**

    ```dart
    class Rectangulo {
      double ancho = 0;
      double alto = 0;

      // Getter
      double get area {
        return ancho * alto;
      }

      // Setter (opcional)
      set area(double valor) {
        ancho = valor / 2;
        alto = valor / 2;
      }
    }

    var rect = Rectangulo();
    rect.ancho = 10;
    rect.alto = 5;
    print(rect.area);  // 50 (usa el getter)
    rect.area = 100;   // Usa el setter
    ```
   *  Se puede usar `=>` para getters y setters cortos:  `double get area => ancho * alto;`

4.  **Herencia:**

    ```dart
    class Empleado extends Persona {
      String? puesto;

      Empleado(String nombre, int edad, this.puesto) : super(nombre, edad);

      @override // Sobre escritura
      void saludar() {
        super.saludar(); // Llama al método de la clase padre
        print("Soy un empleado con puesto $puesto");
      }
    }
    ```

5.  **Clases abstractas:**

    ```dart
    abstract class Animal {
      void hacerSonido();  // Método abstracto (sin implementación)
    }

    class Perro extends Animal {
      @override
      void hacerSonido() {
        print("Guau!");
      }
    }
    ```

6.  **Interfaces (implícitas):**  En Dart, todas las clases definen implícitamente una interfaz.

    ```dart
    class Volador {
      void volar() {}
    }

    // Implementa la interfaz Volador
    class Pato implements Volador {
      @override
      void volar() {
        print("Pato volando...");
      }
    }
    ```

7.  **Mixins:**  Permiten reutilizar código de múltiples clases sin herencia múltiple.

    ```dart
    mixin Caminante {
      void caminar() {
        print("Caminando...");
      }
    }

    mixin Nadador {
      void nadar() {
        print("Nadando...");
      }
    }

    class Pato with Caminante, Nadador {}

    var pato = Pato();
    pato.caminar();
    pato.nadar();
    ```

8.  **Extension methods:**  Añaden funcionalidad a clases existentes (incluso de bibliotecas externas).

    ```dart
    extension StringExtension on String {
      String aMayusculas() {
        return toUpperCase();
      }
    }

    void main() {
      print("hola".aMayusculas());  // HOLA
    }
    ```

9.  **Clases estáticas:**

    ```dart
      class Matematicas {
        static double PI = 3.14159;

        static double cuadrado(double x) {
          return x * x;
        }
      }

      void main() {
        print(Matematicas.PI); // Acceder a un miembro estático
        print(Matematicas.cuadrado(5));
      }
    ```
10. **Operador cascada**: Permite hacer operaciones múltiples sobre el mismo objeto.

## 🧺 Colecciones

1.  **Listas (List):**

    ```dart
    var numeros = [1, 2, 3, 4, 5];  // Lista literal
    var frutas = <String>['manzana', 'plátano']; // Lista con tipo explícito
    var listaVacia = []; // Lista vacía
    var otraListaVacia = <String>[];

    numeros.add(6); // Agregar un elemento
    numeros.addAll([7, 8]); // Agregar varios
    numeros.removeAt(0); // Eliminar por índice
    print(numeros.length);  // Longitud
    print(numeros[2]);  // Acceder a un elemento por índice
    print(numeros.first);
    print(numeros.last);
    print(numeros.isEmpty);
    print(numeros.contains(3));
    print(numeros.indexOf(4));

    // Iterar
    for (var numero in numeros) {
      print(numero);
    }

    numeros.forEach((numero) => print(numero));

    var cuadrados = numeros.map((numero) => numero * numero).toList(); // Transformar
    var filtrados = numeros.where((numero) => numero > 3).toList(); //Filtrar

    numeros.sort(); // Ordenar
    numeros.shuffle(); // Mezclar.
    var sublista = numeros.sublist(1,4);
    var invertida = numeros.reversed.toList();
    ```

2.  **Conjuntos (Set):**  Colecciones de elementos únicos (no ordenados).

    ```dart
    var miSet = {1, 2, 3, 2, 1};  // {1, 2, 3} - elimina duplicados
    var otroSet = <String>{'rojo', 'verde', 'azul'};
     var setVacio = <String>{};

    miSet.add(4);
    miSet.addAll({5,6});
    miSet.remove(1);
    print(miSet.length); // Tamaño
    print(miSet.contains(3)); // true

    // Iterar
      for (var elemento in miSet) {
        print(elemento);
      }
    print(miSet.union(otroSet));
    print(miSet.intersection(otroSet));
    print(miSet.difference(otroSet));
    ```

3.  **Mapas (Map):**  Colecciones de pares clave-valor.

    ```dart
    var miMapa = {  // Mapa literal
      'nombre': 'Juan',
      'edad': 30,
      'ciudad': 'Madrid'
    };

    var otroMapa = <String, dynamic>{ //Con tipado
        'nombre': 'Ana',
        'edad': 25
    };

    var mapaVacio = {};
    var otroMapaVacio = <String, int>{};

    print(miMapa['nombre']);  // Acceder a un valor por clave
    miMapa['pais'] = 'España';  // Agregar o modificar un valor
    miMapa.remove('edad');
    print(miMapa.length);  // Número de pares clave-valor
    print(miMapa.containsKey('ciudad')); // true
    print(miMapa.containsValue('Juan'));
    print(miMapa.keys); //Iterable con las claves
    print(miMapa.values); //Iterable con los valores

    // Iterar
      for (var clave in miMapa.keys) {
        print('$clave: ${miMapa[clave]}');
      }
    miMapa.forEach((clave, valor) => print('$clave: $valor'));

    var mapaTransformado = miMapa.map((key, value) => MapEntry(key, value.toString().toUpperCase())); //Transformar
    ```
4. **Runes:** Representan los *code points* Unicode de un String.
5. **Symbols:** Objetos usados para representar operadores o identificadores.

## ⏳ Programación Asíncrona (async, await, Future, Stream)

1.  **`Future`:**  Representa un valor potencial (o error) que estará disponible en algún momento en el futuro.

    ```dart
    Future<String> obtenerDatos() {
      return Future.delayed(Duration(seconds: 2), () => "Datos recibidos");
    }
    ```

2.  **`async` y `await`:**  Simplifican el trabajo con Futures.

    ```dart
    Future<void> miFuncionAsincrona() async {
      try {
        String datos = await obtenerDatos(); // Espera a que el Future se complete
        print(datos);
      } catch (error) {
        print("Error: $error");
      }
    }
    ```

3.  **`Stream`:**  Secuencia de datos asíncronos (como un `Future` que puede devolver múltiples valores).

    ```dart
      Stream<int> contador(int max) async* {
        for (int i = 1; i <= max; i++) {
          yield i; // Emite un valor
          await Future.delayed(Duration(seconds: 1)); // Espera 1 segundo
        }
      }

      void main() {
        var miStream = contador(5);
        miStream.listen((numero) {
          print(numero); // 1, 2, 3, 4, 5 (cada segundo)
        });
      }
    ```
  * `async*`: Se usa para funciones que retornan un Stream.
  * `yield`: Emite un valor en el Stream.

4. **Then/catchError:** Alternativa a async/await

```dart
    obtenerDatos().then((valor){
        print(valor);
    }).catchError((error){
        print(error);
    });
```

## 📦 Null Safety (Seguridad Nula)

*   A partir de Dart 2.12, la *null safety* es obligatoria.
*   Los tipos no pueden ser `null` por defecto.
*   Para permitir que una variable sea `null`, se usa el operador `?`.

```dart
String? nombre;  // 'nombre' puede ser null
int? edad = null;

print(nombre?.length); // Usar ?. para acceder a propiedades/métodos de forma segura

String valor = nombre ?? "Valor por defecto"; //Operador ??

void miFuncion({String? parametroOpcional}){} // Parámetro opcional que puede ser null.
```

* **Late variables:**
  ```dart
    late String descripcion; // Se inicializará después, antes de ser usada.
  ```
* **Operador ! (bang operator):**
  ```dart
    String? posibleNulo = "Hola";
    String noNulo = posibleNulo!; // Asegura que no es nulo (lanza error si lo es)
  ```

## ✨ Otros Conceptos y Características

*   **Excepciones (try, catch, on, finally, throw):**

    ```dart
    try {
      // Código que puede lanzar una excepción
    } on FormatException catch (e) { // Capturar un tipo específico de excepción
      print("Error de formato: $e");
    } catch (e, s) { // Capturar cualquier excepción
      print("Error: $e");
      print(s); // Stack trace
    } finally {
      // Código que se ejecuta siempre (haya o no excepción)
    }

    // Lanzar una excepción
      throw Exception("Algo salió mal");
    ```

*   **Enumeraciones (enum):**

    ```dart
    enum Color { rojo, verde, azul }

    void main() {
      var miColor = Color.rojo;
      print(miColor);  // Color.rojo

      switch (miColor) {
        case Color.rojo:
          print("Rojo");
          break;
        case Color.verde:
          print("Verde");
          break;
        case Color.azul:
          print("Azul");
          break;
      }
    }
    ```
* **Generics:** Permiten escribir código que funciona con diferentes tipos de datos, de forma segura.
    ```dart
    class Caja<T> {
        T? contenido;
        Caja(this.contenido);
        T? obtenerContenido() => contenido;
    }

    var cajaEnteros = Caja<int>(5);
    var cajaString = Caja<String>('Hola');
    ```

*   **Typedefs:**  Define un alias para un tipo de función.
*  **Metadatos:** Anotaciones como `@deprecated`, `@override`. Se pueden crear anotaciones personalizadas.
* **Mixins:** Permiten reutilizar código de múltiples clases.

Este cheatsheet cubre una amplia gama de características de Dart, desde los fundamentos hasta conceptos más avanzados. ¡Espero que te sea de gran ayuda!