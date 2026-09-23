# ⚡ Cheatsheet de PyTorch

Este cheatsheet proporciona una referencia rápida a los conceptos básicos y operaciones esenciales de PyTorch, una biblioteca de aprendizaje automático de código abierto basada en Python.

## 🔍 Conceptos Fundamentales

*   **Tensor:** 🧱 La estructura de datos fundamental en PyTorch, similar a un array de NumPy, pero con la capacidad de ejecutarse en GPUs para acelerar los cálculos.
*   **Autograd:** ⚙️ El motor de diferenciación automática de PyTorch.  Calcula automáticamente los gradientes de las operaciones realizadas sobre los tensores, lo cual es esencial para el entrenamiento de redes neuronales.
*   **Módulo (Module):** 🧠 La unidad básica para construir redes neuronales.  Un módulo puede contener otros módulos, tensores y parámetros.
*   **Parámetro (Parameter):** 📊 Un tipo especial de tensor que se considera un parámetro del modelo (pesos, sesgos).  Los parámetros se actualizan durante el entrenamiento.
*   **Optimizador (Optimizer):** 📈 Algoritmo que ajusta los parámetros del modelo basándose en los gradientes calculados por Autograd (por ejemplo, SGD, Adam).
*   **Función de pérdida (Loss Function):** 📉 Mide la diferencia entre las predicciones del modelo y los valores reales.  El objetivo del entrenamiento es minimizar esta función.
*   **Dataset y DataLoader:** 📚 Clases para gestionar y cargar datos de entrenamiento de manera eficiente, incluyendo el procesamiento por lotes (batching) y la mezcla (shuffling).
*   **GPU:** 🚀 Unidad de Procesamiento Gráfico. PyTorch permite usar GPUs para acelerar significativamente el entrenamiento y la inferencia.

## 🚀 Operaciones Esenciales

### 🧱 Tensores

1.  **Creación de Tensores:**

    ```python
    import torch

    # Desde datos (listas, arrays de NumPy)
    data = [[1, 2], [3, 4]]
    x_data = torch.tensor(data)  # Tensor a partir de una lista
    x_np = torch.from_numpy(np.array(data))  # Tensor a partir de un array de NumPy

    # Tensores con valores específicos
    x_ones = torch.ones(2, 3)  # Tensor de unos (2x3)
    x_zeros = torch.zeros(5)   # Tensor de ceros (tamaño 5)
    x_rand = torch.rand(3, 4)  # Tensor de números aleatorios (3x4)
    x_randn = torch.randn(2,2,2) #Tensor con valores aleatorios siguiendo una distribución normal

    # Tensores con forma y tipo de datos específicos
    shape = (2, 3,)
    x_empty = torch.empty(shape) # Sin inicializar
    x_int = torch.randint(0, 10, (2, 5)) # Enteros aleatorios entre 0 y 9 (2x5)
    x_float = torch.ones(shape, dtype=torch.float32) # Tipo float32
    ```

2.  **Atributos de un Tensor:**

    ```python
    tensor = torch.rand(3, 4)

    print(f"Shape: {tensor.shape}")    # Tamaño del tensor
    print(f"Dtype: {tensor.dtype}")    # Tipo de datos
    print(f"Device: {tensor.device}")  # Dispositivo (CPU o GPU)
    ```

3.  **Operaciones con Tensores:**

    ```python
    # Operaciones aritméticas
    y = x + 2  # Suma elemento a elemento
    z = x * y  # Multiplicación elemento a elemento
    w = torch.matmul(x, y.T) # Multiplicación matricial (y.T es la transpuesta)
    # También: x @ y.T

    # Indexación y slicing (similar a NumPy)
    first_row = tensor[0]       # Primera fila
    first_column = tensor[:, 0] # Primera columna
    last_element = tensor[-1, -1] # Último elemento

    # Cambio de forma
    x_reshaped = x.reshape(4, 3)  # Cambia la forma a 4x3
    x_viewed = x.view(12)       # Otra forma de cambiar la forma (más eficiente si es posible)
    x_flatten = x.flatten() # Aplana el tensor

    # Transposición
    x_transposed = x.T

    # Concatenación
    x_concat = torch.cat([x, x, x], dim=0)  # Concatena en la dimensión 0 (filas)

    # Operaciones inplace (modifican el tensor original, terminan con _)
    x.add_(5)   # Suma 5 a x (inplace)

    ```

