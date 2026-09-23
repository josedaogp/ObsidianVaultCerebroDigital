#  TypeScript Cheatsheet: JavaScript con Superpoderes 🚀

## 🌟 **Conceptos Fundamentales**

*   **TypeScript:** Un superconjunto de JavaScript que añade *tipado estático* (opcional).  El código TypeScript se *transpila* (compila) a JavaScript.
*   **Tipado Estático:** Define los tipos de datos de las variables, parámetros de funciones, valores de retorno, etc.  El compilador de TypeScript verifica los tipos en tiempo de compilación, ayudando a detectar errores antes de la ejecución.
*   **Infer Types:** TypeScript puede *inferir* (deducir) los tipos automáticamente en muchos casos, sin necesidad de escribirlos explícitamente.
*   **Interfaces:** Define la "forma" de un objeto (sus propiedades y métodos).  Similar a una clase abstracta, pero solo para tipos.
*   **Clases:** Define objetos con propiedades y métodos.  TypeScript añade características a las clases de JavaScript (modificadores de acceso, interfaces, etc.).
*   **Genéricos:** Permite escribir código que funciona con diferentes tipos de datos, manteniendo la seguridad de tipos.
*  **Módulos**: Organiza el código en archivos separados.
*  **Decoradores**: Añade metadatos o modifica el comportamiento de clases, métodos, propiedades, etc.

## 💻 **Instalación y Configuración**

1.  **Instalar TypeScript (globalmente):**

    ```bash
    npm install -g typescript
    ```

2.  **Crear un archivo TypeScript:**  `mi_archivo.ts`

3.  **Compilar (transpilar) a JavaScript:**

    ```bash
    tsc mi_archivo.ts
    ```
    Esto generará un archivo `mi_archivo.js`.

4.  **`tsconfig.json`:** Archivo de configuración del compilador de TypeScript.  Define opciones como:
    *   `target`:  Versión de JavaScript a la que se transpila (ej: "ES5", "ES6", "ES2015", "ESNext").
    *   `module`:  Sistema de módulos (ej: "CommonJS", "ESNext").
    *   `strict`:  Habilita todas las opciones de verificación estricta de tipos (`true`/`false`).
    *   `outDir`:  Directorio de salida para los archivos JavaScript.
    *   `rootDir`:  Directorio raíz de los archivos TypeScript.
    *   `include`, `exclude`:  Patrones para incluir/excluir archivos.
    *  `sourceMap`: Genera archivos source map (para debugging).
    *  `noImplicitAny`: Error si no se especifica el tipo y no se puede inferir.
    *  `strictNullChecks`: Trata `null` y `undefined` como tipos distintos.

    ```json
    {
      "compilerOptions": {
        "target": "ES6",
        "module": "CommonJS",
        "strict": true,
        "outDir": "dist",
        "rootDir": "src",
        "sourceMap": true
      },
      "include": ["src/**/*"],
      "exclude": ["node_modules"]
    }
    ```

    *   Para crear un `tsconfig.json` por defecto: `tsc --init`

5.  **Ejecutar TypeScript directamente (sin compilación manual):**
    *   **`ts-node`:**  Ejecuta archivos TypeScript directamente (útil para desarrollo).  `npm install -g ts-node`
    *   **Deno:**  Un runtime moderno para JavaScript y TypeScript (alternativa a Node.js).

## 📝 **Tipos Básicos**

*   **`number`:**  Números (enteros y decimales).
*   **`string`:**  Cadenas de texto.
*   **`boolean`:**  Booleanos (`true` o `false`).
*   **`null`:**  Valor nulo.
*   **`undefined`:**  Valor indefinido.
*   **`void`:**  Ausencia de valor (generalmente para funciones que no devuelven nada).
*   **`any`:**  Cualquier tipo (desactiva la verificación de tipos).  ¡Evítalo en lo posible!
*   **`unknown`:**  Similar a `any`, pero más seguro.  Debes verificar el tipo antes de usarlo.
*   **`never`:**  Representa un valor que nunca ocurre (ej: una función que siempre lanza un error).
*   **`object`:** Cualquier valor que no sea primitivo.
*  **Arrays:**
    *   `number[]`:  Array de números.
    *   `string[]`:  Array de cadenas.
    *   `Array<number>`:  Otra forma de escribir arrays (usando genéricos).
