# 🐍 Cheatsheet de Python Avanzado: Dominando el Poder 🚀

## 🧙 **Funciones de Orden Superior y Lambdas**

*   **Funciones de Orden Superior:** Son funciones que:
    *   Reciben una o más funciones como argumentos.
    *   O devuelven una función como resultado.

*   **`map(funcion, iterable)`:** Aplica `funcion` a cada elemento de `iterable` y devuelve un iterador con los resultados.

    ```python
    numeros = [1, 2, 3, 4]
    cuadrados = list(map(lambda x: x**2, numeros))  # [1, 4, 9, 16]
    ```

*   **`filter(funcion, iterable)`:** Aplica `funcion` a cada elemento de `iterable`.  Devuelve un iterador con los elementos para los cuales `funcion` devuelve `True`.

    ```python
    numeros = [1, 2, 3, 4, 5, 6]
    pares = list(filter(lambda x: x % 2 == 0, numeros))  # [2, 4, 6]
    ```

*   **`reduce(funcion, iterable[, inicializador])`:** Aplica `funcion` acumulativamente a los elementos de `iterable`, de izquierda a derecha, reduciéndolos a un solo valor.  `functools.reduce`

    ```python
    from functools import reduce

    numeros = [1, 2, 3, 4]
    producto = reduce(lambda x, y: x * y, numeros)  # 1 * 2 * 3 * 4 = 24
    suma = reduce(lambda x, y: x + y, numeros, 10)   # 10 + 1 + 2 + 3 + 4 = 20 (con valor inicial)
    ```

*   **`sorted(iterable, key=funcion, reverse=False)`:** Devuelve una *nueva* lista ordenada.  `key` es una función que se aplica a cada elemento para determinar el orden.

    ```python
    palabras = ["hola", "adiós", "mundo", "python"]
    ordenadas_por_longitud = sorted(palabras, key=len)  # ['hola', 'mundo', 'adiós', 'python']
    ordenadas_inverso = sorted(palabras, reverse=True) #['python', 'mundo', 'hola', 'adiós']

    personas = [("Ana", 25), ("Juan", 30), ("Pedro", 20)]
    ordenadas_por_edad = sorted(personas, key=lambda x: x[1])  # [('Pedro', 20), ('Ana', 25), ('Juan', 30)]
    ```

## 🧵 **Concurrencia y Paralelismo**

*   **Multiprocessing (Múltiples Procesos):**
    *   Usa la biblioteca `multiprocessing`.
    *   Crea procesos separados, cada uno con su propio intérprete de Python y espacio de memoria.
    *   Ideal para tareas que están *limitadas por la CPU* (CPU-bound), como cálculos intensivos.
    *   Evita el *Global Interpreter Lock (GIL)* de Python, permitiendo verdadera ejecución paralela en múltiples núcleos.

    ```python
    from multiprocessing import Process, Pool, Value, Array

    def cuadrado(n, resultado, indice):
        resultado[indice] = n * n

    if __name__ == '__main__':  # Importante para evitar problemas en Windows
        numeros = [1, 2, 3, 4, 5]
        #Usando Process
        procesos = []
        resultados = Array('i', len(numeros)) #Array compartido entre procesos. 'i'=entero
        for i, num in enumerate(numeros):
            p = Process(target=cuadrado, args=(num, resultados, i))
            procesos.append(p)
            p.start()
        for p in procesos:
            p.join()  # Espera a que todos los procesos terminen
        print(list(resultados))

        #Usando Pool
        with Pool(processes=4) as pool:  # Crea un pool de 4 procesos
            resultados = pool.map(lambda x: x*x, numeros)
            print(resultados)

        #Compartiendo un valor simple:
        contador = Value('i', 0) # 'i' = entero
        # ... (dentro de los procesos, usar contador.value para acceder y modificar)

    ```

*   **Threading (Múltiples Hilos):**

    *   Usa la biblioteca `threading`.
    *   Crea hilos dentro del *mismo* proceso.  Comparten el mismo espacio de memoria.
    *   Ideal para tareas que están *limitadas por E/S* (I/O-bound), como esperar respuestas de la red o leer archivos.
    *   *No* evita el GIL, por lo que no hay verdadera ejecución paralela en múltiples núcleos para tareas CPU-bound.
    *  Más ligero que los procesos (menor sobrecarga).

    ```python
    import threading
    import time

    def tarea(nombre):
        print(f"Hilo {nombre}: Iniciando")
        time.sleep(2)  # Simula una tarea que tarda un tiempo
        print(f"Hilo {nombre}: Terminando")

    hilos = []
    for i in range(3):
        hilo = threading.Thread(target=tarea, args=(i,)) #No pasar como tupla si es un solo arg
        hilos.append(hilo)
        hilo.start()

    for hilo in hilos:
        hilo.join() #Espera a que los hilos acaben

    print("Todos los hilos han terminado.")
    ```
