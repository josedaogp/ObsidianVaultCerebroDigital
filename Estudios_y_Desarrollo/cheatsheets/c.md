#  C Cheatsheet: El Lenguaje de Programación Fundamental ⚙️

## 🌟 **Conceptos Fundamentales**

*   **C:** Un lenguaje de programación *compilado*, *de propósito general*, *estructurado* y *de nivel medio*.
*   **Compilado:** El código fuente (archivos `.c`) se *compila* a código máquina (ejecutable) antes de la ejecución.
*   **Estructurado:** Utiliza funciones, estructuras de control (if, for, while) y bloques de código para organizar el programa.
*   **Nivel Medio:** Combina características de lenguajes de alto nivel (más fáciles de usar) y de bajo nivel (más control sobre el hardware).
*   **Tipado Estático:** Los tipos de datos de las variables se deben declarar explícitamente y se verifican en tiempo de compilación.
*   **Punteros (Pointers):** Variables que almacenan *direcciones de memoria*.  Son una característica *fundamental* de C, y permiten un control muy preciso sobre la memoria.
*   **Gestión Manual de Memoria:**  El programador es responsable de *asignar* y *liberar* memoria dinámicamente (con `malloc`, `calloc`, `realloc` y `free`).  Esto da mucha flexibilidad, pero también es una fuente común de errores (memory leaks, segmentation faults).
*   **Biblioteca Estándar (Standard Library):**  Conjunto de funciones predefinidas para realizar tareas comunes (entrada/salida, manipulación de cadenas, matemáticas, etc.).
* **Preprocesador:** Procesa el código fuente *antes* de la compilación (directivas `#include`, `#define`, etc.).

## 💻 **Estructura de un Programa en C**

```c
#include <stdio.h>  // Directiva del preprocesador: incluye la cabecera stdio.h (para printf, scanf, etc.)

// Declaraciones de funciones (prototipos)
int sumar(int a, int b);

// Variable global
int variableGlobal = 10;

// Función principal (main): punto de entrada del programa
int main() {
  // Declaraciones de variables locales
  int x = 5;
  int y = 7;
  int resultado;

  // Llamada a la función sumar
  resultado = sumar(x, y);

  // Imprimir el resultado
  printf("La suma de %d y %d es: %d\n", x, y, resultado);

  // Valor de retorno de la función main (0 indica éxito)
  return 0;
}

// Definición de la función sumar
int sumar(int a, int b) {
  return a + b;
}
```

*   **`#include`:**  Directiva del preprocesador.  Incluye el contenido de un archivo de cabecera (header file).
    *   `<stdio.h>`:  Entrada/salida estándar (printf, scanf, etc.).
    *   `<stdlib.h>`:  Funciones de utilidad general (malloc, free, atoi, rand, etc.).
    *   `<string.h>`:  Manipulación de cadenas (strcpy, strlen, strcmp, etc.).
    *   `<math.h>`:  Funciones matemáticas (sin, cos, sqrt, pow, etc.).
    *   `<stdbool.h>`:  Tipo booleano (`bool`, `true`, `false`).  (C99)
    *   `<stdint.h>`:  Tipos enteros con tamaño específico (ej: `int32_t`, `uint64_t`).  (C99)
    * `<ctype.h>`: Funciones para clasificación de caracteres
    *  `<time.h>`: Funciones relacionadas con el tiempo
*   **`main()`:**  Función principal.  El punto de entrada del programa.  Debe devolver un entero (`int`).
*   **Comentarios:**
    *   `//`:  Comentario de una sola línea.
    *   `/* ... */`:  Comentario de varias líneas.

## 📝 **Tipos de Datos**

*   **Enteros:**
    *   `int`:  Entero (el tamaño depende del sistema, generalmente 4 bytes).
    *   `short`:  Entero corto (generalmente 2 bytes).
    *   `long`:  Entero largo (generalmente 4 u 8 bytes).
    *   `long long`:  Entero muy largo (generalmente 8 bytes).  (C99)
    *   `unsigned int`, `unsigned short`, `unsigned long`, `unsigned long long`:  Versiones sin signo (solo valores positivos).
    *  `char`:  Carácter (1 byte).  También se puede usar como un entero pequeño.
