# 🐼 Cheatsheet de Pandas en Python

Este cheatsheet cubre los comandos y técnicas más comunes de Pandas, con explicaciones, ejemplos y trucos.

## 🔍 Conceptos Básicos

*   **DataFrame:** 📊 Estructura de datos tabular, bidimensional, con filas y columnas etiquetadas.  Es como una hoja de cálculo o una tabla de base de datos.
*   **Series:** 📈 Estructura de datos unidimensional, similar a un array o una lista, pero con etiquetas (índices).  Es como una columna de un DataFrame.
*   **Index:** 🏷️ Etiquetas de las filas (y columnas en un DataFrame).  Puede ser numérico, de texto, fechas, etc.
* **dtype**: El tipo de dato de una columna

## 🚀 Creación y Carga de Datos

1.  **Crear un DataFrame desde cero:**

    ```python
    import pandas as pd

    # Desde un diccionario de listas (cada lista es una columna)
    data = {'col1': [1, 2, 3], 'col2': ['a', 'b', 'c']}
    df = pd.DataFrame(data)

    # Desde una lista de listas (cada lista es una fila)
    data = [[1, 'a'], [2, 'b'], [3, 'c']]
    df = pd.DataFrame(data, columns=['col1', 'col2'])

    # Desde un array de NumPy
    import numpy as np
    data = np.array([[1, 2], [3, 4], [5, 6]])
    df = pd.DataFrame(data, columns=['A', 'B'])
    ```

2.  **Leer datos desde archivos:**

    ```python
    # CSV
    df = pd.read_csv('archivo.csv')  # 📁
    #Opciones importantes de read_csv:
    # sep: separador (por defecto ',')
    # header: fila del encabezado (por defecto 0, la primera)
    # names: lista de nombres de columnas
    # index_col: columna a usar como índice
    # skiprows: número de filas a saltar al inicio
    # nrows: número de filas a leer
    # na_values: valores a considerar como NaN
    # parse_dates: columnas a parsear como fechas
    # encoding: codificación del archivo ('utf-8', 'latin-1', etc.)

    # Excel
    df = pd.read_excel('archivo.xlsx', sheet_name='Hoja1')  # 📊
    #Opciones importantes de read_excel:
    # sheet_name: nombre o índice de la hoja
    #  (resto de opciones como read_csv)

    # JSON
    df = pd.read_json('archivo.json')  # 📝
    #Opciones importantes de read_json:
    # orient: formato del JSON ('split', 'records', 'index', 'columns', 'values')
    # typ: 'frame' o 'series'

    # SQL (requiere una conexión a la base de datos)
    import sqlite3
    conn = sqlite3.connect('mi_base_de_datos.db')
    query = "SELECT * FROM mi_tabla"
    df = pd.read_sql(query, conn) # 🗄️
    conn.close()

    #Desde el portapapeles
    df = pd.read_clipboard()

    #HTML
    df = pd.read_html('url')
    ```

## 👀 Exploración y Visualización

1.  **Ver las primeras/últimas filas:**

    ```python
    df.head()  # 5 primeras filas (por defecto)  # ⬆️
    df.head(10) # 10 primeras filas
    df.tail()  # 5 últimas filas  # ⬇️
    df.tail(3) # 3 últimas
    ```

2.  **Información básica del DataFrame:**

    ```python
    df.info()  # Resumen: tipos de datos, valores no nulos, memoria  # ℹ️
    df.describe()  # Estadísticas descriptivas (solo columnas numéricas) # 📈
    df.describe(include='all') # Incluye todas las columnas
    df.shape  # (número de filas, número de columnas)  # 📏
    df.columns  # Lista de nombres de columnas  # 📝
    df.index  # Rango o etiquetas del índice  # 🏷️
    df.dtypes # Tipos de datos
    ```

3.  **Valores únicos y conteo:**

    ```python
    df['columna'].unique()  # Array con valores únicos  # 🦄
    df['columna'].value_counts()  # Conteo de cada valor único  # 🔢
    df['columna'].nunique() # Número de valores únicos
    ```

4.  **Visualización rápida (requiere matplotlib):**

    ```python
    df['columna'].plot()  # Gráfico de línea  # 📉
    df.plot(x='col1', y='col2') # Gráfico de dispersión
    df['columna'].hist()  # Histograma  # 📊
    df.boxplot(column='columna')  # Diagrama de caja
    ```

## 🧹 Limpieza de Datos

1.  **Valores faltantes (NaN):**

    ```python
    df.isnull()  # DataFrame booleano: True donde hay NaN  # ❓
    df.isnull().sum()  # Número de NaN por columna
    df.dropna()  # Elimina filas con *cualquier* NaN  # 🗑️
    df.dropna(axis=1)  # Elimina columnas con *cualquier* NaN
    df.dropna(subset=['col1', 'col2'])  # Elimina filas si hay NaN en col1 o col2
    df.fillna(0)  # Reemplaza NaN por 0  # 0️⃣
    df['columna'].fillna(df['columna'].mean())  # Reemplaza NaN por la media
    ```