*   **`asyncio` (Programación Asíncrona):**
    *   Permite escribir código concurrente que se ejecuta en un *solo hilo*, pero que puede realizar múltiples tareas de forma no bloqueante.
    *   Usa las palabras clave `async` y `await`.
    *   Ideal para tareas I/O-bound, especialmente cuando hay muchas operaciones de red o E/S que pueden realizarse simultáneamente.
    *   Ofrece un mejor rendimiento que el threading en muchos casos de uso de E/S.

    ```python
    import asyncio
    import time

    async def tarea(nombre):
        print(f"Tarea {nombre}: Iniciando")
        await asyncio.sleep(2)  # Espera no bloqueante
        print(f"Tarea {nombre}: Terminando")

    async def main():
        tareas = [tarea(i) for i in range(3)]
        await asyncio.gather(*tareas)  # Ejecuta las tareas concurrentemente

    #Usar asyncio.run solo para ejecutar el "main"
    asyncio.run(main())
    ```

## 🔗 **Redes (Sockets y APIs)**

* **Sockets (Bajo Nivel):**
  * Comunicación entre procesos, ya sea en la misma máquina o a través de una red.
    ```python
    import socket

    # Servidor (TCP)
    HOST = '127.0.0.1'  # localhost
    PORT = 65432

    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.bind((HOST, PORT))
        s.listen()
        conn, addr = s.accept()  # Espera una conexión
        with conn:
            print('Conectado por', addr)
            while True:
                data = conn.recv(1024)  # Recibe datos (hasta 1024 bytes)
                if not data:
                    break
                conn.sendall(data)  # Envía los mismos datos de vuelta


    # Cliente (TCP)
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((HOST, PORT))
        s.sendall(b'Hola, servidor!') #Envía bytes, no strings
        data = s.recv(1024)

    print('Recibido:', repr(data))
    ```

*   **`requests` (Alto Nivel - para APIs HTTP):**

    ```python
    import requests

    respuesta = requests.get("https://www.ejemplo.com")
    print(respuesta.status_code)  # Código de estado HTTP (200 = OK)
    print(respuesta.headers)     # Encabezados de la respuesta
    print(respuesta.text)        # Contenido de la respuesta (como texto)
    # print(respuesta.json())   # Si la respuesta es JSON, la convierte en un diccionario de Python

    #Enviar datos
    respuesta = requests.post("https://api.ejemplo.com/datos", data={"clave": "valor"}) #Datos como diccionario
    respuesta = requests.post("https://api.ejemplo.com/datos", json={"clave": "valor"}) #Envía como JSON
    respuesta = requests.get("https://api.ejemplo.com/datos", params={"param1": "valor1"}) #Parámetros en la URL
    ```
## 🔬 **Metaprogramación**

*   **Metaclases:**  "Clases de clases".  Permiten controlar la creación de clases.

    ```python
    class MiMetaclase(type):  # Hereda de "type"
        def __new__(cls, nombre, bases, diccionario):
            # Personaliza la creación de la clase
            diccionario['atributo_nuevo'] = 123  # Añade un atributo
            print(f"Creando clase: {nombre}")
            return super().__new__(cls, nombre, bases, diccionario)

    class MiClase(metaclass=MiMetaclase):  # Usa la metaclase
        pass

    obj = MiClase()
    print(obj.atributo_nuevo)  # 123
    ```

*   **Decoradores (ya vistos, pero son una forma de metaprogramación).**
*   **`getattr`, `setattr`, `hasattr`, `delattr`:**  Funciones para manipular atributos de objetos dinámicamente.

    ```python
    class MiClase:
        def __init__(self):
            self.x = 10

    obj = MiClase()
    print(hasattr(obj, 'x'))        # True
    print(getattr(obj, 'x'))        # 10
    setattr(obj, 'y', 20)           # obj.y = 20
    print(getattr(obj, 'y'))        # 20
    delattr(obj, 'x')               # del obj.x
    print(hasattr(obj, 'x'))        # False
    ```

*   **`eval`, `exec`:**  Ejecutan código Python como cadenas (¡usar con *extremo cuidado*!).

    ```python
    x = 1
    resultado = eval("x + 1")  # resultado será 2
    print(resultado)
    codigo = """
    def mi_funcion():
        print("Hola desde exec!")
    """
    exec(codigo)
    mi_funcion()  # Llama a la función definida en la cadena
    ```