4. **Mover tensores a la GPU (si está disponible):**

    ```python
    if torch.cuda.is_available():
      tensor = tensor.to('cuda')  # Mueve el tensor a la GPU
      # También se puede crear directamente en GPU:
      x_gpu = torch.ones(5, device='cuda')

    # Volver a CPU:
    tensor_cpu = tensor.to('cpu') # O .cpu()
    ```

### ⚙️ Autograd y Gradientes

1.  **Habilitar el seguimiento de gradientes:**

    ```python
    x = torch.ones(2, 2, requires_grad=True) # requires_grad=True habilita Autograd
    ```

2.  **Realizar operaciones y calcular gradientes:**

    ```python
    y = x + 2
    z = y * y * 3
    out = z.mean() # Calcula la media

    out.backward()  # Calcula los gradientes (backpropagation)

    print(x.grad)   # Gradientes de out con respecto a x
    ```

3.  **Desactivar el seguimiento de gradientes (para inferencia):**

    ```python
    with torch.no_grad():
        y = x + 2  # No se calcularán gradientes
    # O:
    x.requires_grad_(False)
    ```
4. **Detachment**:
    ```python
    y = x.detach() # crea un tensor nuevo, sin historico de operaciones
    ```

### 🧠 Módulos (Redes Neuronales)

1.  **Definir un módulo personalizado:**

    ```python
    import torch.nn as nn
    import torch.nn.functional as F

    class MiRed(nn.Module):
        def __init__(self):
            super(MiRed, self).__init__()  # Llama al constructor de la clase base
            self.fc1 = nn.Linear(784, 128)  # Capa lineal (fully connected)
            self.fc2 = nn.Linear(128, 10)   # Capa de salida

        def forward(self, x):
            x = F.relu(self.fc1(x))  # Activación ReLU
            x = self.fc2(x)
            return x
    ```

2.  **Capas comunes (en `torch.nn`):**

    *   `nn.Linear`: Capa lineal (fully connected).
    *   `nn.Conv2d`: Capa convolucional 2D.
    *   `nn.ReLU`: Activación ReLU.
    *   `nn.Sigmoid`: Activación sigmoide.
    *   `nn.Tanh`: Activación tangente hiperbólica.
    *   `nn.MaxPool2d`: Max pooling 2D.
    *   `nn.Dropout`: Dropout (regularización).
    *   `nn.BatchNorm2d`: Batch normalization.
    *   `nn.Embedding`: Capa de embedding (para texto, por ejemplo).
    *   `nn.LSTM`, `nn.GRU`: Capas recurrentes.

3. **Funciones de activación (en `torch.nn.functional`):**
    *  `F.relu`, `F.sigmoid`, `F.tanh`, `F.softmax`

4.  **Crear una instancia del módulo y moverlo a la GPU:**

    ```python
    modelo = MiRed()
    if torch.cuda.is_available():
        modelo = modelo.to('cuda')
    ```

5.  **Obtener los parámetros del modelo:**

    ```python
    parametros = modelo.parameters()
    ```

### 📈 Optimizadores

1.  **Crear un optimizador:**

    ```python
    import torch.optim as optim

    optimizador = optim.SGD(modelo.parameters(), lr=0.01, momentum=0.9)  # SGD
    optimizador_adam = optim.Adam(modelo.parameters(), lr=0.001)        # Adam
    ```

2.  **Pasos del entrenamiento:**

    ```python
    # Bucle de entrenamiento
    for epoca in range(num_epocas):
        for entradas, etiquetas in dataloader:
            entradas = entradas.to(device) # Mueve a GPU si es necesario
            etiquetas = etiquetas.to(device)

            # 1. Pasar los datos por el modelo (forward pass)
            salidas = modelo(entradas)

            # 2. Calcular la pérdida
            perdida = funcion_perdida(salidas, etiquetas)

            # 3. Limpiar los gradientes anteriores
            optimizador.zero_grad()

            # 4. Calcular los gradientes (backward pass)
            perdida.backward()

            # 5. Actualizar los parámetros
            optimizador.step()
    ```