*   **Tuplas:**  Arrays con un número fijo de elementos de tipos específicos.
    *   `[number, string]`:  Tupla con un número y una cadena.
*   **Enums:**  Conjunto de valores con nombre.

    ```typescript
    enum Color {
      Rojo,  // 0
      Verde, // 1
      Azul,  // 2
    }

    let c: Color = Color.Verde;

    enum Direccion {
        Arriba = "UP",
        Abajo = "DOWN",
        Izquierda = "LEFT",
        Derecha = "RIGHT"
    }
    ```
* **Literales**:

```typescript
  let miColor: "rojo" | "verde" | "azul";
  miColor = "rojo"; // OK
  // miColor = "amarillo"; // Error

  let miNumero: 0 | 1 | 2;

  function miFuncion(arg: 1 | 2 | 3): "a" | "b" | "c" {
    // ...
  }
```

## ✍️ **Declaración de Variables**

*   **`let`:**  Declara una variable (similar a `var` en JavaScript, pero con mejor *scoping*).
*   **`const`:**  Declara una constante (su valor no puede cambiar).
*   **Inferencia de tipos:**  Si no especificas el tipo, TypeScript lo inferirá a partir del valor inicial.

```typescript
let edad: number = 30;
let nombre = "Ana";  // TypeScript infiere que nombre es de tipo string
const PI = 3.14159;
```

## ➡️ **Funciones**

*   **Tipos de parámetros:**

    ```typescript
    function saludar(nombre: string) {
      console.log("Hola, " + nombre);
    }
    ```

*   **Tipo de retorno:**

    ```typescript
    function sumar(a: number, b: number): number {
      return a + b;
    }
    ```

*   **Parámetros opcionales:**  Usa `?` después del nombre del parámetro.

    ```typescript
    function construirNombre(nombre: string, apellido?: string) {
      // ...
    }
    ```

*   **Parámetros por defecto:**

    ```typescript
    function saludar(nombre: string = "Invitado") {
      // ...
    }
    ```

*   **Funciones flecha (arrow functions):**

    ```typescript
    const multiplicar = (a: number, b: number): number => {
      return a * b;
    };

    const dividir = (a: number, b:number) => a / b; // Return implícito

    ```

*   **Sobrecarga de funciones (function overloading):**  Define múltiples firmas para una misma función.

    ```typescript
    function procesar(valor: string): string;
    function procesar(valor: number): number;
    function procesar(valor: any): any {
      // ... (implementación)
    }
    ```
* **Rest parameters:**
  ```typescript
  function buildName(firstName: string, ...restOfName: string[]) {
      return firstName + " " + restOfName.join(" ");
  }

  let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
  ```

## 🎭 **Interfaces**

*   Definen la "forma" de un objeto.

    ```typescript
    interface Persona {
      nombre: string;
      edad: number;
      email?: string;  // Propiedad opcional
      saludar(): void; // Método
    }

    let juan: Persona = {
      nombre: "Juan",
      edad: 30,
      saludar() {
        console.log("Hola, soy " + this.nombre);
      }
    };
    ```

*   **Propiedades opcionales:**  Usa `?`.
*   **Propiedades de solo lectura:**  Usa `readonly`.
*   **Extender interfaces:**

    ```typescript
    interface Empleado extends Persona {
      puesto: string;
    }
    ```
*  **Index signatures:** Permite definir propiedades dinámicas.

    ```typescript
      interface StringMap {
        [key: string]: string; // Cualquier propiedad de tipo string debe tener un valor de tipo string
      }
    ```

##  ക്ലാ **Clases**

```typescript
class Animal {
  nombre: string; // Por defecto es público

  constructor(nombre: string) {
    this.nombre = nombre;
  }

  moverse(distancia: number = 0) {
    console.log(`${this.nombre} se movió ${distancia} metros.`);
  }
}
```

*   **Modificadores de acceso:**
    *   `public`:  Accesible desde cualquier lugar (por defecto).
    *   `private`:  Accesible solo desde dentro de la clase.
    *   `protected`:  Accesible desde dentro de la clase y sus subclases.
*   **Constructor:**  Método especial que se ejecuta al crear una instancia de la clase.
*   **`readonly`:**  Permite que una propiedad sea de solo lectura.
*   **Herencia:**

    ```typescript
    class Perro extends Animal {
      ladrar() {
        console.log("Guau!");
      }
    }

    const miPerro = new Perro("Fido");
    miPerro.moverse(10);
    miPerro.ladrar();

    ```