*   **Decimales (punto flotante):**
    *   `float`:  Número de punto flotante de precisión simple (generalmente 4 bytes).
    *   `double`:  Número de punto flotante de doble precisión (generalmente 8 bytes).
    * `long double`:  Número de punto flotante de precisión extendida.
*   **Booleanos:**
    *   `bool`:  `true` o `false`.  (Requiere `<stdbool.h>`).  En C, *cualquier valor distinto de cero es verdadero*, y *cero es falso*.
*   **`void`:**  Tipo especial que representa la *ausencia de tipo*.  Se usa para:
    *   Funciones que no devuelven ningún valor.
    *   Punteros genéricos (`void *`).
*  **Enumeraciones (`enum`):**
* **Tipos definidos por el usuario:** `struct`, `union`, `typedef`.

## ✍️ **Declaración de Variables**

```c
int edad = 30;
float peso = 75.5;
char letra = 'A';
bool esValido = true;

int x, y, z; // Declaración múltiple

const float PI = 3.14159; // Constante
```

*   **`const`:**  Define una constante (su valor no puede cambiar).

## 🧮 **Operadores**

*   **Aritméticos:**  `+`, `-`, `*`, `/`, `%` (módulo/resto), `++` (incremento), `--` (decremento).
*   **De asignación:**  `=`, `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `|=`, `^=`, `<<=`, `>>=`.
*   **De comparación:**  `==` (igual), `!=` (no igual), `>` (mayor que), `<` (menor que), `>=` (mayor o igual que), `<=` (menor o igual que).
*   **Lógicos:**  `&&` (AND), `||` (OR), `!` (NOT).
*   **De bits (Bitwise):**  `&` (AND), `|` (OR), `^` (XOR), `~` (NOT), `<<` (desplazamiento a la izquierda), `>>` (desplazamiento a la derecha).
*   **Ternario (condicional):**  `condición ? valor_si_verdadero : valor_si_falso`.

    ```c
    int max = (a > b) ? a : b; // Si a > b, max = a; si no, max = b.
    ```

*   **`sizeof`:**  Operador que devuelve el tamaño (en bytes) de un tipo de dato o una variable.
* **Operador coma (`,`):** Evalúa varias expresiones de izquierda a derecha, y devuelve el valor de la última.
* **Casting**: Conversión explícita de tipos. `(tipo)expresion`

## 🚦 **Control de Flujo (Condicionales y Bucles)**

*   **`if`-`else`:**

    ```c
    if (edad >= 18) {
      printf("Eres mayor de edad.\n");
    } else {
      printf("Eres menor de edad.\n");
    }
    ```

*   **`switch`-`case`:**

    ```c
    switch (opcion) {
      case 1:
        printf("Opción 1 seleccionada.\n");
        break;
      case 2:
        printf("Opción 2 seleccionada.\n");
        break;
      default:
        printf("Opción no válida.\n");
    }
    ```

*   **`for` (bucle):**

    ```c
    for (int i = 0; i < 10; i++) {
      printf("%d ", i);
    }
    ```

*   **`while` (bucle):**

    ```c
    int i = 0;
    while (i < 10) {
      printf("%d ", i);
      i++;
    }
    ```

*   **`do`-`while` (bucle):**

    ```c
    int i = 0;
    do {
      printf("%d ", i);
      i++;
    } while (i < 10);
    ```

*   **`break`:**  Sale de un bucle o de un `switch`.
*   **`continue`:**  Salta a la siguiente iteración de un bucle.
*  **`goto`:** Salta a una etiqueta (generalmente desaconsejado).

##  ফাংশൻ **Funciones**

```c
// Declaración (prototipo)
int sumar(int a, int b);

// Definición
int sumar(int a, int b) {
  return a + b;
}

