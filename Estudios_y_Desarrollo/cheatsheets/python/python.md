# 🐍 Cheatsheet de Python: Tu Guía Rápida 🚀

## 📝 **Variables y Tipos de Datos**

*   **Declaración:** No necesitas declarar el tipo explícitamente.
*   **Tipos Comunes:**

    *   `int`: Enteros 🔢 (ej: `x = 5`)
    *   `float`: Decimales  দশমিক (ej: `y = 3.14`)
    *   `str`: Cadenas de texto 🔤 (ej: `nombre = "Ana"`)
    *   `bool`: Booleanos (Verdadero/Falso) ✅❎ (ej: `es_valido = True`)
    *   `list`: Listas (colecciones ordenadas y modificables) 📑 (ej: `mi_lista = [1, 2, 3]`)
    *   `tuple`: Tuplas (colecciones ordenadas e inmutables) 📦 (ej: `mi_tupla = (1, 2, 3)`)
    *   `dict`: Diccionarios (pares clave-valor) 🔑: 🚪 (ej: `mi_diccionario = {"nombre": "Juan", "edad": 30}`)
    *   `set`: Conjuntos (colecciones no ordenadas de elementos únicos) 🧩 (ej: `mi_set = {1, 2, 3}`)
*   **Conversión de Tipos:**

    *   `int(x)`: Convierte `x` a entero.
    *   `float(x)`: Convierte `x` a decimal.
    *   `str(x)`: Convierte `x` a cadena.
    *   `bool(x)`: Convierte `x` a booleano (0 es `False`, otros valores son `True`).
    *   `list(x)`: Convierte `x` a lista.
    *   `tuple(x)`: Convierte `x` a tupla.
    *   `dict(x)`: Intenta convertir `x` a diccionario (ej: lista de tuplas de pares).
    *   `set(x)`: Convierte `x` a conjunto.
*   **Comprobar tipo**
    *   `type(x)`: Retorna el tipo de dato de una variable

## 🧮 **Operadores**

*   **Aritméticos:** `+` (suma), `-` (resta), `*` (multiplicación), `/` (división), `//` (división entera), `%` (módulo/resto), `**` (potencia).
*   **Comparación:** `==` (igual), `!=` (diferente), `>` (mayor que), `<` (menor que), `>=` (mayor o igual), `<=` (menor o igual).
*   **Lógicos:** `and` (y), `or` (o), `not` (no).
*   **Asignación:** `=`, `+=`, `-=`, `*=`, `/=`, `//=`, `%=`, `**=`.  (ej: `x += 5` es lo mismo que `x = x + 5`).
*   **Identidad**: `is`, `is not` (comprueba si dos variables se refieren al *mismo* objeto, no solo si tienen el mismo valor).
*   **Pertenencia**: `in`, `not in` (comprueba si un valor está presente en una secuencia).

## 🚦 **Control de Flujo (Condicionales y Bucles)**

*   **`if`-`elif`-`else`:**

    ```python
    if condicion1:
        # Bloque si condicion1 es verdadera
    elif condicion2:
        # Bloque si condicion2 es verdadera
    else:
        # Bloque si ninguna condición es verdadera
    ```

*   **`for` (bucle definido):** Itera sobre los elementos de una secuencia (lista, tupla, cadena, rango, etc.).

    ```python
    for elemento in secuencia:
        # Haz algo con cada elemento
    
    # Ejemplo con rango:
    for i in range(5):  # Itera de 0 a 4
        print(i)

    # Iterar con indices
    for index, value in enumerate(my_list):
    ```

*   **`while` (bucle indefinido):** Se ejecuta mientras una condición sea verdadera.

    ```python
    while condicion:
        # Se ejecuta mientras la condición sea verdadera
    ```
* **`break`:** Sale del bucle inmediatamente.
* **`continue`:** Salta a la siguiente iteración del bucle.
* **`pass`:** No hace nada (útil como marcador de posición en bloques que aún no has implementado).

## 📦 **Funciones**

