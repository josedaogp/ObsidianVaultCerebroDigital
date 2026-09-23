# 🚀 Cheatsheet de JavaScript

Este cheatsheet cubre los fundamentos de JavaScript, incluyendo tipos de datos, operadores, estructuras de control, funciones, objetos, DOM, eventos, y más.

## 🔍 Variables y Tipos de Datos

1.  **Declaración de variables:**

    ```javascript
    let miVariable = 10;  // Variable mutable (puede cambiar)  # 🔄
    const MI_CONSTANTE = 20;  // Constante (no puede cambiar)  # 🔒
    var otraVariable = 30; // (Evitar usar 'var' en código moderno)
    ```

2.  **Tipos de datos primitivos:**

    *   **Number:** Números (enteros y decimales).
        ```javascript
        let edad = 30;
        let precio = 99.99;
        ```

    *   **String:** Cadenas de texto.
        ```javascript
        let nombre = "Juan";
        let mensaje = 'Hola, mundo!';
        let templateString = `Hola, ${nombre}`; // Template literals (más versátil)
        ```

    *   **Boolean:** `true` o `false`.
        ```javascript
        let esValido = true;
        let estaCompleto = false;
        ```

    *   **Null:** Valor nulo (ausencia intencional de valor).
        ```javascript
        let resultado = null;
        ```

    *   **Undefined:** Valor indefinido (variable declarada pero no inicializada).
        ```javascript
        let miVariable;
        console.log(miVariable); // undefined
        ```
    * **Symbol**: Valor único e inmutable

    * **BigInt**: Números enteros muy grandes.

3.  **Tipos de datos compuestos (objetos):**

    *   **Object:** Colección de pares clave-valor.
        ```javascript
        let persona = {
            nombre: "Ana",
            edad: 25,
            ciudad: "Madrid"
        };
        ```

    *   **Array:** Lista ordenada de valores.
        ```javascript
        let numeros = [1, 2, 3, 4, 5];
        let frutas = ["manzana", "plátano", "naranja"];
        ```

    *   **Function:** Bloque de código reutilizable.
        ```javascript
        function saludar(nombre) {
            console.log("Hola, " + nombre + "!");
        }
        ```

    * **Date**: Para trabajar con fechas y horas.
    *  **RegExp**: Para trabajar con expresiones regulares.

4.  **`typeof`:** Operador para obtener el tipo de dato de una variable.

    ```javascript
    console.log(typeof 42); // "number"
    console.log(typeof "hola"); // "string"
    console.log(typeof true); // "boolean"
    console.log(typeof {}); // "object"
    console.log(typeof []); // "object" (¡los arrays son objetos en JS!)
    console.log(typeof function() {}); // "function"
    ```

## ➕ Operadores