// Llamada
int resultado = sumar(5, 3);
```

*   **Paso de parámetros:**
    *   **Por valor (by value):**  Se pasa una *copia* del valor del argumento.  Los cambios dentro de la función no afectan a la variable original.  (Por defecto en C).
    *   **Por referencia (by reference):**  Se pasa la *dirección de memoria* de la variable (usando punteros).  Los cambios dentro de la función *sí* afectan a la variable original.

        ```c
        void incrementar(int *x) { // Recibe un puntero a int
          (*x)++;  // Incrementa el valor apuntado por x
        }

        int main() {
          int num = 10;
          incrementar(&num); // Pasa la dirección de memoria de num
          printf("%d\n", num); // Imprime 11
          return 0;
        }
        ```

*   **Recursividad:** Una función puede llamarse a sí misma.
* **Funciones inline**: El compilador *puede* sustituir la llamada a la función por el código de la función (para optimizar).  `inline int max(int a, int b) { ... }`
*  **`return`**:  Devuelve un valor de la función (y termina la ejecución de la función).

## ➡️ **Punteros (Pointers)**

*   Un puntero es una variable que almacena la *dirección de memoria* de otra variable.
*   **Declaración:**

    ```c
    int *ptr;  // Declara un puntero a un entero
    ```

*   **Operador `&` (dirección de):**  Obtiene la dirección de memoria de una variable.

    ```c
    int x = 10;
    int *ptr = &x; // ptr apunta a x
    ```

*   **Operador `*` (indirección o desreferenciación):**  Accede al valor almacenado en la dirección de memoria apuntada por el puntero.

    ```c
    int x = 10;
    int *ptr = &x;
    printf("%d\n", *ptr); // Imprime 10 (el valor de x)
    *ptr = 20; // Modifica el valor de x (ahora x es 20)
    ```

*   **Punteros a `void` (`void *`):**  Punteros genéricos (pueden apuntar a cualquier tipo de dato).  Necesitan un *cast* para desreferenciarlos.
*   **Aritmética de punteros:**  Se pueden sumar o restar enteros a los punteros.  El puntero se incrementa o decrementa en múltiplos del tamaño del tipo de dato al que apunta.
*   **Punteros y arrays:**  El nombre de un array *es* un puntero al primer elemento del array.

    ```c
    int arr[5] = {1, 2, 3, 4, 5};
    int *ptr = arr; // ptr apunta al primer elemento de arr

    printf("%d\n", *ptr);     // Imprime 1
    printf("%d\n", *(ptr + 2)); // Imprime 3 (accede al tercer elemento)
    printf("%d\n", ptr[2]);   // Equivalente a *(ptr + 2)
    ```

*   **Punteros a punteros:**  Un puntero que apunta a otro puntero.
*   **Punteros a funciones:**  Un puntero que apunta a una función.
*  **`NULL`**:  Un puntero nulo (no apunta a ninguna dirección válida).

##  অ্যারে **Arrays (Arreglos)**

*   **Declaración:**

    ```c
    int numeros[5]; // Declara un array de 5 enteros
    float precios[10] = {1.99, 2.50, 3.75}; // Declara e inicializa
    char letras[] = {'a', 'b', 'c'}; // El tamaño se deduce de la inicialización
    ```

*   **Acceso a elementos:**  Usa corchetes `[]` y el índice (empezando desde 0).

    ```c
    numeros[0] = 10;
    printf("%d\n", numeros[2]);
    ```

*   **Arrays multidimensionales:**

    ```c
    int matriz[3][4]; // Matriz de 3 filas y 4 columnas
    matriz[0][0] = 1;
    ```
*   **Arrays y punteros:**  El nombre de un array es un puntero constante al primer elemento.

## 📝 **Cadenas de Caracteres (Strings)**

*   En C, las cadenas son *arrays de caracteres* terminados en el carácter nulo (`\0`).

    ```c
    char nombre[] = "Juan"; // El compilador añade '\0' automáticamente
    char saludo[10] = "Hola";
    char cadena[5] = {'H', 'o', 'l', 'a', '\0'};
    ```

*   **Funciones de la biblioteca `<string.h>`:**
    *   `strcpy(destino, origen)`:  Copia una cadena.  ¡Cuidado con el desbordamiento de búfer!
    *   `strncpy(destino, origen, n)`: Copia hasta `n` caracteres.
    *   `strcat(destino, origen)`:  Concatena cadenas.
    *   `strncat(destino, origen, n)`: Concatena hasta `n` caracteres.
    *   `strlen(cadena)`:  Devuelve la longitud de una cadena (sin contar el `\0`).
    *   `strcmp(cadena1, cadena2)`:  Compara dos cadenas.  Devuelve 0 si son iguales, un valor negativo si `cadena1` es menor que `cadena2`, y un valor positivo si `cadena1` es mayor que `cadena2`.
    *   `strncmp(cadena1, cadena2, n)`: Compara hasta `n` caracteres.
    *   `strchr(cadena, caracter)`:  Busca la primera ocurrencia de un carácter.
    *   `strrchr(cadena, caracter)`:  Busca la última ocurrencia de un carácter.
    *   `strstr(cadena, subcadena)`:  Busca una subcadena.
    *   `strtok(cadena, delimitadores)`:  Divide una cadena en tokens.
* **Lectura de cadenas con `scanf`**:
    *   `scanf("%s", buffer);`  ¡Peligroso! Puede causar desbordamiento de búfer.
    *   Es mejor usar `fgets()`.

        ```c
        char buffer[100];
        fgets(buffer, sizeof(buffer), stdin); // Lee una línea de la entrada estándar (incluyendo espacios)
        ```

## 🏗️ **Estructuras (`struct`)**

*   Permiten agrupar variables de diferentes tipos bajo un mismo nombre.

    ```c
    struct Punto {
      int x;
      int y;
    };

    struct Punto p1;  // Declara una variable de tipo Punto
    p1.x = 10;
    p1.y = 20;

    struct Punto p2 = {5, 8}; // Declara e inicializa

    struct Rectangulo {
        struct Punto supIzq;
        struct Punto infDer;
    }
    ```

*   **Acceso a miembros:**  Usa el operador `.` (punto).
*   **Punteros a estructuras:**  Usa el operador `->` (flecha).

    ```c
    struct Punto *ptr = &p1;
    printf("%d\n", ptr->x); // Accede a p1.x a través del puntero
    ```

*   **`typedef`:**  Crea un alias para un tipo de dato.

    ```c
    typedef struct Punto {
      int x;
      int y;
    } Punto; //Ahora puedes usar "Punto" en vez de "struct Punto"

    Punto p1;
    ```
* **Arrays de estructuras**:

## 🤝 **Uniones (`union`)**

*   Similar a una estructura, pero todos sus miembros *comparten la misma ubicación de memoria*.  Solo se puede usar un miembro a la vez.  El tamaño de una unión es el tamaño de su miembro más grande.

    ```c
    union Valor {
      int entero;
      float flotante;
      char caracter;
    };

    union Valor v;
    v.entero = 10;
    printf("%d\n", v.entero);
    v.flotante = 3.14; // Sobrescribe el valor anterior
    printf("%f\n", v.flotante);
    // printf("%d\n", v.entero); // Valor indefinido (se ha sobrescrito)
    ```

## 💾 **Gestión Dinámica de Memoria**

*   **`malloc(size_t size)`:**  Asigna un bloque de memoria de tamaño `size` (en bytes).  Devuelve un puntero a `void` (que se debe castear al tipo de dato deseado) o `NULL` si falla.
*   **`calloc(size_t num, size_t size)`:**  Asigna un bloque de memoria para `num` elementos, cada uno de tamaño `size` (en bytes).  Inicializa la memoria a cero.  Devuelve un puntero a `void` o `NULL` si falla.
*   **`realloc(void *ptr, size_t new_size)`:**  Redimensiona un bloque de memoria previamente asignado con `malloc` o `calloc`.  `ptr` es el puntero al bloque original, `new_size` es el nuevo tamaño.  Devuelve un puntero a `void` o `NULL` si falla.  Puede mover el bloque de memoria.
*   **`free(void *ptr)`:**  Libera un bloque de memoria previamente asignado con `malloc`, `calloc` o `realloc`.  *Muy importante* para evitar memory leaks.

```c
#include <stdlib.h> // Necesario para malloc, calloc, realloc, free

