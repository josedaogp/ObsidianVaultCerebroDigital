# 🧠 Cheatsheet de TensorFlow

Este cheatsheet cubre los fundamentos de TensorFlow, incluyendo Keras, `tf.data`, y ejemplos prácticos. ¡Usa los íconos para navegar rápidamente!

## 🔍 Conceptos Básicos

*   **Tensor:** 🧮 Arreglo multidimensional (similar a un array de NumPy). Es la unidad fundamental de datos en TensorFlow.
*   **Grafo (Graph):** 📈 Representación de un cálculo como un grafo dirigido. TensorFlow 1.x usaba grafos estáticos, mientras que TensorFlow 2.x usa *eager execution* por defecto (grafos dinámicos), lo que hace el desarrollo más intuitivo (similar a PyTorch).
*   **Variable:** 📦 Tensor especial que contiene valores que pueden modificarse durante el entrenamiento (los pesos y sesgos de un modelo).
*   **Operación (Operation/Op):** ➕➖✖️➗ Nodo en el grafo que realiza un cálculo sobre tensores.
*   **Sesión (Session):** (Obsoleto en TF 2.x)  Entorno donde se ejecutaba el grafo en TensorFlow 1.x. En TF 2.x, las operaciones se ejecutan *eagerly* (inmediatamente).
*   **Eager Execution:** ▶️ Modo de ejecución por defecto en TF 2.x. Las operaciones se ejecutan inmediatamente, sin necesidad de construir un grafo estático.
*   **`tf.function`:** 🚀 Decorador que convierte una función de Python en un grafo de TensorFlow, optimizando su ejecución.
*   **Gradiente (Gradient):** ∇ Vector que indica la dirección y magnitud del cambio de una función.  Fundamental para el entrenamiento de modelos (descenso del gradiente).
*   **Keras:** 👑 API de alto nivel para construir y entrenar modelos de aprendizaje profundo de forma sencilla.  Está integrada en TensorFlow.
*   **`tf.data`:** 💾 API para construir pipelines de entrada de datos eficientes y escalables.
*   **TensorBoard:** 📊 Herramienta de visualización para monitorizar el entrenamiento de modelos.
*   **Módulos Guardados (SavedModel):** 💾 Formato para guardar y cargar modelos de TensorFlow (pesos, grafo, metadatos).
*   **TPU (Tensor Processing Unit):** ⚡️ Acelerador de hardware de Google diseñado para cargas de trabajo de aprendizaje profundo.
*  **`tf.distribute.Strategy`**: API para entrenar modelos en multiples GPUs, TPUs, o máquinas.

## 🚀 TensorFlow Eager Execution (TF 2.x)

TensorFlow 2.x usa *eager execution* por defecto.  Esto significa que las operaciones se ejecutan inmediatamente, como en Python puro o NumPy.

```python
import tensorflow as tf

# Crear tensores
a = tf.constant([[1.0, 2.0],
                 [3.0, 4.0]])
b = tf.constant([[5.0, 6.0],
                 [7.0, 8.0]])

# Operaciones
c = a + b  # Suma
d = tf.matmul(a, b)  # Multiplicación de matrices

print(c)
print(d)

# Variables (para parámetros del modelo)
w = tf.Variable([[1.0], [2.0]])
print(w)
w.assign([[3.0], [4.0]]) # Modificar el valor
print(w)

# Funciones y grafos con @tf.function
@tf.function
def mi_funcion(x, y):
  z = tf.matmul(x, y)
  return z

x = tf.constant([[1.0, 2.0]])
y = tf.constant([[3.0], [4.0]])
resultado = mi_funcion(x, y) # La primera llamada construye el grafo
print(resultado)
```

## 👑 Keras API

Keras es la forma recomendada de construir modelos en TensorFlow.  Ofrece una API intuitiva y modular.

### Modelos Secuenciales (Sequential)