*   **Definición:**

    ```python
    def nombre_funcion(parametro1, parametro2=valor_predeterminado):
        """Docstring:  Describe lo que hace la función."""
        # Cuerpo de la función
        return valor  # Opcional:  devuelve un valor
    ```

*   **Llamada:** `nombre_funcion(argumento1, argumento2)`

*   **Parámetros por defecto:** Permite especificar valores predeterminados para los parámetros, haciéndolos opcionales al llamar a la función.

*   **`return`:**  Devuelve un valor de la función (si no hay `return`, la función devuelve `None` implícitamente).

*   **Funciones Lambda (Anónimas):**

    ```python
    mi_funcion_lambda = lambda x, y: x + y  # Función que suma dos números
    resultado = mi_funcion_lambda(3, 4)  # resultado será 7
    ```
    Son funciones pequeñas, de una sola línea, sin nombre (anónimas), definidas con la palabra clave `lambda`.  Muy útiles para funciones simples que se pasan como argumentos a otras funciones (ej: `map`, `filter`, `sort`).

## 📑 **Listas**

*   **Creación:** `mi_lista = [1, 2, "tres", 4.0]`
*   **Acceso:**
    *   `mi_lista[0]` (primer elemento, índice 0)
    *   `mi_lista[-1]` (último elemento)
    *   `mi_lista[1:3]` (slicing: elementos desde el índice 1 hasta el 3 *sin incluir* el 3)
    *   `mi_lista[:3]` (desde el inicio hasta el índice 3 sin incluirlo)
    *   `mi_lista[2:]` (desde el índice 2 hasta el final)
    *   `mi_lista[:]` (copia completa de la lista)
*   **Modificación:**
    *   `mi_lista[0] = 10` (cambia el primer elemento)
    *   `mi_lista.append(5)` (agrega un elemento al final)
    *   `mi_lista.insert(1, "nuevo")` (inserta "nuevo" en la posición 1)
    *   `mi_lista.extend([6, 7, 8])` (agrega varios elementos al final)
    *   `del mi_lista[2]` (elimina el elemento en la posición 2)
    *   `mi_lista.remove("tres")` (elimina la primera ocurrencia de "tres")
    *   `elemento = mi_lista.pop(1)` (elimina y devuelve el elemento en la posición 1; si no se especifica el índice, elimina y devuelve el último elemento)
*   **Métodos Útiles:**
    *   `len(mi_lista)` (longitud de la lista)
    *   `mi_lista.sort()` (ordena la lista en su lugar)
    *   `nueva_lista = sorted(mi_lista)` (devuelve una *nueva* lista ordenada, sin modificar la original)
    *   `mi_lista.reverse()` (invierte el orden de la lista en su lugar)
    *   `mi_lista.count(2)` (cuenta cuántas veces aparece el elemento 2)
    *   `mi_lista.index("tres")` (devuelve el índice de la primera ocurrencia de "tres"; lanza un error si no se encuentra)
    *   `2 in mi_lista` (verifica si 2 está en la lista, devuelve `True` o `False`)
    *   `mi_lista.copy()`(retorna una copia *superficial* de la lista)

## 📦 **Tuplas**

*   **Creación:** `mi_tupla = (1, 2, "tres", 4.0)`  (o sin paréntesis: `mi_tupla = 1, 2, "tres"`)
*   **Acceso:** Igual que las listas (indexación y slicing).
*   **Inmutabilidad:**  ¡No se pueden modificar!  (No puedes agregar, eliminar ni cambiar elementos).  Esto las hace más eficientes en memoria que las listas en algunos casos.
*   **Métodos:**  Muchos menos métodos que las listas (debido a la inmutabilidad).  Principalmente `count()` e `index()`.
*   **Uso común**:  Para representar datos que no deben cambiar, como coordenadas, valores de configuración, etc.  También se usan para devolver múltiples valores desde una función.

## 🔑:🚪 **Diccionarios**