### 📉 Funciones de Pérdida

1.  **Funciones de pérdida comunes (en `torch.nn`):**

    *   `nn.MSELoss`: Error cuadrático medio (regresión).
    *   `nn.CrossEntropyLoss`: Entropía cruzada (clasificación).  *Combina `LogSoftmax` y `NLLLoss`.*
    *   `nn.NLLLoss`: Negative log likelihood (clasificación, después de aplicar `LogSoftmax`).
    *   `nn.BCELoss`: Entropía cruzada binaria (clasificación binaria).
    *   `nn.BCEWithLogitsLoss`: Combina `Sigmoid` y `BCELoss`.  *Más estable numéricamente.*

### 📚 Datasets y DataLoaders

1.  **Crear un Dataset personalizado:**

    ```python
    from torch.utils.data import Dataset, DataLoader

    class MiDataset(Dataset):
        def __init__(self, datos, etiquetas, transform=None):
            self.datos = datos
            self.etiquetas = etiquetas
            self.transform = transform

        def __len__(self):
            return len(self.datos)

        def __getitem__(self, idx):
            muestra = self.datos[idx]
            etiqueta = self.etiquetas[idx]
            if self.transform:
                muestra = self.transform(muestra)
            return muestra, etiqueta

    # Ejemplo de uso
    dataset = MiDataset(mis_datos, mis_etiquetas, transform=mi_transformacion)

    ```

2.  **Crear un DataLoader:**

    ```python
    dataloader = DataLoader(dataset, batch_size=32, shuffle=True, num_workers=4)
    ```
    *   `batch_size`:  Tamaño del lote (batch).
    *   `shuffle`:  Mezclar los datos en cada época.
    *   `num_workers`:  Número de procesos para cargar los datos en paralelo.

3. **Datasets predefinidos (en `torchvision.datasets`):**
    * `MNIST`, `CIFAR10`, `CIFAR100`, `ImageNet`

4. **Transformaciones (en `torchvision.transforms`):**
    * `ToTensor`, `Normalize`, `Resize`, `RandomCrop`, `RandomHorizontalFlip`

### 💾 Guardar y Cargar Modelos

1.  **Guardar el modelo:**

    ```python
    torch.save(modelo.state_dict(), 'modelo.pth')  # Guarda solo los parámetros
    # O, para guardar todo el modelo (incluyendo la arquitectura):
    torch.save(modelo, 'modelo_completo.pth') # Menos recomendado
    ```

2.  **Cargar el modelo:**

    ```python
    modelo = MiRed() # Primero se crea la instancia del mismo tipo de red
    modelo.load_state_dict(torch.load('modelo.pth')) # Carga los parámetros
    modelo.eval()  # Importante: Pone el modelo en modo de evaluación (desactiva dropout, batchnorm, etc.)

    # Si se guardó el modelo completo:
    # modelo = torch.load('modelo_completo.pth') # No es necesario crear la instancia antes
    ```

### 📝 Otros

*   **torch.manual_seed(semilla):**  Fija la semilla aleatoria para reproducibilidad.

* **torch.cuda.is_available():**  Comprueba si hay una GPU disponible.

* **torch.cuda.device_count()**: Devuelve el número de GPUs

*  **torch.cuda.get_device_name(0)**:  Obtiene el nombre de la GPU.

*  **Funciones útiles**:
    * `torch.argmax(tensor, dim=1)`: Devuelve los índices de los valores máximos a lo largo de una dimensión.
    *  `torch.sum()`, `torch.mean()`, `torch.max()`, `torch.min()`, `torch.std()`
    * `torch.clamp(tensor, min, max)`

Este cheatsheet cubre lo esencial de PyTorch.  ¡Recuerda consultar la documentación oficial para más detalles y funciones avanzadas!