```python
from tensorflow import keras
from tensorflow.keras import layers

# Crear un modelo secuencial
model = keras.Sequential([
    layers.Dense(128, activation='relu', input_shape=(784,)),  # Capa densa (fully connected)
    layers.Dropout(0.2),  # Dropout para regularización
    layers.Dense(10, activation='softmax')  # Capa de salida (10 clases, softmax para clasificación)
])

# Compilar el modelo
model.compile(optimizer='adam',
              loss='categorical_crossentropy',
              metrics=['accuracy'])

# Resumen del modelo
model.summary()

# Entrenar el modelo (requiere datos X_train, y_train)
# model.fit(X_train, y_train, epochs=10, batch_size=32)

# Evaluar el modelo (requiere datos X_test, y_test)
# loss, accuracy = model.evaluate(X_test, y_test)

# Hacer predicciones
# predictions = model.predict(X_new)
```

### API Funcional (Functional API)

Más flexible que el modelo secuencial.  Permite construir modelos con múltiples entradas/salidas, capas compartidas, etc.

```python
from tensorflow import keras
from tensorflow.keras import layers, Model

# Definir las capas
inputs = keras.Input(shape=(784,))
x = layers.Dense(128, activation='relu')(inputs)
x = layers.Dropout(0.2)(x)
outputs = layers.Dense(10, activation='softmax')(x)

# Crear el modelo
model = Model(inputs=inputs, outputs=outputs)

# El resto es igual que en el modelo secuencial (compile, summary, fit, evaluate, predict)
model.compile(...)
model.summary()
# ...
```
### Capas (Layers) Comunes

*   `Dense`: Capa densa (fully connected).
*   `Conv2D`: Capa convolucional 2D (para imágenes).
*   `MaxPooling2D`: Capa de max pooling 2D.
*   `Dropout`: Capa de dropout (regularización).
*   `Flatten`: Aplana la entrada (útil después de capas convolucionales).
*   `Embedding`: Capa de embedding (para texto).
*   `LSTM`: Capa LSTM (red recurrente).
*   `GRU`:  Capa GRU (otra red recurrente).
*   `BatchNormalization`: Normalización por lotes.
* `ReLU`, `LeakyReLU`, `Sigmoid`, `Tanh`: Capas de activaciones

### Optimizadores (Optimizers)

*   `Adam`:  Algoritmo de optimización popular (generalmente una buena opción por defecto).
*   `SGD`:  Descenso del gradiente estocástico (con o sin momentum).
*   `RMSprop`: Otro algoritmo de optimización.
* `Adagrad`: Optimizador con learning rate adaptativo

### Funciones de Pérdida (Loss Functions)

*   `categorical_crossentropy`:  Para clasificación multiclase (cuando las etiquetas están en formato one-hot).
*   `sparse_categorical_crossentropy`:  Para clasificación multiclase (cuando las etiquetas son enteros).
*   `binary_crossentropy`:  Para clasificación binaria.
*   `mse` (mean squared error):  Para regresión.
*  `mae` (mean absolute error) : Para regresión
*  `hinge`: Para máquinas de soporte vectorial.

### Métricas (Metrics)

*   `accuracy`: Precisión (para clasificación).
*   `categorical_accuracy`:  Precisión categórica.
*   `sparse_categorical_accuracy`: Precisión categórica (para etiquetas enteras).
*   `binary_accuracy`: Precisión binaria.
*   `mse`, `mae`:  Para regresión.
*  `AUC`: Area bajo la curva ROC
*  `Precision`, `Recall`: Para problemas de clasificación

### Callbacks
*   `ModelCheckpoint`: Guarda el modelo periódicamente.
*   `EarlyStopping`: Detiene el entrenamiento si la métrica de validación deja de mejorar.
*   `TensorBoard`:  Para visualización con TensorBoard.
*   `ReduceLROnPlateau`: Reduce el learning rate si la métrica de validación se estanca.
*   `LearningRateScheduler`:  Programa el learning rate.
*   `LambdaCallback`:  Permite definir callbacks personalizados.