*   **Creación:** `mi_diccionario = {"nombre": "Juan", "edad": 30, "ciudad": "Madrid"}`
*   **Acceso:**
    *   `mi_diccionario["nombre"]` (accede al valor asociado con la clave "nombre")
    *   `mi_diccionario.get("edad")` (otra forma de acceder; si la clave no existe, devuelve `None` en lugar de lanzar un error; puedes proporcionar un valor predeterminado: `mi_diccionario.get("telefono", "No disponible")`)
*   **Modificación:**
    *   `mi_diccionario["edad"] = 31` (cambia el valor asociado con "edad")
    *   `mi_diccionario["profesion"] = "Ingeniero"` (agrega un nuevo par clave-valor)
    *   `del mi_diccionario["ciudad"]` (elimina el par clave-valor con clave "ciudad")
    *   `valor = mi_diccionario.pop("edad")` (elimina y devuelve el valor asociado con "edad")
*   **Métodos Útiles:**
    *   `len(mi_diccionario)` (número de pares clave-valor)
    *   `mi_diccionario.keys()` (devuelve una "vista" de las claves)
    *   `mi_diccionario.values()` (devuelve una "vista" de los valores)
    *   `mi_diccionario.items()` (devuelve una "vista" de tuplas (clave, valor))
    *   `"nombre" in mi_diccionario` (verifica si la clave "nombre" existe)
    *  `mi_diccionario.update(otro_diccionario)`:  Añade o actualiza los pares clave-valor de `otro_diccionario` en `mi_diccionario`.
    *   `mi_diccionario.copy()`(retorna una copia *superficial* del diccionario)
    *  `mi_diccionario.clear()`:  Elimina todos los elementos del diccionario.
    *   `nuevo_diccionario = dict.fromkeys(claves, valor_predeterminado)`:  Crea un nuevo diccionario a partir de una lista de `claves`, asignando a todas ellas el mismo `valor_predeterminado`.

## 🧩 **Conjuntos (Sets)**

* **Creación:** `mi_set = {1, 2, 3, 2, 1}` (los elementos repetidos se ignoran; el set resultante será `{1, 2, 3}`)
* **Características:**
    *   *No* son ordenados.
    *   *No* permiten elementos duplicados.
    *   Son *mutables* (puedes agregar y quitar elementos).
    *   Los elementos *deben ser inmutables* (no puedes tener listas o diccionarios dentro de un set, pero sí tuplas).
* **Métodos:**
  *  `mi_set.add(4)`:  Añade un elemento.
    *   `mi_set.remove(3)` (elimina un elemento; lanza un error si no existe)
    *   `mi_set.discard(3)` (elimina un elemento si existe; no hace nada si no existe)
    *   `elemento = mi_set.pop()` (elimina y devuelve un elemento arbitrario; lanza un error si el set está vacío)
* **Operaciones de Conjuntos:**
    *   `union = set1 | set2` (unión: elementos en `set1` o `set2` o ambos)
    *   `interseccion = set1 & set2` (intersección: elementos en `set1` *y* `set2`)
    *   `diferencia = set1 - set2` (diferencia: elementos en `set1` pero *no* en `set2`)
    *   `diferencia_simetrica = set1 ^ set2` (diferencia simétrica: elementos en `set1` o `set2`, pero *no* en ambos)
*   `elemento in mi_set`:  Comprueba la pertenencia de un elemento (más eficiente que en listas).
*   `len(mi_set)`: Número de elementos.
* `mi_set.copy()`(retorna una copia *superficial* del set)
*  `mi_set.clear()`:  Elimina todos los elementos.
*   `set1.issubset(set2)`:  Comprueba si `set1` es un subconjunto de `set2`.
*   `set1.issuperset(set2)`:  Comprueba si `set1` es un superconjunto de `set2`.
*   `set1.isdisjoint(set2)`: Comprueba si la intersección es vacía

## 🧵 **Cadenas (Strings)**