1.  **Aritméticos:** `+`, `-`, `*`, `/`, `%` (módulo), `**` (exponenciación).
2.  **De asignación:** `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `**=`.
3.  **De comparación:** `==` (igualdad abstracta), `===` (igualdad estricta), `!=` (desigualdad abstracta), `!==` (desigualdad estricta), `>`, `<`, `>=`, `<=`.
4.  **Lógicos:** `&&` (AND), `||` (OR), `!` (NOT).
5.  **De cadena:** `+` (concatenación).
6.  **Condicional (ternario):** `condicion ? valorSiTrue : valorSiFalse`.
7.  **Operador de coma (,):** Evalúa múltiples expresiones y retorna la última.
8. **Operador delete:** Elimina una propiedad de un objeto.
9. **Operador in**: Comprueba si una propiedad existe en un objeto.
10. **Operador instanceof:** Comprueba si un objeto es instancia de un constructor.
11. **Operador void:** Evalúa una expresión, pero no retorna un valor.
12. **Operador de propagación (...):** Expande elementos iterables (arrays, strings, etc.)

## 📝 Estructuras de Control

1.  **`if...else`:**

    ```javascript
    if (condicion) {
      // Código si la condición es verdadera
    } else if (otraCondicion) {
      // Código si otraCondicion es verdadera
    } else {
      // Código si ninguna condición es verdadera
    }
    ```

2.  **`switch`:**

    ```javascript
    switch (expresion) {
      case valor1:
        // Código si expresion === valor1
        break;
      case valor2:
        // Código si expresion === valor2
        break;
      default:
        // Código si expresion no coincide con ningún valor
    }
    ```

3.  **`for`:**

    ```javascript
    for (let i = 0; i < 5; i++) {
      console.log(i);
    }
    ```

4.  **`for...of`:** (Para iterar sobre valores de objetos iterables, como arrays y strings)

    ```javascript
    let numeros = [10, 20, 30];
    for (let numero of numeros) {
      console.log(numero);
    }

    let texto = "Hola";
      for (let caracter of texto) {
        console.log(caracter);
    }
    ```

5.  **`for...in`:** (Para iterar sobre *claves* de un objeto)

    ```javascript
    let persona = { nombre: "Juan", edad: 30 };
    for (let clave in persona) {
      console.log(clave + ": " + persona[clave]);
    }
    ```
    *Cuidado: `for...in` no garantiza el orden y puede iterar sobre propiedades heredadas.*

6.  **`while`:**

    ```javascript
    let i = 0;
    while (i < 5) {
      console.log(i);
      i++;
    }
    ```

7.  **`do...while`:**

    ```javascript
    let i = 0;
    do {
      console.log(i);
      i++;
    } while (i < 5);
    ```

8. **`break` y `continue`:**
  * `break`: Sale de un bucle o un `switch`.
  * `continue`: Salta a la siguiente iteración de un bucle.

## ⚙️ Funciones

1.  **Declaración de función:**

    ```javascript
    function saludar(nombre) {
      console.log("Hola, " + nombre + "!");
    }
    saludar("Juan");
    ```

2.  **Expresión de función:**

    ```javascript
    const sumar = function(a, b) {
      return a + b;
    };
    console.log(sumar(2, 3));
    ```

3.  **Funciones flecha (arrow functions):**

    ```javascript
    const multiplicar = (a, b) => a * b;  // Retorno implícito
    console.log(multiplicar(5, 2));

    const saludar = nombre => console.log("Hola", nombre); //Un solo parámetro

    const cuadrado = (num) => {  // Con bloque de código
        return num * num;
    }
    ```
    *   *Importante:* Las funciones flecha no tienen su propio `this` (heredan el `this` del contexto circundante).

4.  **Parámetros por defecto:**

    ```javascript
    function saludar(nombre = "Invitado") {
      console.log("Hola, " + nombre + "!");
    }
    saludar(); // Hola, Invitado!
    saludar("Ana"); // Hola, Ana!
    ```

5.  **Rest parameters (`...`):**

    ```javascript
    function sumarTodos(...numeros) {
      let total = 0;
      for (let numero of numeros) {
        total += numero;
      }
      return total;
    }
    console.log(sumarTodos(1, 2, 3, 4)); // 10
    ```

6.  **Closures:**  Una función interna tiene acceso a las variables de su función externa, incluso después de que la función externa haya terminado de ejecutarse.

    ```javascript
    function crearContador() {
      let contador = 0;
      return function() {
        contador++;
        console.log(contador);
      };
    }

    const miContador = crearContador();
    miContador(); // 1
    miContador(); // 2
    ```

## 🗂️ Objetos

1.  **Creación de objetos:**

    ```javascript
    let persona = {
      nombre: "María",
      edad: 30,
      saludar: function() {
        console.log("Hola, soy " + this.nombre);
      }
    };
    ```

2.  **Acceso a propiedades:**

    ```javascript
    console.log(persona.nombre); // "María"
    console.log(persona["edad"]); // 30
    persona.saludar(); // "Hola, soy María"
    ```

3.  **Métodos:**  Funciones que son propiedades de un objeto.

4.  **`this`:**  Se refiere al objeto actual (el contexto de ejecución).  *Su valor depende de cómo se llama la función.*

5.  **Constructores y `new`:**

    ```javascript
    function Persona(nombre, edad) {
      this.nombre = nombre;
      this.edad = edad;
      this.saludar = function() {
          console.log(`Hola, soy ${this.nombre} y tengo ${this.edad} años`);
      }
    }

    let juan = new Persona("Juan", 25);
    juan.saludar();
    ```

6. **Object.assign()**: Copia las propiedades de uno o más objetos a otro.
7. **Object.keys()**: Devuelve un array con las *claves* de un objeto.
8. **Object.values()**: Devuelve un array con los *valores* de un objeto.
9. **Object.entries()**: Devuelve un array con los pares [clave, valor] de un objeto.

## 🧺 Arrays

1.  **Creación de arrays:**

    ```javascript
    let numeros = [1, 2, 3, 4, 5];
    let frutas = ["manzana", "plátano", "naranja"];
    ```

2.  **Acceso a elementos:**

    ```javascript
    console.log(numeros[0]); // 1
    console.log(frutas[1]); // "plátano"
    ```

3.  **Métodos de array:**

    *   `push()`: Agrega uno o más elementos al final.
    *   `pop()`: Elimina y devuelve el último elemento.
    *   `shift()`: Elimina y devuelve el primer elemento.
    *   `unshift()`: Agrega uno o más elementos al principio.
    *   `splice()`: Agrega/elimina elementos en una posición específica.
    *   `slice()`: Devuelve una porción (copia superficial) de un array.
    *   `concat()`: Concatena arrays.
    *   `join()`: Une los elementos de un array en una cadena.
    *   `indexOf()`: Busca un elemento y devuelve su índice (o -1 si no se encuentra).
    *   `includes()`: Verifica si un array contiene un elemento (retorna `true` o `false`).
    *   `find()`: Devuelve el primer elemento que cumple una condición.
    *   `findIndex()`: Devuelve el índice del primer elemento que cumple una condición.
    *   `filter()`: Crea un nuevo array con los elementos que cumplen una condición.
    *   `map()`: Crea un nuevo array aplicando una función a cada elemento.
    *   `reduce()`: Aplica una función acumuladora a cada elemento (de izquierda a derecha) para reducir el array a un solo valor.
    *   `forEach()`: Ejecuta una función para cada elemento del array.
    *   `sort()`: Ordena los elementos de un array (modifica el array original).
    *   `reverse()`: Invierte el orden de los elementos (modifica el array original).
    *   `some()`: Verifica si al menos un elemento cumple una condición.
    *   `every()`: Verifica si todos los elementos cumplen una condición.
    *  `flat()`: Aplana un array anidado.
    *  `flatMap()`: Aplana un array anidado y aplica una función a cada elemento.

## 💻 DOM (Document Object Model)

1.  **Seleccionar elementos:**

    ```javascript
    // Por ID
    let elemento = document.getElementById("miElemento");

    // Por clase
    let elementos = document.getElementsByClassName("miClase"); // Devuelve una HTMLCollection (similar a un array)

    // Por etiqueta
    let parrafos = document.getElementsByTagName("p"); // Devuelve una HTMLCollection

    // Por selector CSS (el más flexible)
    let elemento2 = document.querySelector("#miElemento"); // Devuelve el primer elemento que coincide
    let elementos2 = document.querySelectorAll(".miClase"); // Devuelve una NodeList (similar a un array)
    ```

2.  **Manipular elementos:**

    ```javascript
    elemento.innerHTML = "Nuevo contenido"; // Modificar el contenido HTML
    elemento.textContent = "Texto plano"; // Modificar el contenido de texto
    elemento.style.color = "red"; // Modificar estilos CSS
    elemento.setAttribute("atributo", "valor"); // Establecer un atributo
    elemento.getAttribute("atributo"); // Obtener un atributo
    elemento.classList.add("nuevaClase"); // Agregar una clase CSS
    elemento.classList.remove("viejaClase"); // Eliminar una clase CSS
    elemento.classList.toggle("clase"); // Alternar una clase CSS
    ```

3.  **Crear y agregar elementos:**

    ```javascript
    let nuevoElemento = document.createElement("p");
    nuevoElemento.textContent = "Este es un nuevo párrafo.";
    document.body.appendChild(nuevoElemento); // Agregar al final del body
    // Otras opciones: appendChild, insertBefore, replaceChild, removeChild
    ```

## ⚡️ Eventos

1.  **Manejadores de eventos:**

    ```javascript
    // En línea (no recomendado)
    // <button onclick="miFuncion()">Haz clic</button>

    // Propiedad del elemento (mejor, pero solo un manejador por evento)
    let boton = document.getElementById("miBoton");
    boton.onclick = function() {
      alert("¡Haz hecho clic!");
    };

    // addEventListener (la mejor opción, permite múltiples manejadores)
    boton.addEventListener("click", function() {
      alert("¡Clic!");
    });
    boton.addEventListener("click", otraFuncion);
    ```

2.  **Eventos comunes:**

    *   `click`: Clic del ratón.
    *   `mouseover`: El ratón entra en un elemento.
    *   `mouseout`: El ratón sale de un elemento.
    *   `keydown`: Se presiona una tecla.
    *   `keyup`: Se suelta una tecla.
    *   `keypress`: Se presiona y suelta una tecla.
    *   `submit`: Se envía un formulario.
    *   `change`: El valor de un elemento cambia (input, select, textarea).
    *   `focus`: Un elemento recibe el foco.
    *   `blur`: Un elemento pierde el foco.
    *   `load`: La página o un elemento se ha cargado completamente.
    *   `scroll`: Se desplaza la ventana o un elemento.
    *   `resize`: Se redimensiona la ventana.

3.  **Objeto `event`:**

    ```javascript
    boton.addEventListener("click", function(event) {
      console.log(event.type); // "click"
      console.log(event.target); // El elemento que desencadenó el evento
      console.log(event.clientX, event.clientY); // Coordenadas del clic
      event.preventDefault(); // Evitar el comportamiento predeterminado (ej. en un enlace)
      event.stopPropagation();  // Detener la propagación del evento a elementos padres
    });
    ```

## ⏱️ Asincronía (Callbacks, Promesas, Async/Await)

1.  **Callbacks:**

    ```javascript
    function hacerAlgo(callback) {
      // ...hacer algo...
      callback();
    }

    hacerAlgo(function() {
      console.log("El callback se ha ejecutado.");
    });
    ```

2.  **Promesas (Promises):**

    ```javascript
    let promesa = new Promise((resolve, reject) => {
      // ...operación asíncrona...
      if (/* éxito */) {
        resolve(resultado);
      } else {
        reject(error);
      }
    });

    promesa.then(resultado => {
      console.log("Éxito:", resultado);
    }).catch(error => {
      console.error("Error:", error);
    }).finally(()=>{
        //Código que se ejecuta siempre
    });
    ```

3.  **`async`/`await`:**  (Forma más moderna y legible de trabajar con promesas)

    ```javascript
    async function miFuncionAsincrona() {
      try {
        let resultado = await miPromesa;
        console.log("Éxito:", resultado);
      } catch (error) {
        console.error("Error:", error);
      }
    }
    ```

## 🌐 AJAX (XMLHttpRequest y Fetch API)

1. **XMLHttpRequest**
    ```javascript
    const xhr = new XMLHttpRequest();
    xhr.open('GET', 'https://api.example.com/data');
    xhr.onload = function() {
        if (xhr.status >= 200 && xhr.status < 300) {
            const data = JSON.parse(xhr.response);
            console.log(data)
        }
    };
    xhr.onerror = function() {
        console.log("Error")
    }
    xhr.send();
    ```

2.  **Fetch API:** (Forma más moderna de hacer peticiones HTTP)

    ```javascript
    fetch("https://api.example.com/data")
      .then(response => {
        if (!response.ok) { // Comprobar si la respuesta es exitosa
          throw new Error("Error en la red");
        }
        return response.json(); // Parsear la respuesta como JSON
      })
      .then(data => {
        console.log(data);
      })
      .catch(error => {
        console.error("Error:", error);
      });
    ```
    *Con async/await:*
    ```javascript
        async function fetchData() {
            try {
                const response = await fetch('https://api.example.com/data');
                if (!response.ok) {
                    throw new Error('Network response error');
                }
                const data = await response.json();
                console.log(data);
            } catch (error) {
                console.log(error);
            }
        }
    ```
## 📦 JSON (JavaScript Object Notation)

*   `JSON.stringify()`: Convierte un objeto JavaScript en una cadena JSON.
*   `JSON.parse()`: Convierte una cadena JSON en un objeto JavaScript.

## 📚 Módulos (ES Modules)

```javascript
// archivo modulo.js
export const PI = 3.14159;
export function cuadrado(x) {
  return x * x;
}

// archivo principal.js
import { PI, cuadrado } from './modulo.js';
console.log(PI);
console.log(cuadrado(5));

//Exportación por defecto
// modulo.js
export default function() { ... }

// main.js
import miFuncion from './modulo.js';
```

## ✨ Otros Conceptos y Características

*   **Hoisting:**  Las declaraciones de variables y funciones se "elevan" al principio de su ámbito (pero no las inicializaciones).
*   **Strict mode (`"use strict";`):**  Modo más estricto de JavaScript (ayuda a evitar errores comunes).
*   **Expresiones regulares (RegExp):**  Patrones para buscar y manipular texto.
*   **Manejo de errores (try...catch...finally):**
*   **Web Storage (localStorage y sessionStorage):**  Para almacenar datos en el navegador.
*  **Clases (ES6)**: Sintaxis para crear objetos y herencia
*  **Destructuring:** Extrae valores de arrays u objetos
*  **Spread operator:** Expande un iterable

Este cheatsheet de JavaScript cubre los fundamentos del lenguaje, desde los tipos de datos y operadores hasta conceptos más avanzados como el DOM, la asincronía y los módulos. ¡Consúltalo a menudo!