*   **Métodos estáticos (`static`):**  Métodos que pertenecen a la clase, no a las instancias.
*   **Clases abstractas (`abstract`):**  Clases que no se pueden instanciar directamente.  Sirven como base para otras clases.
    *   Pueden tener métodos abstractos (sin implementación), que deben ser implementados por las subclases.
*   **Getters y setters:**

    ```typescript
      class Persona {
          private _nombreCompleto: string = "";

          get nombreCompleto(): string {
              return this._nombreCompleto;
          }
          set nombreCompleto(nombre: string) {
              if (nombre && nombre.length > 3) {
                this._nombreCompleto = nombre;
              } else {
                console.error("Nombre inválido");
              }
          }
      }
    ```
* **Implementar interfaces**:

```typescript
  interface IClock {
      currentTime: Date;
      setTime(d: Date): void;
  }

  class Clock implements IClock {
      currentTime: Date = new Date();
      setTime(d: Date) {
          this.currentTime = d;
      }
      constructor(h: number, m: number) {}
}
```

## ➕ **Tipos Avanzados**

*   **Uniones (Union Types):**  Una variable puede ser de uno de varios tipos.

    ```typescript
    let valor: number | string;
    valor = 10;
    valor = "Hola";
    // valor = true; // Error
    ```

*   **Intersecciones (Intersection Types):**  Combina varios tipos en uno solo.

    ```typescript
    interface A {
      propA: string;
    }

    interface B {
      propB: number;
    }

    type C = A & B; // C tiene propA y propB

    let obj: C = { propA: "Hola", propB: 10 };
    ```

*   **Alias de tipos (Type Aliases):**  Crea un nombre para un tipo.

    ```typescript
    type Punto = { x: number; y: number };

    let p: Punto = { x: 10, y: 20 };
    ```

*   **Type Guards:**  Funciones que verifican el tipo de un valor.

    ```typescript
    function esNumero(valor: any): valor is number { // "valor is number" es el type guard
      return typeof valor === "number";
    }

    if (esNumero(x)) {
      // Dentro de este bloque, TypeScript sabe que x es un número
      console.log(x + 10);
    }
    ```
* **Tipos condicionales:**
  ```typescript
  type IsString<T> = T extends string ? true : false;

  type A = IsString<string>; // A es true
  type B = IsString<number>; // B es false
  ```

*   **`keyof`:**  Obtiene las claves de un tipo (como unión de literales).

    ```typescript
    interface Persona {
      nombre: string;
      edad: number;
    }

    type ClavesDePersona = keyof Persona; // "nombre" | "edad"
    ```

*   **`typeof` (en tipos):**  Obtiene el tipo de una variable.
*   **Mapped Types:**  Transforma un tipo en otro.

    ```typescript
    type ReadonlyProps<T> = {
      readonly [P in keyof T]: T[P];
    };

    type ReadonlyPersona = ReadonlyProps<Persona>; // Todas las propiedades de Persona se vuelven readonly
    ```
*   **Utility Types (Tipos de utilidad):**  Tipos predefinidos que realizan transformaciones comunes.
    *   `Partial<T>`:  Hace que todas las propiedades de `T` sean opcionales.
    *   `Required<T>`:  Hace que todas las propiedades de `T` sean obligatorias.
    *   `Readonly<T>`:  Hace que todas las propiedades de `T` sean de solo lectura.
    *   `Pick<T, K>`:  Selecciona un subconjunto de propiedades (`K`) de un tipo (`T`).
    *   `Omit<T, K>`:  Omite un subconjunto de propiedades (`K`) de un tipo (`T`).
    *   `Record<K, T>`:  Crea un tipo con claves (`K`) y valores (`T`).
    *   `Exclude<T, U>`:  Excluye de `T` los tipos que son asignables a `U`.
    *   `Extract<T, U>`:  Extrae de `T` los tipos que son asignables a `U`.
    *   `NonNullable<T>`:  Excluye `null` y `undefined` de `T`.
    *   `ReturnType<T>`:  Obtiene el tipo de retorno de una función.
    *   `Parameters<T>`: Obtiene los tipos de los parámetros de una función (como tupla).
    *  `InstanceType<T>`:  Obtiene el tipo de instancia de una clase.