*   **Creación:** `mi_cadena = "Hola, mundo!"`  o  `mi_cadena = 'Hola, mundo!'`
*   **Acceso:**  Similar a listas (indexación y slicing).  ¡Pero son *inmutables*!
*   **Métodos (¡Muchos!):**
    *   `len(mi_cadena)` (longitud)
    *   `mi_cadena.upper()` (a mayúsculas)
    *   `mi_cadena.lower()` (a minúsculas)
    *   `mi_cadena.capitalize()` (primera letra en mayúscula)
    *   `mi_cadena.title()` (cada palabra empieza en mayúscula)
    *   `mi_cadena.strip()` (quita espacios en blanco al principio y al final; `lstrip` y `rstrip` solo quitan de un lado)
    *   `mi_cadena.split(",")` (divide la cadena en una lista de subcadenas, usando "," como separador; si no se especifica el separador, se usan espacios en blanco)
    *   `" ".join(lista_de_cadenas)` (une una lista de cadenas en una sola cadena, usando " " como separador)
    *   `mi_cadena.find("mundo")` (devuelve el índice de la primera ocurrencia de "mundo", o -1 si no se encuentra)
    *   `mi_cadena.replace("Hola", "Adiós")` (reemplaza "Hola" por "Adiós")
    *   `mi_cadena.startswith("Hola")` (devuelve `True` si la cadena empieza con "Hola")
    *   `mi_cadena.endswith("!")` (devuelve `True` si la cadena termina con "!")
    *   `mi_cadena.isdigit()` (devuelve `True` si todos los caracteres son dígitos)
    *   `mi_cadena.isalpha()` (devuelve `True` si todos los caracteres son letras)
    *   `mi_cadena.isalnum()` (devuelve `True` si todos los caracteres son letras o números)
    *   `mi_cadena.isspace()` (devuelve `True` si todos los caracteres son espacios en blanco)
*   **Formateo de Cadenas:**

    *   **f-strings (la forma más moderna y recomendada):**

        ```python
        nombre = "Ana"
        edad = 25
        print(f"Hola, me llamo {nombre} y tengo {edad} años.")
        print(f"El doble de mi edad es {edad * 2:.2f}")  # Formato:  .2f (dos decimales)
        ```

    *   **`.format()` (método más antiguo):**

        ```python
        print("Hola, me llamo {} y tengo {} años.".format(nombre, edad))
        print("El doble de mi edad es {:.2f}".format(edad * 2))
        ```

    *   **`%` (estilo antiguo de C, menos recomendado):**

        ```python
        print("Hola, me llamo %s y tengo %d años." % (nombre, edad))
        print("El doble de mi edad es %.2f" % (edad * 2))
        ```

## 📂 **Manejo de Archivos**

*   **Abrir un archivo:**

    ```python
    archivo = open("mi_archivo.txt", "r")  # "r" = leer, "w" = escribir (sobrescribe), "a" = añadir, "x" = crear (falla si existe)
                                        # "t" = texto (modo por defecto), "b" = binario
                                        # "+" = actualizar (lectura y escritura)
    ```
    *   Es *fundamental* cerrar el archivo después de usarlo:  `archivo.close()`
    *   **Mejor práctica: usar `with` (context manager):**  El archivo se cierra automáticamente, incluso si hay errores.

        ```python
        with open("mi_archivo.txt", "r") as archivo:
            contenido = archivo.read()  # Lee todo el archivo
            # Otras opciones:
            #   linea = archivo.readline()  # Lee una línea
            #   lineas = archivo.readlines()  # Lee todas las líneas y las devuelve como una lista de cadenas

        # El archivo ya está cerrado aquí
        print(contenido)
        ```

*   **Escribir en un archivo:**

    ```python
    with open("mi_archivo.txt", "w") as archivo:  # "w" sobrescribe el archivo
        archivo.write("Este es el nuevo contenido.\n")  # Escribe una cadena
        archivo.writelines(["Línea 1\n", "Línea 2\n"])  # Escribe una lista de cadenas

    with open("mi_archivo.txt", "a") as archivo: # "a" añade al final sin sobrescribir
      archivo.write("Añado una línea")

    ```

## ❗ **Manejo de Excepciones (Errores)**

