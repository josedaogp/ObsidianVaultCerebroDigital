# 🧠 Cheatsheet de Keras

Este cheatsheet cubre los aspectos esenciales de Keras, la API de alto nivel de TensorFlow para construir y entrenar modelos de aprendizaje profundo.

## 🔍 Conceptos Básicos

*   **Modelo (Model):** 🧠 Contenedor que define la arquitectura de la red neuronal (capas, conexiones, etc.).
*   **Capa (Layer):** 🧱 Bloque de construcción fundamental de un modelo.  Realiza una transformación (lineal o no lineal) sobre los datos de entrada.
*   **Función de Activación (Activation Function):** ⚡️ Función no lineal aplicada a la salida de una capa (ReLU, sigmoid, tanh, etc.).  Introduce no linealidad en el modelo.
*   **Optimizador (Optimizer):** 🚀 Algoritmo que ajusta los pesos del modelo durante el entrenamiento (Adam, SGD, RMSprop, etc.).
*   **Función de Pérdida (Loss Function):** 📉 Mide la diferencia entre las predicciones del modelo y los valores reales (categorical_crossentropy, mse, etc.).
*   **Métrica (Metric):** 📊 Mide el rendimiento del modelo (accuracy, precision, recall, etc.).
*   **Entrenamiento (Training):** 💪 Proceso iterativo de ajustar los pesos del modelo para minimizar la función de pérdida.
*   **Época (Epoch):** 🔄 Un ciclo completo de entrenamiento sobre todos los datos de entrenamiento.
*   **Lote (Batch):** 📦 Subconjunto de datos de entrenamiento utilizado en una iteración del optimizador.
*   **Validación (Validation):** 🧪 Evaluación del modelo en un conjunto de datos separado (no utilizado para el entrenamiento) para monitorizar el sobreajuste (overfitting).
*   **Sobreajuste (Overfitting):** 📉 Cuando el modelo se ajusta demasiado a los datos de entrenamiento y no generaliza bien a datos nuevos.
*   **Regularización (Regularization):** 🛡️ Técnicas para prevenir el sobreajuste (dropout, L1/L2 regularization, etc.).
*   **Callback:** 📞 Función que se ejecuta en diferentes puntos durante el entrenamiento (por ejemplo, para guardar el modelo, detener el entrenamiento anticipadamente, etc.).
* **Transfer Learning**: ♻️ Aprovechar un modelo pre-entrenado en un dataset grande.

## 🚀 Construcción de Modelos

Keras ofrece tres formas principales de construir modelos:

1.  **Modelo Secuencial (Sequential):**

    ```python
    from tensorflow import keras
    from tensorflow.keras import layers

    model = keras.Sequential([
        layers.Dense(64, activation='relu', input_shape=(784,)),  # Capa de entrada
        layers.Dense(64, activation='relu'),
        layers.Dense(10, activation='softmax')  # Capa de salida (10 clases)
    ])
    ```
    *   La forma más sencilla.  Ideal para modelos con una secuencia lineal de capas.

2.  **API Funcional (Functional API):**

    ```python
    from tensorflow import keras
    from tensorflow.keras import layers, Model

    inputs = keras.Input(shape=(784,))
    x = layers.Dense(64, activation='relu')(inputs)
    x = layers.Dense(64, activation='relu')(x)
    outputs = layers.Dense(10, activation='softmax')(x)

    model = Model(inputs=inputs, outputs=outputs)
    ```
    *   Más flexible.  Permite crear modelos con múltiples entradas/salidas, capas compartidas, conexiones no lineales, etc.

