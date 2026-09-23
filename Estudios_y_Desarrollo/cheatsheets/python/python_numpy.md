# 🚀 Cheatsheet de NumPy en Python

Este cheatsheet cubre los comandos y técnicas más comunes de NumPy, con explicaciones, ejemplos y trucos.

## 🔍 Conceptos Básicos

*   **ndarray (N-dimensional array):** 🧱 Estructura de datos principal de NumPy.  Un array multidimensional, homogéneo (todos los elementos del mismo tipo), de tamaño fijo.
*   **dtype:** 🧮 Tipo de datos de los elementos del array (int, float, bool, etc.).
*   **shape:** 📏 Tupla que indica las dimensiones del array (número de filas, columnas, etc.).
*   **axis:** 🧭 Eje o dimensión del array.  En un array 2D, axis=0 es el eje de las filas, y axis=1 es el eje de las columnas.
*   **Broadcasting:** 📡 Mecanismo que permite a NumPy realizar operaciones entre arrays de diferentes formas, bajo ciertas condiciones.

## 📦 Creación de Arrays

1.  **Desde listas o tuplas:**

    ```python
    import numpy as np

    arr = np.array([1, 2, 3])  # Array 1D  # 1️⃣
    arr = np.array([[1, 2], [3, 4]])  # Array 2D  # 2️⃣
    arr = np.array([1, 2, 3], dtype=float)  # Especificando el tipo de datos
    ```

2.  **Arrays especiales:**

    ```python
    np.zeros(5)  # Array de ceros  # 0️⃣
    np.zeros((2, 3))  # Array de ceros 2x3
    np.ones(4)  # Array de unos  # 1️⃣
    np.ones((3, 2), dtype=int) # Array de unos enteros, de tamaño 3x2
    np.empty((2, 2)) # Array vacío (valores sin inicializar, rápido)
    np.eye(3)  # Matriz identidad 3x3  # 👁️
    np.full((2, 3), 7)  # Array 2x3 relleno con 7
    np.arange(10)  # Similar a range(), pero crea un array  # ➡️
    np.arange(2, 10, 2)  # De 2 a 10 (sin incluir), de 2 en 2
    np.linspace(0, 1, 5)  # 5 números espaciados uniformemente entre 0 y 1  # ➖
    ```

3.  **Números aleatorios:**

    ```python
    np.random.rand(3)  # Números aleatorios uniformes [0, 1)  # 🎲
    np.random.rand(2, 3)  # Array 2x3 de números aleatorios uniformes
    np.random.randn(3)  # Números aleatorios normales (media 0, desviación estándar 1)
    np.random.randint(1, 10, 5)  # 5 enteros aleatorios entre 1 y 10 (sin incluir)  # 🔢
    np.random.randint(1,10, size=(2,4)) # Enteros aleatorios con un shape dado
    np.random.choice([1, 2, 3], 2)  # Elige 2 elementos aleatorios de la lista
    np.random.seed(42) # Semilla para reproducibilidad
    ```

## 👀 Inspección y Propiedades

```python
arr = np.array([[1, 2, 3], [4, 5, 6]])

arr.shape  # (2, 3) - Tupla con las dimensiones  # 📏
arr.ndim  # 2 - Número de dimensiones  # 📐
arr.size  # 6 - Número total de elementos  # 🔢
arr.dtype  # dtype('int64') - Tipo de datos  # 🧮
arr.itemsize # 8 - Tamaño en bytes de cada elemento
arr.nbytes # 48 - Tamaño total en bytes
```

## 🎯 Selección y Slicing

1.  **Indexación (unidimensional):**

    ```python
    arr = np.array([10, 20, 30, 40, 50])
    arr[0]  # Primer elemento  # 1️⃣
    arr[-1]  # Último elemento  # ⬅️
    arr[2:4]  # Elementos desde el índice 2 hasta el 4 (sin incluir)  # ➡️
    arr[:3]  # Desde el inicio hasta el índice 3 (sin incluir)
    arr[2:]  # Desde el índice 2 hasta el final
    arr[:]  # Todos los elementos (copia del array)
    ```

2.  **Indexación (multidimensional):**

    ```python
    arr = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
    arr[0, 0]  # Elemento en la fila 0, columna 0  # 0️⃣
    arr[1, 2]  # Fila 1, columna 2
    arr[0]  # Primera fila  # ➡️
    arr[:, 1]  # Segunda columna
    arr[:2, 1:]  # Subarray: primeras 2 filas, últimas 2 columnas
    arr[0,:] #Primera fila
    ```

3.  **Indexación booleana:**

    ```python
    arr = np.array([1, 2, 3, 4, 5])
    bool_index = arr > 2  # [False, False, True, True, True]
    arr[bool_index]  # [3, 4, 5] - Selecciona elementos donde bool_index es True
    arr[arr > 2]  # Equivalente (más conciso)
    ```

4. **Fancy Indexing:**

```python
arr = np.array([10, 20, 30, 40, 50])
indices = np.array([0, 2, 4])
arr[indices]  # [10, 30, 50] - Selecciona elementos con los índices dados
```

## 🔄 Manipulación de Forma

1.  **Reshape:**

    ```python
    arr = np.arange(12)  # [ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11]
    arr.reshape(3, 4)  # Cambia la forma a 3x4  # 🔄
    arr.reshape(2, 2, 3)  # A 3 dimensiones
    arr.reshape(4, -1)  # -1 calcula automáticamente la dimensión
    # reshape no modifica el array original, devuelve uno nuevo
    ```