int main() {
  int *ptr;
  int n = 5;

  // Asignar memoria para 5 enteros
  ptr = (int *)malloc(n * sizeof(int)); // Castear el puntero void* a int*
  //También: ptr = (int *)calloc(n, sizeof(int));

  if (ptr == NULL) {
    printf("Error al asignar memoria.\n");
    return 1; // Salir con error
  }

  // Usar la memoria asignada
  for (int i = 0; i < n; i++) {
    ptr[i] = i + 1;
  }

  // Redimensionar la memoria (ej: para 10 enteros)
  ptr = (int *)realloc(ptr, 10 * sizeof(int));

  if (ptr == NULL) { //realloc puede fallar.
      printf("Error al realocar memoria");
      return 1;
  }

  // Liberar la memoria (¡siempre!)
  free(ptr);
  ptr = NULL; // Buena práctica: asignar NULL después de free

  return 0;
}
```

## ⚙️ **Preprocesador**

*   **`#include`:**  Incluye archivos de cabecera.
*   **`#define`:**  Define macros.

    ```c
    #define PI 3.14159
    #define CUADRADO(x) ((x) * (x)) // Macro con parámetros (¡cuidado con los paréntesis!)

    int main() {
      float area = PI * CUADRADO(5); // Se reemplaza por: PI * ((5) * (5))
      // ...
    }
    ```