```python
# Ejemplo de callbacks
callbacks = [
    keras.callbacks.ModelCheckpoint(filepath='mi_modelo_{epoch}.h5', save_best_only=True, monitor='val_loss'),
    keras.callbacks.EarlyStopping(monitor='val_loss', patience=3),  # Detener si val_loss no mejora en 3 epochs
    keras.callbacks.TensorBoard(log_dir='./logs')
]

# model.fit(..., callbacks=callbacks)
```

## 💾 `tf.data` API

Para construir pipelines de entrada de datos eficientes.

```python
import tensorflow as tf

# Crear un dataset a partir de una lista
dataset = tf.data.Dataset.from_tensor_slices([1, 2, 3, 4, 5, 6])

# Crear un dataset a partir de tensores
features = tf.constant([[1, 2], [3, 4], [5, 6]])
labels = tf.constant([0, 1, 0])
dataset = tf.data.Dataset.from_tensor_slices((features, labels))

# Operaciones comunes
dataset = dataset.shuffle(buffer_size=1000)  # Mezclar los datos
dataset = dataset.batch(32)  # Agrupar en lotes de 32
dataset = dataset.repeat()  # Repetir el dataset (útil para entrenamiento)
dataset = dataset.prefetch(tf.data.AUTOTUNE)  # Precargar datos para mejorar el rendimiento

# Iterar sobre el dataset
for element in dataset:
    print(element)
    break #solo se muestra el primer elemento por brevedad.

# Leer datos desde archivos CSV
dataset = tf.data.experimental.make_csv_dataset(
    file_pattern="*.csv",
    batch_size=32,
    column_names=["col1", "col2", "col3"],
    label_name="col3",
    num_epochs=1 #Si no se pone, itera indefinidamente.
)

# Leer imágenes
def parse_image(filename):
    image_string = tf.io.read_file(filename)
    image = tf.image.decode_jpeg(image_string, channels=3) #o decode_png
    image = tf.image.convert_image_dtype(image, tf.float32)  # Normalizar a [0, 1]
    image = tf.image.resize(image, [224, 224])
    return image

image_filenames = tf.data.Dataset.list_files("*.jpg") #o .png, etc
image_dataset = image_filenames.map(parse_image)
```

## 📊 TensorBoard

```bash
# Inicia TensorBoard (desde la línea de comandos)
tensorboard --logdir ./logs  # Apunta al directorio donde se guardan los logs
```

## 🌐 `tf.distribute.Strategy` (Entrenamiento Distribuido)

```python
import tensorflow as tf

# Estrategia para múltiples GPUs (si están disponibles)
strategy = tf.distribute.MirroredStrategy()

with strategy.scope():
  # Todo el código que use la GPU (creación del modelo, compilación, entrenamiento)
  # debe ir dentro de este 'scope'
  model = keras.Sequential(...)
  model.compile(...)

# model.fit(...)  # El entrenamiento se distribuirá automáticamente
```

## 💾 Guardar y Cargar Modelos

```python
# Guardar el modelo completo (arquitectura + pesos + estado del optimizador)
model.save('mi_modelo.h5')  # Formato HDF5 (más antiguo)
model.save('mi_modelo')     # Formato SavedModel (recomendado)

# Cargar el modelo
loaded_model = keras.models.load_model('mi_modelo.h5')  # o 'mi_modelo'
```

## ✨ Consejos Adicionales

*   Usa `tf.function` para optimizar el rendimiento de tus funciones.
*   Aprovecha `tf.data` para construir pipelines de entrada eficientes.
*   Utiliza TensorBoard para monitorizar el entrenamiento.
*   Considera usar TPUs si necesitas entrenar modelos muy grandes.
*  Consulta la documentación oficial para detalles: [https://www.tensorflow.org/](https://www.tensorflow.org/)
*  Explora los tutoriales y ejemplos de la web de TensorFlow.