2.  **Ravel y Flatten:**

    ```python
    arr = np.array([[1, 2], [3, 4]])
    arr.ravel()  # Aplanar a 1D (vista, si es posible)  # 1️⃣
    arr.flatten()  # Aplanar a 1D (siempre una copia)
     # Ambos devuelven una copia, no modifican el original.
    ```

3.  **Transpose:**

    ```python
    arr = np.array([[1, 2], [3, 4]])
    arr.T  # Transpuesta (intercambia filas y columnas)  # ∷
    arr.transpose() # Equivalente a .T
    # No modifica el original
    ```

4.  **Concatenar y dividir:**

    ```python
    a = np.array([[1, 2], [3, 4]])
    b = np.array([[5, 6]])
    np.concatenate((a, b), axis=0)  # Concatenar a lo largo del eje 0 (filas)  # ➕
    np.concatenate((a,b.T), axis=1) # Concatenar a lo largo del eje 1 (columnas)
    np.vstack((a, b))  # Apilar verticalmente (equivalente a concatenate axis=0)
    np.hstack((a, b.T))  # Apilar horizontalmente (equivalente a concatenate axis=1)

    arr = np.arange(9).reshape(3, 3)
    np.split(arr, 3)  # Dividir en 3 partes iguales  # ➗
    np.vsplit(arr, 3) # Split vertical
    np.hsplit(arr, 3) # Split horizontal
    ```

5. **Resize**
```python
arr = np.array([1,2,3,4])
np.resize(arr, (2,4)) #Modifica el array original
```

## ➕ Operaciones Matemáticas y Estadísticas

NumPy realiza operaciones elemento a elemento (element-wise) y aprovecha el broadcasting.

1.  **Operaciones aritméticas:**

    ```python
    a = np.array([1, 2, 3])
    b = np.array([4, 5, 6])
    a + b  # Suma  # ➕
    a - b  # Resta  # ➖
    a * b  # Multiplicación  # ✖️
    a / b  # División  # ➗
    a ** 2  # Potencia
    a + 10 # Suma 10 a cada elemento (broadcasting)
    ```

2.  **Funciones universales (ufuncs):**

    ```python
    arr = np.array([1, 4, 9])
    np.sqrt(arr)  # Raíz cuadrada  # √
    np.exp(arr)  # Exponencial
    np.sin(arr)  # Seno
    np.cos(arr) # Coseno
    np.log(arr)  # Logaritmo natural
    np.abs(arr) # Valor absoluto
    ```

3.  **Estadísticas:**

    ```python
    arr = np.array([[1, 2, 3], [4, 5, 6]])
    arr.sum()  # Suma de todos los elementos  # Σ
    arr.sum(axis=0)  # Suma por columnas
    arr.sum(axis=1)  # Suma por filas
    arr.mean()  # Media  # μ
    arr.mean(axis=0) # Media por columnas
    arr.std()  # Desviación estándar  # σ
    arr.var()  # Varianza
    arr.min()  # Mínimo
    arr.max()  # Máximo
    arr.argmin()  # Índice del mínimo
    arr.argmax()  # Índice del máximo
    np.median(arr)  # Mediana
    ```

4.  **Álgebra lineal (np.linalg):**

    ```python
    a = np.array([[1, 2], [3, 4]])
    b = np.array([[5, 6], [7, 8]])
    np.dot(a, b)  # Producto matricial  # ·
    a @ b # Equivalente a np.dot(a,b)
    np.linalg.inv(a)  # Inversa de una matriz  # ⁻¹
    np.linalg.det(a)  # Determinante
    np.linalg.eig(a)  # Autovalores y autovectores
    np.linalg.solve(a, b) # Resolver un sistema de ecuaciones lineales
    ```

## ✨ Trucos y Consejos

*   **Vectorización:**  Evita bucles explícitos en Python siempre que sea posible.  Usa las operaciones vectorizadas de NumPy, que son mucho más rápidas.

*   **Broadcasting:**  Entiende cómo funciona el broadcasting para realizar operaciones entre arrays de diferentes formas de manera eficiente.  Las dimensiones deben ser compatibles (iguales o una de ellas ser 1).

*   **Vistas vs. Copias:**  Algunas operaciones (como slicing) devuelven *vistas* del array original (no copian los datos).  Modificar la vista modifica el array original.  Usa `.copy()` para crear una copia explícita si es necesario.

*   **Usar `np.where`:**

    ```python
    arr = np.array([1, 2, 3, 4, 5])
    np.where(arr > 2, 10, 0)  # [ 0,  0, 10, 10, 10] - Reemplaza según la condición
    # (condición, valor si True, valor si False)
    np.where(arr>2) # Devuelve los índices donde se cumple la condición
    ```

*   **Usar `np.nan` para representar valores faltantes:**  NumPy tiene funciones para manejar `np.nan` de forma eficiente ( `np.nansum`, `np.nanmean`, etc.).

*  **Funciones útiles**
    ```python
    arr = np.array([2,1,5,3,2])
    np.unique(arr) # Devuelve un array con los valores únicos
    np.sort(arr) # Ordena de menor a mayor
    arr.argsort() # Devuelve los índices que ordenarían el array.
    np.tile(arr, 2) # Repite el array
    np.repeat(arr, 2) # Repite los elementos del array.
    ```

Este cheatsheet está diseñado para ser una referencia rápida y completa para el trabajo diario con NumPy. ¡Guárdalo y consúltalo a menudo!