2.  **Duplicados:**

    ```python
    df.duplicated()  # Serie booleana: True donde hay filas duplicadas  # 👯
    df.duplicated().sum() # Numero de duplicados
    df.drop_duplicates()  # Elimina filas duplicadas  # 🗑️
     #Opciones importantes de drop_duplicates
     # subset: lista de columnas a considerar
     # keep: 'first', 'last', False (elimina todos)
    ```

3.  **Cambiar nombres de columnas:**

    ```python
    df.rename(columns={'col1': 'nueva_col1', 'col2': 'nueva_col2'})  # 📝
    # inplace=True para modificar el DataFrame original
    df.columns = ['col1', 'col2', 'col3'] # Cambiar todos los nombres a la vez.
    ```

4.  **Cambiar tipos de datos:**

    ```python
    df['columna'] = df['columna'].astype(int)  # A entero  # 1️⃣
    df['columna'] = df['columna'].astype(float)  # A float
    df['columna'] = df['columna'].astype(str)  # A string
    df['fecha'] = pd.to_datetime(df['fecha'])  # A datetime  # 📅
    ```

5.  **Eliminar columnas/filas:**

    ```python
    df.drop('columna', axis=1)  # Elimina columna  # 🗑️
    df.drop(0, axis=0)  # Elimina fila con índice 0
    df.drop([0, 1, 2], axis=0) # Eliminar por índice
    df.drop(columns=['col1', 'col2']) #Eliminar por nombre de columna
    #inplace=True  # Modifica el DataFrame original
    ```

6. **Reemplazar Valores**
    ```python
    df['columna'].replace('valor_viejo','valor_nuevo')
    df.replace({'col1':{'valor_viejo':'valor_nuevo'}})
    ```

## 🎯 Selección y Filtrado

1.  **Seleccionar columnas:**

    ```python
    df['columna']  # Devuelve una Serie  # 1️⃣
    df[['columna']]  # Devuelve un DataFrame (con una sola columna)  # 📊
    df[['col1', 'col2', 'col3']]  # Varias columnas
    ```

2.  **Seleccionar filas por índice (loc y iloc):**

    ```python
    df.loc[0]  # Fila con índice 0 (etiqueta)  # 🏷️
    df.loc[0:5]  # Filas con índices 0 a 5 (inclusivo)
    df.iloc[0]  # Primera fila (posición)  # 1️⃣
    df.iloc[0:5]  # Primeras 5 filas (0 a 4, exclusivo el 5)
    df.loc[0, 'col1'] # Valor de la fila 0 y la columna 'col1'
    df.iloc[0, 0] # Valor de la primera fila y la primera columna
    df.loc[df.index[0:10],['col1']] # Devuelve un dataframe con los valores de la columna 'col1' de las filas 0 a 9
    ```

3.  **Filtrado booleano:**

    ```python
    df[df['columna'] > 10]  # Filas donde 'columna' es mayor que 10  # >️⃣
    df[(df['col1'] > 5) & (df['col2'] < 20)]  # Condiciones múltiples (and: &, or: |)
    df[df['columna'].isin(['a', 'b', 'c'])]  # Valores en una lista  # 📝
    df.query('col1 > 5 and col2 < 20')  # Filtrado con una expresión (más legible)
    ```

## ✨ Transformación y Manipulación

1.  **Aplicar funciones (apply):**

    ```python
    df['columna'].apply(lambda x: x * 2)  # Aplica una función a cada elemento  # ⚙️
    df.apply(lambda row: row['col1'] + row['col2'], axis=1)  # A cada fila
    df.applymap(lambda x: x*2 if isinstance(x, (int,float)) else x) #Aplica a cada elemento del dataframe

    def mi_funcion(x):
        return x**2 + 1

    df['columna'].apply(mi_funcion)
    ```

2.  **Agrupación (groupby):**

    ```python
    df.groupby('columna')['otra_columna'].mean()  # Media de 'otra_columna' por cada valor de 'columna'  # ➕
    df.groupby('columna').agg({'otra_columna': 'sum', 'col3': 'mean'})  # Múltiples agregaciones
    df.groupby(['col1', 'col2']).size() # Agrupa por varias columnas
    #Otras funciones de agregación: sum, count, min, max, std, var, median, first, last
    ```

3.  **Tablas dinámicas (pivot_table):**

    ```python
    pd.pivot_table(df, values='valor', index='fila', columns='columna', aggfunc='sum')  # 🔄
    # values: columna a agregar
    # index: columna(s) a usar como índice
    # columns: columna(s) a usar como columnas
    # aggfunc: función de agregación (mean, sum, count, etc.)
    # fill_value: valor para reemplazar NaN
    # margins: True para agregar totales
    ```

4.  **Concatenación (concat):**

    ```python
    df1 = pd.DataFrame({'A': [1, 2], 'B': [3, 4]})
    df2 = pd.DataFrame({'A': [5, 6], 'B': [7, 8]})
    pd.concat([df1, df2])  # Apila DataFrames verticalmente  # ⬆️⬇️
    pd.concat([df1,df2], axis = 1) # Apila DataFrames horizontalmente
    ```