3.  **Subclasificación de Modelos (Model Subclassing):**

    ```python
    from tensorflow import keras
    from tensorflow.keras import layers, Model

    class MiModelo(Model):
        def __init__(self):
            super(MiModelo, self).__init__()
            self.dense1 = layers.Dense(64, activation='relu')
            self.dense2 = layers.Dense(64, activation='relu')
            self.dense3 = layers.Dense(10, activation='softmax')

        def call(self, inputs):
            x = self.dense1(inputs)
            x = self.dense2(x)
            return self.dense3(x)

    model = MiModelo()
    ```
    *   Máxima flexibilidad.  Permite definir la lógica de la propagación hacia adelante (forward pass) de forma personalizada.  Útil para modelos con flujos de control complejos.  *Pero* es más difícil de usar, depurar y guardar/cargar.

## 🧱 Capas (Layers)

*   **`Dense`:** Capa densa (totalmente conectada).  `units`: número de neuronas.  `activation`: función de activación.
*   **`Conv2D`:** Capa convolucional 2D (para imágenes).  `filters`: número de filtros.  `kernel_size`: tamaño del filtro.  `strides`: paso de la convolución.  `padding`: 'valid' o 'same'.
*   **`MaxPooling2D`:** Capa de max pooling 2D.  `pool_size`: tamaño de la ventana de pooling.  `strides`: paso del pooling.
*   **`Dropout`:** Capa de dropout (regularización).  `rate`: fracción de neuronas a desactivar.
*   **`Flatten`:** Aplana la entrada (convierte un tensor multidimensional en un vector).
*   **`Embedding`:** Capa de embedding (para texto/secuencias).  `input_dim`: tamaño del vocabulario.  `output_dim`: dimensión del embedding.
*   **`LSTM`:** Capa LSTM (red recurrente).  `units`: número de unidades LSTM.
*   **`GRU`:** Capa GRU (otra red recurrente).
*   **`BatchNormalization`:** Normalización por lotes.
*   **`Activation`:** Aplica una función de activación.
*   **`Reshape`**: Cambia la forma de la entrada.
*   **`Input`**: Capa de entrada (se usa en la API funcional).

## ⚡️ Funciones de Activación (Activation Functions)

*   **`relu`:** Rectified Linear Unit (la más común).
*   **`sigmoid`:** Sigmoide (para clasificación binaria).
*   **`softmax`:** Softmax (para clasificación multiclase).
*   **`tanh`:** Tangente hiperbólica.
*   **`elu`:** Exponential Linear Unit.
*   **`selu`:** Scaled Exponential Linear Unit.
*   **`linear`:** Identidad (sin activación).

## 🚀 Optimizadores (Optimizers)

*   **`Adam`:** Adam (generalmente la mejor opción por defecto).
*   **`SGD`:** Descenso del gradiente estocástico (con o sin momentum).
*   **`RMSprop`:** RMSprop.
*   **`Adagrad`:** Adagrad.
*   **`AdamW`**: Adam con weight decay.

```python
# Ejemplo de uso
optimizer = keras.optimizers.Adam(learning_rate=0.001)
```

## 📉 Funciones de Pérdida (Loss Functions)

*   **`categorical_crossentropy`:** Para clasificación multiclase (etiquetas one-hot).
*   **`sparse_categorical_crossentropy`:** Para clasificación multiclase (etiquetas enteras).
*   **`binary_crossentropy`:** Para clasificación binaria.
*   **`mse`:** Mean Squared Error (para regresión).
*   **`mae`:** Mean Absolute Error (para regresión).
*   **`huber`:** Pérdida de Huber (robusta a outliers).

## 📊 Métricas (Metrics)

*   **`accuracy`:** Precisión (para clasificación).
*   **`categorical_accuracy`:** Precisión categórica.
*   **`sparse_categorical_accuracy`:** Precisión categórica (etiquetas enteras).
*   **`binary_accuracy`:** Precisión binaria.
*   **`precision`:** Precisión.
*   **`recall`:** Recall.
*   **`AUC`:** Área bajo la curva ROC.
*    `F1Score`: Combinación de precision y recall

## ⚙️ Compilación del Modelo

```python
model.compile(optimizer='adam',
              loss='categorical_crossentropy',
              metrics=['accuracy'])
```