## 🧪 **Bibliotecas Científicas y de Datos (Ecosistema SciPy)**

*   **NumPy (Fundamental):**

    ```python
    import numpy as np

    # Creación de arrays
    arr = np.array([1, 2, 3])  # A partir de una lista
    arr_ceros = np.zeros((2, 3))  # Array de ceros con forma (2, 3)
    arr_unos = np.ones((3, 2))   # Array de unos
    arr_aleatorio = np.random.rand(2, 2)  # Números aleatorios entre 0 y 1
    arr_enteros = np.arange(1, 10, 2) #Array con valores entre 1 y 10 (sin incluirlo) y paso de 2

    # Operaciones
    a = np.array([1, 2, 3])
    b = np.array([4, 5, 6])
    suma = a + b  # Suma elemento a elemento
    producto = a * b # Producto
    producto_punto = a.dot(b) #Producto escalar
    matriz = np.array([[1, 2], [3, 4]])
    transpuesta = matriz.T

    # Indexación y slicing
    print(arr[0])  # Primer elemento
    print(matriz[0, 1])  # Elemento en la fila 0, columna 1
    print(matriz[:, 1])  # Todas las filas, columna 1
    print(matriz[0, :])  # Fila 0, todas las columnas
    bool_idx = matriz > 2 #Devuelve un array de booleanos
    print(matriz[bool_idx]) #Devuelve solo los elementos mayores que 2

    # Broadcasting: Operaciones entre arrays de diferentes formas (NumPy lo maneja automáticamente en muchos casos)
    c = np.array([1, 2, 3])
    d = 2  # Escalar
    resultado = c + d  # [3, 4, 5] (el escalar "se expande" para coincidir con la forma del array)

    # Funciones universales (ufunc): Se aplican elemento a elemento
    arr = np.array([1, 4, 9])
    raiz_cuadrada = np.sqrt(arr)  # [1., 2., 3.]
    ```

*   **Pandas (Análisis de Datos):**

    ```python
    import pandas as pd

    # Series (array unidimensional con etiquetas)
    s = pd.Series([1, 3, 5, np.nan, 6, 8])

    # DataFrames (tabla bidimensional)
    data = {'Nombre': ['Juan', 'Ana', 'Pedro'],
            'Edad': [30, 25, 40],
            'Ciudad': ['Madrid', 'Barcelona', 'Valencia']}
    df = pd.DataFrame(data)

    # Leer datos de archivos (CSV, Excel, SQL, etc.)
    # df = pd.read_csv("mi_archivo.csv")
    # df = pd.read_excel("mi_archivo.xlsx")

    # Acceso a datos
    print(df['Nombre'])  # Columna 'Nombre'
    print(df.Edad)     # Otra forma de acceder a una columna
    print(df.iloc[0])   # Primera fila (por índice numérico)
    print(df.loc[0])    # Primera fila (por etiqueta de índice, si la hay)
    print(df[df['Edad'] > 28])  # Filas donde la edad es mayor que 28
    print(df.describe()) #Estadísticas descriptivas de las columnas numéricas

    # Manipulación de datos
    df['NuevaColumna'] = [1, 2, 3]
    df = df.drop('Ciudad', axis=1)  # Elimina la columna 'Ciudad' (axis=1 indica columnas)
    df = df.dropna()  # Elimina filas con valores faltantes (NaN)
    df = df.fillna(0) #Rellena valores NaN
    df['Edad_doble'] = df['Edad'].apply(lambda x: x * 2)  # Aplica una función a una columna
    df_agrupado = df.groupby('Nombre')['Edad'].mean()  # Agrupa por 'Nombre' y calcula la media de 'Edad'
    ```

*   **Matplotlib (Visualización):**

    ```python
    import matplotlib.pyplot as plt

    # Gráfico de líneas
    x = [1, 2, 3, 4, 5]
    y = [2, 4, 1, 3, 5]
    plt.plot(x, y)
    plt.xlabel("Eje X")
    plt.ylabel("Eje Y")
    plt.title("Mi Gráfico")
    plt.show()

    # Histograma
    datos = [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]
    plt.hist(datos, bins=5)  # bins: número de barras
    plt.show()

    # Gráfico de dispersión (scatter plot)
    x = [1, 2, 3, 4, 5]
    y = [2, 4, 1, 3, 5]
    plt.scatter(x, y)
    plt.show()

    # Gráfico de barras
    categorias = ['A', 'B', 'C']
    valores = [10, 5, 8]
    plt.bar(categorias, valores)
    plt.show()
    #Gráfico circular
    plt.pie(valores, labels = categorias)
    plt.show()
    ```