5.  **Fusión (merge):**

    ```python
    df1 = pd.DataFrame({'clave': ['a', 'b', 'c'], 'valor1': [1, 2, 3]})
    df2 = pd.DataFrame({'clave': ['b', 'c', 'd'], 'valor2': [4, 5, 6]})
    pd.merge(df1, df2, on='clave')  # Une DataFrames por una columna común  # 🤝
    # how: 'left', 'right', 'outer', 'inner' (por defecto)
    # left_on, right_on: nombres de columnas si son diferentes
    # suffixes: sufijos para columnas con el mismo nombre
    ```
6. **Unión (join)**
    ```python
    df1 = pd.DataFrame({'valor1': [1, 2, 3]}, index = ['a', 'b', 'c'])
    df2 = pd.DataFrame({'valor2': [4, 5, 6]}, index = ['b', 'c', 'd'])
    df1.join(df2) #Por defecto es un left join, utilizando los índices.
    # how: 'left', 'right', 'outer', 'inner' (por defecto)
    ```

7.  **Ordenación:**

    ```python
    df.sort_values('columna')  # Ordena por una columna (ascendente por defecto)  # ⬆️
    df.sort_values('columna', ascending=False)  # Descendente
    df.sort_index()  # Ordena por índice
    ```

8.  **Operaciones con strings (str):**

    ```python
    df['columna'].str.lower()  # A minúsculas  # 🔡
    df['columna'].str.upper()  # A mayúsculas
    df['columna'].str.contains('patron')  # True si contiene el patrón
    df['columna'].str.replace('viejo', 'nuevo')  # Reemplaza
    df['columna'].str.split(',')  # Divide por un delimitador
    # Y muchas más...
    ```

9.  **Operaciones con fechas (dt):**

    ```python
    df['fecha'] = pd.to_datetime(df['fecha'])
    df['fecha'].dt.year  # Año  # 📅
    df['fecha'].dt.month  # Mes
    df['fecha'].dt.day  # Día
    df['fecha'].dt.hour  # Hora
    df['fecha'].dt.dayofweek  # Día de la semana (0 = lunes, 6 = domingo)
    # Y muchas más...
    ```
10. **Iterar un DataFrame**

```python
# Iterar por filas
for index, row in df.iterrows():
    print(index, row['col1'], row['col2'])

#Iterar por columnas
for col_name, col_data in df.items():
    print(col_name)
```

## 💡 Trucos y Consejos

*   **Encadenamiento de métodos (method chaining):**  Encadena operaciones para un código más limpio y eficiente.

    ```python
    df = (pd.read_csv('archivo.csv')
            .dropna()
            .rename(columns={'old_name': 'new_name'})
            .assign(nueva_columna = lambda x: x['col1'] * 2)
            .query('nueva_columna > 10')
          )
    ```

*   **`inplace=True` con cuidado:**  Modifica el DataFrame original *in situ*.  Puede ser útil, pero también puede llevar a errores si no se usa con cuidado.  A menudo es mejor reasignar el DataFrame ( `df = df.dropna()` en lugar de `df.dropna(inplace=True)` ).

*   **Usar `.loc` y `.iloc` para evitar ambigüedades:**  Es más explícito y evita problemas con la "chained indexing" (indexación encadenada).

*   **Aprovechar las funciones vectorizadas de NumPy:**  Pandas está construido sobre NumPy, y muchas operaciones de NumPy se pueden aplicar directamente a Series y DataFrames.  Son mucho más rápidas que los bucles.

*   **`df.style` para visualizaciones mejoradas:** Permite aplicar estilos condicionales, barras de datos, mapas de calor, etc., directamente en la salida del DataFrame (útil en Jupyter Notebooks).

*   **Usar tipos de datos categóricos ( `astype('category')` ):**  Ahorra memoria y acelera las operaciones si tienes columnas con un número limitado de valores únicos.

*   **Guardar DataFrames en formatos eficientes (Parquet, Feather):**  Son más rápidos de leer y escribir que CSV, y ocupan menos espacio.

    ```python
    df.to_parquet('archivo.parquet')  # 📁
    df = pd.read_parquet('archivo.parquet')

    df.to_feather('archivo.feather')  # 🪶
    df = pd.read_feather('archivo.feather')
    ```

*   **`pd.set_option` para configurar opciones de visualización:**

    ```python
    pd.set_option('display.max_rows', 100)  # Mostrar hasta 100 filas
    pd.set_option('display.max_columns', 20)  # Mostrar hasta 20 columnas
    pd.set_option('display.precision', 2)  # Mostrar 2 decimales
    ```
* **Memory Usage**
    ```python
    df.memory_usage(deep=True) #Muestra la memoria usada por cada columna
    ```

Este cheatsheet está diseñado para ser una referencia rápida y completa para el trabajo diario con Pandas. ¡Guárdalo y consúltalo a menudo!