*   `optimizer`: El optimizador a utilizar.
*   `loss`: La función de pérdida.
*   `metrics`: Lista de métricas para evaluar el modelo.

## 💪 Entrenamiento del Modelo

```python
history = model.fit(X_train, y_train,
                    batch_size=32,
                    epochs=10,
                    validation_data=(X_val, y_val),
                    callbacks=...) # Opcional: Lista de Callbacks
```

*   `X_train`, `y_train`: Datos de entrenamiento.
*   `batch_size`: Tamaño del lote.
*   `epochs`: Número de épocas.
*   `validation_data`: Datos de validación.
*   `callbacks`: Lista de callbacks.

## 🧪 Evaluación del Modelo

```python
loss, accuracy = model.evaluate(X_test, y_test)
```

*   `X_test`, `y_test`: Datos de prueba.

## 🔮 Predicción

```python
predictions = model.predict(X_new)
```

*   `X_new`: Nuevos datos para hacer predicciones.

## 📞 Callbacks

*   **`ModelCheckpoint`:** Guarda el modelo periódicamente.
*   **`EarlyStopping`:** Detiene el entrenamiento anticipadamente si la métrica de validación deja de mejorar.
*   **`TensorBoard`:** Para visualización con TensorBoard.
*   **`ReduceLROnPlateau`:** Reduce el learning rate si la métrica de validación se estanca.
*   **`LearningRateScheduler`:** Permite programar el learning rate.
*  `CSVLogger`: Guarda los logs en un fichero CSV.
* `LambdaCallback`: Permite crear Callbacks customizados

```python
callbacks = [
    keras.callbacks.ModelCheckpoint(filepath='mi_modelo_{epoch}.h5', save_best_only=True, monitor='val_loss'),
    keras.callbacks.EarlyStopping(monitor='val_loss', patience=3),
    keras.callbacks.TensorBoard(log_dir='./logs')
]
```

## 💾 Guardar y Cargar Modelos

```python
# Guardar el modelo completo (arquitectura + pesos + estado del optimizador)
model.save('mi_modelo.h5')  # Formato HDF5
model.save('mi_modelo')     # Formato SavedModel (recomendado)

# Cargar el modelo
loaded_model = keras.models.load_model('mi_modelo.h5')  # O 'mi_modelo'
```

## ♻️ Transfer Learning (Aprovechar Modelos Pre-entrenados)

```python
from tensorflow.keras.applications import VGG16
from tensorflow.keras import layers, Model

# Cargar un modelo pre-entrenado (sin la capa de clasificación)
base_model = VGG16(weights='imagenet', include_top=False, input_shape=(224, 224, 3))

# Congelar las capas del modelo base (para no entrenarlas)
base_model.trainable = False

# Añadir capas personalizadas encima
inputs = keras.Input(shape=(224, 224, 3))
x = base_model(inputs, training=False)  # training=False es importante
x = layers.GlobalAveragePooling2D()(x)
x = layers.Dense(10, activation='softmax')(x)  # Capa de salida para 10 clases
model = Model(inputs, x)
model.compile(...)
#Ahora se puede entrenar, pero solo se entrenarán las últimas capas.
```

## ✨ Consejos Adicionales

*   Comienza con modelos simples y aumenta gradualmente la complejidad.
*   Usa la API funcional para mayor flexibilidad.
*   Monitoriza el entrenamiento con TensorBoard.
*   Utiliza callbacks para guardar el modelo y detener el entrenamiento anticipadamente.
*   Experimenta con diferentes arquitecturas, optimizadores, funciones de pérdida y métricas.
*   Considera el transfer learning para acelerar el entrenamiento y mejorar el rendimiento.
*   Consulta la documentación oficial de Keras: [https://keras.io/](https://keras.io/)

Este cheatsheet de Keras proporciona una visión completa de los componentes y técnicas clave, junto con ejemplos prácticos. ¡Espero que te sea muy útil!