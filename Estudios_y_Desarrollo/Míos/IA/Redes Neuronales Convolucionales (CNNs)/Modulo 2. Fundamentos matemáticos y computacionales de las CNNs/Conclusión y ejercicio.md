- La **convolución** es la operación básica que se realiza en las CNNs, donde un filtro se aplica a una imagen para extraer características.
- El **pooling** reduce la dimensionalidad de la imagen, ayudando a la eficiencia y a la robustez del modelo.
- Las **funciones de activación** como ReLU introducen no linealidades que permiten a la red aprender relaciones complejas.

**Ejercicio Propuesto**:

1. Implementa una operación de convolución y pooling desde cero en Python para entender cómo funcionan internamente.
2. Experimenta con diferentes tamaños de filtro, stride y padding y observa cómo afecta al resultado.

**Código de ejemplo (convolución y pooling)**:
``` python
import numpy as np

# Imagen de entrada 5x5
image = np.array([[1, 2, 3, 0, 1],
                  [4, 5, 6, 1, 0],
                  [7, 8, 9, 0, 1],
                  [1, 2, 3, 0, 1],
                  [4, 5, 6, 1, 0]])

# Filtro (3x3)
filter = np.array([[1, 0, -1],
                   [1, 0, -1],
                   [1, 0, -1]])

# Operación de convolución (producto punto)
def convolve2d(image, filter):
    height, width = image.shape
    filter_height, filter_width = filter.shape
    output_height = height - filter_height + 1
    output_width = width - filter_width + 1
    output = np.zeros((output_height, output_width))

    for i in range(output_height):
        for j in range(output_width):
            output[i, j] = np.sum(image[i:i+filter_height, j:j+filter_width] * filter)
    return output

# Convolución
conv_result = convolve2d(image, filter)
print("Resultado de la convolución:")
print(conv_result)

# Max Pooling (2x2)
def max_pooling(image, pool_size=2):
    height, width = image.shape
    pool_height, pool_width = pool_size, pool_size
    output_height = height // pool_height
    output_width = width // pool_width
    output = np.zeros((output_height, output_width))

    for i in range(output_height):
        for j in range(output_width):
            output[i, j] = np.max(image[i*pool_height:(i+1)*pool_height, j*pool_width:(j+1)*pool_width])
    return output

# Max Pooling
pool_result = max_pooling(conv_result)
print("Resultado del Max Pooling:")
print(pool_result)
```
Este código realiza convolución y pooling en una imagen simple. Experimenta con diferentes tamaños de filtro y pooling para ver cómo cambia el resultado.