*  **Scikit-learn (Aprendizaje Automático):**

    ```python
    from sklearn.model_selection import train_test_split
    from sklearn.linear_model import LinearRegression
    from sklearn.metrics import mean_squared_error
    from sklearn import datasets

    # Cargar un conjunto de datos de ejemplo (diabetes)
    diabetes = datasets.load_diabetes()
    X = diabetes.data
    y = diabetes.target

    # Dividir los datos en conjuntos de entrenamiento y prueba
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42) #20% para test

    # Crear un modelo de regresión lineal
    modelo = LinearRegression()

    # Entrenar el modelo
    modelo.fit(X_train, y_train)

    # Hacer predicciones en el conjunto de prueba
    y_pred = modelo.predict(X_test)

    # Evaluar el modelo
    rmse = np.sqrt(mean_squared_error(y_test, y_pred))
    print(f"RMSE: {rmse}")
    ```

## 📝 **Expresiones Regulares (`re`)**

```python
import re
# Buscar un patrón
texto = "Mi número de teléfono es 123-456-7890."
patron = r"\d{3}-\d{3}-\d{4}"  # \d: dígito, {3}: exactamente 3 veces
resultado = re.search(patron, texto)  # Devuelve un objeto Match si lo encuentra, None si no.
if resultado:
  print(resultado.group(0))  # 123-456-7890
  print(resultado.start()) # Posición donde empieza
  print(resultado.end()) #Posición donde acaba
# Buscar todas las coincidencias
texto = "Tengo dos números: 123 y 456."
patron = r"\d+"  # +: una o más veces
resultados = re.findall(patron, texto)  # Devuelve una lista de todas las coincidencias
print(resultados)  # ['123', '456']
# Sustituir
texto = "Mi correo es usuario@dominio.com"
nuevo_texto = re.sub(r"[\w\.-]+@[\w\.-]+", "[correo redactado]", texto)  # Sustituye la dirección de correo
print(nuevo_texto)
# Dividir una cadena
texto = "uno,dos,tres;cuatro"
partes = re.split(r"[,;]", texto)  # Divide por comas o puntos y comas
print(partes)  # ['uno', 'dos', 'tres', 'cuatro']
# Compilar un patrón (para reutilizarlo y mejorar el rendimiento)
patron_compilado = re.compile(r"\d+")
resultado1 = patron_compilado.search("Número 123")
resultado2 = patron_compilado.findall("Números 456 y 789")
# Banderas (flags)
texto = "Hola Mundo"
resultado = re.search(r"mundo", texto, re.IGNORECASE)  # Ignora mayúsculas/minúsculas
print(resultado.group())
```

## 📦 **Gestión de Paquetes y Entornos Virtuales**

*   **`pip`:** El gestor de paquetes de Python.
    *   `pip install nombre_paquete`: Instala un paquete.
    *   `pip uninstall nombre_paquete`: Desinstala un paquete.
    *   `pip list`: Lista los paquetes instalados.
    *   `pip freeze > requirements.txt`: Guarda las versiones de los paquetes instalados en un archivo `requirements.txt` (para reproducibilidad).
    *   `pip install -r requirements.txt`: Instala los paquetes desde un archivo `requirements.txt`.
    * `pip show <package>`: Muestra información del paquete
    * `pip search <package>`: Busca paquetes

*   **Entornos Virtuales (`venv`, `virtualenv`)**:
    *   Aíslan las dependencias de tus proyectos.  Evitan conflictos entre versiones de paquetes.
    *   `python3 -m venv nombre_entorno` (crea un entorno virtual con `venv`)
    *   `source nombre_entorno/bin/activate` (activa el entorno virtual en Linux/macOS)
    *   `nombre_entorno\Scripts\activate` (activa el entorno virtual en Windows)
    * `deactivate`: Desactiva el entorno
    *   `virtualenv nombre_entorno` (crea un entorno virtual con `virtualenv`, más antiguo pero aún muy usado)

* **`conda`**:  Gestor de paquetes *y* entornos virtuales, muy popular en ciencia de datos.  No es parte de la biblioteca estándar de Python (viene con Anaconda o Miniconda).
  * `conda create --name myenv`: Crea un entorno llamado 'myenv'
  * `conda activate myenv`: Activa
  * `conda deactivate`: Desactiva
  * `conda install <package>`
  * `conda list`: Lista los paquetes del entorno
  * `conda env export > environment.yml`: Exporta a un yml
  * `conda env create -f environment.yml`: Crea un entorno desde un yml