* **Tipos literales**:
  ```typescript
  type Resultado = "success" | "error";
  let miResultado: Resultado;
  miResultado = "success"; // OK
  // miResultado = "failure"; // Error
  ```

## <> **Genéricos**

*   Permiten escribir código que funciona con diferentes tipos de datos, manteniendo la seguridad de tipos.

    ```typescript
    function identidad<T>(arg: T): T {
      return arg;
    }

    let resultado = identidad<string>("Hola"); // Especificamos el tipo
    let resultado2 = identidad(10); // Inferencia de tipos
    ```

*   **Restricciones de tipos (Type Constraints):**  Limita los tipos que se pueden usar con un genérico.

    ```typescript
    interface Longitud {
      length: number;
    }
                                //Extiende de Longitud
    function imprimirLongitud<T extends Longitud>(arg: T) {
      console.log(arg.length);
    }

    ```
* **Clases genéricas:**

```typescript
  class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
  }
```

## 📦 **Módulos**

*   **`export`:**  Exporta variables, funciones, clases, interfaces, etc., desde un módulo.
*   **`import`:**  Importa elementos desde otro módulo.

    ```typescript
    // modulo.ts
    export const PI = 3.14159;

    export function sumar(a: number, b: number): number {
      return a + b;
    }

    // main.ts
    import { PI, sumar } from "./modulo";

    console.log(PI);
    console.log(sumar(2, 3));

    ```

*   **Exportaciones por defecto (`export default`):**

    ```typescript
    // modulo.ts
    export default class MiClase {
      // ...
    }

    // main.ts
    import MiClase from "./modulo";
    ```

*   **Importar todo (`import * as ...`):**

    ```typescript
    import * as miModulo from "./modulo";

    console.log(miModulo.PI);
    ```
* **Re-exportar:** `export { x } from "./modulo";` o `export * from "./modulo";`
* **Namespaces**: Agrupa código relacionado (menos común hoy en día que los módulos).

## ✨ **Decoradores**

*   Funciones especiales que se aplican a clases, métodos, propiedades, parámetros, etc., usando la sintaxis `@nombreDecorador`.
*   Permiten añadir metadatos, modificar el comportamiento o realizar acciones adicionales.
*  Debes habilitarlos en `tsconfig.json` con `"experimentalDecorators": true`.

```typescript
function log(target: any, key: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;

  descriptor.value = function(...args: any[]) {
    console.log(`Llamando a ${key} con argumentos: ${JSON.stringify(args)}`);
    const result = originalMethod.apply(this, args);
    console.log(`Resultado de ${key}: ${result}`);
    return result;
  };

  return descriptor;
}

class Calculadora {
  @log
  sumar(a: number, b: number) {
    return a + b;
  }
}

const calc = new Calculadora();
calc.sumar(2, 3); // Imprime logs adicionales
```

* **Decoradores de clase:** Se aplican a la clase.
* **Decoradores de método:** Se aplican a un método.
* **Decoradores de propiedad:** Se aplican a una propiedad.
* **Decoradores de parámetro:** Se aplican a un parámetro de un método o constructor.
* **Factories de decoradores**: Funciones que *retornan* un decorador.

```typescript
function color(value: string) { // Factory de decorador
  return function (target: any) { // El decorador
    // ...
  };
}

@color("red") //Uso del decorador, pasando un valor a la factory.
class MiClase {}
```

## 🛠️ **Utilidades y Técnicas Adicionales**

*   **`as` (Type Assertion):**  Le dice al compilador que trate un valor como un tipo específico (similar a un *cast* en otros lenguajes).  ¡Úsalo con cuidado!

    ```typescript
    let valor: any = "Hola";
    let longitud: number = (valor as string).length; // Le decimos al compilador que valor es un string.
    //Otra forma (equivalente):
    let longitud: number = (<string>valor).length;
    ```

*   **Archivos de declaración (`.d.ts`):**  Describen los tipos de bibliotecas de JavaScript existentes.  Permiten usar bibliotecas de JavaScript en TypeScript con seguridad de tipos.
    *  Muchas bibliotecas ya incluyen sus propios archivos de declaración.
    *  `DefinitelyTyped`:  Repositorio con archivos de declaración para miles de bibliotecas de JavaScript.
* **Compilación condicional:** Usar comentarios especiales para incluir/excluir código según la configuración.
* **Mixins**: Combina funcionalidades de varias clases.