*   **`try`-`except`-`else`-`finally`:**

    ```python
    try:
        # Código que podría lanzar una excepción
        resultado = 10 / 0  # Esto causará un ZeroDivisionError
    except ZeroDivisionError:
        # Código que se ejecuta si ocurre un ZeroDivisionError
        print("Error: División por cero.")
    except (TypeError, ValueError) as e:
        # Maneja TypeError y ValueError juntos
        print(f"Ocurrió un error: {e}")
    except Exception as e:
        # Código que se ejecuta si ocurre *cualquier otra* excepción
        print(f"Ocurrió un error inesperado: {e}")
    else:
        # Código que se ejecuta *si no* ocurrió ninguna excepción en el bloque try
        print("La división se realizó correctamente.")
    finally:
        # Código que se ejecuta *siempre*, haya ocurrido o no una excepción
        print("Este código siempre se ejecuta.")
    ```

    *   Puedes capturar excepciones específicas (como `ZeroDivisionError`, `TypeError`, `FileNotFoundError`, etc.) o excepciones más generales (como `Exception`).
    * Puedes tener múltiples bloques `except` para manejar diferentes tipos de excepciones.
    * `else` se usa con poca frecuencia.
    *  `raise`:  Puedes lanzar tus propias excepciones.  Ejemplo:  `raise ValueError("El valor no puede ser negativo")`

## 📚 **Módulos e Importación**
* **Importar módulos**
  *   `import math` (importa el módulo completo)
  *   `import math as m` (importa el módulo y le da un alias)
  *   `from math import sqrt, pi` (importa funciones o variables específicas del módulo)
  *   `from math import *` (importa todo del módulo, no recomendado)
* **Crear tus propios módulos:**
    *   Simplemente crea un archivo `.py` con tu código.
    *   Para usarlo, importa el archivo (sin la extensión `.py`) desde otro script de Python en el mismo directorio (o en un directorio en `sys.path`).

## ⚙️ **Clases y Objetos (Programación Orientada a Objetos - POO)**

```python
class Perro:  # Define la clase "Perro"
    # Método especial "__init__" (constructor): se llama al crear un nuevo objeto
    def __init__(self, nombre, raza):
        self.nombre = nombre  # "self" se refiere al objeto actual
        self.raza = raza
        self._atributo_privado = "valor" #Atributo "privado" (convención)
        self.__atributo_muy_privado = "valor" #Atributo *más* "privado" (name mangling)

    # Métodos de la clase
    def ladrar(self):
        print("Guau!")

    def presentar(self):
        print(f"Hola, soy {self.nombre} y soy de raza {self.raza}.")

# Crear objetos (instancias) de la clase
mi_perro = Perro("Fido", "Labrador")
otro_perro = Perro("Luna", "Golden Retriever")

# Acceder a los atributos
print(mi_perro.nombre)  # Salida: Fido
print(otro_perro.raza)  # Salida: Golden Retriever

# Llamar a los métodos
mi_perro.ladrar()  # Salida: Guau!
otro_perro.presentar()  # Salida: Hola, soy Luna y soy de raza Golden Retriever.

# Herencia
class PerroGuia(Perro): #PerroGuia *hereda* de Perro
    def __init__(self, nombre, raza, escuela):
        super().__init__(nombre, raza) #Llama al constructor de la clase padre
        self.escuela = escuela

    def guiar(self): #Nuevo método
        print("Guiando...")

perro_guia = PerroGuia("Buddy", "Pastor Alemán", "ONCE")
perro_guia.ladrar() #Funciona (heredado)
perro_guia.guiar() #Funciona (propio)

```

## 📦 **Bibliotecas Comunes (¡Hay muchas más!)**

*   **`math`:** Funciones matemáticas (trigonométricas, logarítmicas, etc.).
*   **`random`:** Generación de números aleatorios.
*   **`datetime`:** Manipulación de fechas y horas.
*   **`os`:** Interacción con el sistema operativo (rutas de archivos, variables de entorno, etc.).
*   **`sys`:** Acceso a parámetros y funciones específicas del sistema (argumentos de línea de comandos, etc.).
*   **`re`:** Expresiones regulares (búsqueda y manipulación de texto basada en patrones).
*   **`json`:** Codificación y decodificación de datos en formato JSON.
*   **`requests`:** Realizar solicitudes HTTP (para interactuar con APIs web).
*   **`numpy`:** Computación numérica (arrays multidimensionales, álgebra lineal, etc.).  *Fundamental* para ciencia de datos.
*   **`pandas`:** Análisis y manipulación de datos (dataframes, series temporales, etc.).  *Fundamental* para ciencia de datos.
*   **`matplotlib`:** Creación de gráficos y visualizaciones.
*   **`scikit-learn`:** Aprendizaje automático (machine learning).