*   **`#ifdef`, `#ifndef`, `#else`, `#endif`:**  Compilación condicional.

    ```c
    #define DEBUG

    #ifdef DEBUG
      printf("Modo debug activado.\n");
    #endif

    #ifndef MI_MACRO // Si no está definida MI_MACRO
    #define MI_MACRO 123
    #endif
    ```

* **`#if`, `#elif`, `#else`, `#endif`**: Compilación condicional (más flexible).
*  **`#error`**:  Emite un error de compilación.
*  **`#pragma`**:  Directivas específicas del compilador.
* **Macros predefinidas**: `__FILE__`, `__LINE__`, `__DATE__`, `__TIME__`, `__STDC__`, etc.

## 🔀 **`typedef`**

*   Crea un alias para un tipo de dato.

    ```c
    typedef int Entero; // Ahora "Entero" es sinónimo de "int"
    Entero x = 10;

    typedef struct {
      int x;
      int y;
    } Punto;

    Punto p1;
    ```

## 📂 **Entrada/Salida (I/O)**

*   **`stdio.h`:**
    *   **`printf()`:**  Imprime texto formateado en la salida estándar (consola).

        ```c
        printf("Hola, %s! Tienes %d años.\n", nombre, edad);
        ```

        *   **Especificadores de formato:**
            *   `%d`, `%i`:  Entero decimal con signo.
            *   `%u`:  Entero decimal sin signo.
            *   `%f`:  Número de punto flotante (float, double).
            *   `%lf`:  `double` (para `scanf`).
            *   `%c`:  Carácter.
            *   `%s`:  Cadena de caracteres.
            *   `%x`, `%X`:  Entero hexadecimal.
            *   `%o`:  Entero octal.
            *   `%p`:  Puntero (dirección de memoria).
            *   `%e`, `%E`:  Notación científica.
            *   `%g`, `%G`:  Formato más corto entre `%f` y `%e`/`%E`.
            *   `%%`:  Imprime un `%`.
        * **Modificadores de formato:**
            *  Ancho:  `%5d` (entero con ancho mínimo de 5 caracteres).
            *  Precisión: `%.2f` (número de punto flotante con 2 decimales).
            *  Alineación: `%-10s` (cadena alineada a la izquierda con ancho 10).
            *  Relleno: `%05d` (entero con ancho 5, relleno con ceros).

    *   **`scanf()`:**  Lee datos formateados de la entrada estándar (teclado).  ¡Cuidado con el desbordamiento de búfer!

        ```c
        int edad;
        char nombre[50];
        printf("Ingresa tu edad: ");
        scanf("%d", &edad); // ¡Importante el & para pasar la dirección de memoria!
        printf("Ingresa tu nombre: ");
        scanf("%s", nombre);  // ¡Peligroso! Puede desbordar el buffer.
        // Mejor usar fgets() para leer cadenas:
        fgets(nombre, sizeof(nombre), stdin);
        ```

    *   **`getchar()`:**  Lee un carácter de la entrada estándar.
    *   **`putchar()`:**  Escribe un carácter en la salida estándar.
    *   **`gets()`:**  Lee una línea de la entrada estándar (¡obsoleto y peligroso! No usar).
    *   **`puts()`:**  Escribe una cadena en la salida estándar (añade un salto de línea al final).
    *   **`fopen()`, `fclose()`, `fread()`, `fwrite()`, `fprintf()`, `fscanf()`, `fgetc()`, `fputc()`, `fgets()`, `fputs()`, `feof()`, `ferror()`, `rewind()`, `fseek()`, `ftell()`:**  Funciones para trabajar con archivos.

        ```c
        FILE *archivo;
        archivo = fopen("mi_archivo.txt", "r"); // Abre el archivo en modo lectura ("r", "w", "a", "rb", "wb", "ab", "r+", "w+", "a+")
        if (archivo == NULL) {
          perror("Error al abrir el archivo"); // Imprime un mensaje de error descriptivo
          return 1;
        }

        char buffer[256];
        while (fgets(buffer, sizeof(buffer), archivo) != NULL) { // Lee línea por línea
          printf("%s", buffer);
        }

        fclose(archivo); // Cierra el archivo (¡siempre!)
        ```

## 🚧 **Estructuras de Datos (Más Allá de lo Básico)**

*   **Listas enlazadas (Linked Lists):**  Estructuras de datos dinámicas donde cada elemento (nodo) contiene un valor y un puntero al siguiente elemento.
*   **Pilas (Stacks):**  Estructuras de datos LIFO (Last In, First Out).
*   **Colas (Queues):**  Estructuras de datos FIFO (First In, First Out).
*   **Árboles (Trees):**  Estructuras de datos jerárquicas.
*   **Grafos (Graphs):**  Estructuras de datos que representan relaciones entre nodos.
*   **Tablas hash (Hash Tables):**  Estructuras de datos que permiten búsquedas rápidas.

(La implementación de estas estructuras de datos generalmente implica el uso de `struct` y punteros).

## ⚙️ **Compilación**

*   **Compilador:**  GCC (GNU Compiler Collection) es el compilador de C más utilizado.
*   **Comando:**

    ```bash
    gcc mi_programa.c -o mi_programa  # Compila mi_programa.c y genera el ejecutable mi_programa
    ```

    *   `-o <nombre_ejecutable>`:  Especifica el nombre del archivo ejecutable.
    *   `-Wall`:  Habilita todas las advertencias (warnings).  ¡Muy recomendado!
    *   `-Werror`:  Convierte las advertencias en errores.
    *   `-g`:  Incluye información de depuración (para usar con un debugger como GDB).
    *   `-O0`, `-O1`, `-O2`, `-O3`, `-Os`:  Niveles de optimización.
    *   `-c`:  Compila a un archivo objeto (`.o`), pero no enlaza.
    *   `-l<biblioteca>`:  Enlaza con una biblioteca (ej: `-lm` para enlazar con la biblioteca matemática).
    * `-I<directorio>`: Añade un directorio a la ruta de búsqueda de includes.
    * `-L<directorio>`: Añade un directorio a la ruta de búsqueda de bibliotecas.
* **Compilación separada**: Compila cada archivo `.c` a un archivo objeto (`.o`), y luego enlaza los archivos `.o` para crear el ejecutable.