## 💡 **Comprensiones de Listas, Diccionarios y Conjuntos (List, Dict and Set Comprehensions)**

*   **Listas:**  Forma concisa de crear listas.

    ```python
    numeros = [1, 2, 3, 4, 5]
    cuadrados = [x**2 for x in numeros]  # [1, 4, 9, 16, 25]
    pares = [x for x in numeros if x % 2 == 0]  # [2, 4]
    ```

*   **Diccionarios:**

    ```python
    nombres = ["Juan", "Ana", "Pedro"]
    edades = [30, 25, 40]
    diccionario = {nombre: edad for nombre, edad in zip(nombres, edades)}  # {'Juan': 30, 'Ana': 25, 'Pedro': 40}
    diccionario_cuadrados = {x: x**2 for x in range(5)} # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

    ```

*   **Conjuntos:**

    ```python
    numeros = [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]
    conjunto_pares = {x for x in numeros if x % 2 == 0}  # {2, 4}
    ```

## 🐍 **Decoradores**

*   Funciones que modifican el comportamiento de otras funciones.

    ```python
    def mi_decorador(funcion):
        def wrapper(*args, **kwargs):  # *args y **kwargs permiten pasar cualquier número de argumentos posicionales y de palabra clave
            print("Antes de llamar a la función.")
            resultado = funcion(*args, **kwargs)
            print("Después de llamar a la función.")
            return resultado
        return wrapper

    @mi_decorador  # Aplica el decorador a la función "saludar"
    def saludar(nombre):
        print(f"Hola, {nombre}!")

    saludar("Mundo")  # Imprime:  Antes de llamar a la función.  Hola, Mundo!  Después de llamar a la función.
    ```

## 🐍 **Generadores**
* Los generadores son un tipo especial de iterador.  Se definen como funciones, pero en lugar de usar `return`, usan `yield`.
*   `yield` devuelve un valor *y* pausa la ejecución de la función, guardando su estado.  La próxima vez que se llame al generador, la ejecución continúa desde donde se quedó.
* Son muy eficientes en memoria, ya que no generan todos los valores a la vez (como una lista), sino que los generan uno por uno, *bajo demanda*.

```python
def generador_numeros(n):
  for i in range(n):
    yield i

gen = generador_numeros(5)
print(next(gen))  # 0
print(next(gen))  # 1
for num in gen:  # Continua desde donde se quedo
    print(num)  # 2, 3, 4
```

## 🐍 **Context Managers (with statement)**

* Ya lo hemos visto con archivos, pero es un concepto más general.
*  Un context manager define qué debe ocurrir *antes* y *después* de la ejecución de un bloque de código.
*   Se usan con la palabra clave `with`.
*  El ejemplo clásico es el manejo de archivos (asegurándose de que el archivo se cierre), pero se pueden usar para muchas otras cosas (conexiones a bases de datos, bloqueos de concurrencia, etc.).

```python
class MiContextManager:
    def __enter__(self):
        print("Entrando en el contexto...")
        return "Valor de retorno de __enter__"

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Saliendo del contexto...")
        if exc_type:  # Si hubo una excepción
            print(f"  Tipo de excepción: {exc_type}")
            print(f"  Valor de la excepción: {exc_val}")
            # Si __exit__ devuelve True, la excepción se suprime.
            # Si devuelve False (o None), la excepción se propaga.
        return False

with MiContextManager() as valor:
    print(f"Dentro del bloque with.  Valor: {valor}")
    # raise ValueError("Error provocado")  # Descomenta para probar el manejo de excepciones
print("Fuera del bloque with")
```
