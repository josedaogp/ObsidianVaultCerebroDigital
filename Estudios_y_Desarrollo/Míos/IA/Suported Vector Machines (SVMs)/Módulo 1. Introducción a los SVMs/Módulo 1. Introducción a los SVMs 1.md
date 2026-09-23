## 1.1 ¿Qué es un SVM?

Un **Support Vector Machine (SVM)** es un algoritmo de aprendizaje supervisado utilizado principalmente para problemas de clasificación y regresión. A su core, su objetivo principal es encontrar un "hiperplano" que separe las clases de datos de manera eficiente. En otras palabras, es como si estuviéramos buscando la mejor línea (en 2D), plano (en 3D) o más general, un hiperplano, que divide dos grupos de datos de forma que maximiza la distancia entre ellos.

### Analogía:
Imagina que tienes un conjunto de puntos de diferentes colores (clases) sobre una mesa. El objetivo del SVM es colocar una regla (hiperplano) sobre la mesa que divida estos puntos de tal manera que los puntos más cercanos de cada color estén lo más alejados posible de la regla. Los puntos más cercanos a la regla son llamados "vectores de soporte", que son los que realmente definen la posición y orientación del hiperplano.

---

## 1.2 Historia y Fundamentos

El concepto de SVM fue propuesto por **Vladimir Vapnik** y **Alexey Chervonenkis** en 1963. El trabajo inicial en SVM se centraba en encontrar soluciones óptimas en clasificación de datos lineales, pero con el tiempo, los SVMs se extendieron para manejar problemas no lineales a través de la introducción de los *kernels*. Estos avances hicieron que SVM fuera muy popular, especialmente en campos como la visión computacional, el procesamiento de lenguaje natural y la biología computacional.

---

## 1.3 Aplicaciones de los SVMs

Los SVMs se pueden usar en una amplia variedad de aplicaciones, incluyendo:
- **Clasificación de imágenes**: Detectar objetos en imágenes o reconocer caras.
- **Clasificación de texto**: Determinar si un email es spam o no.
- **Reconocimiento de voz**: Clasificación de diferentes sonidos o palabras.
- **Biología computacional**: Identificación de genes o proteínas.

---

## 1.4 Introducción al Aprendizaje Supervisado

El **aprendizaje supervisado** es un enfoque en el que el modelo aprende a partir de un conjunto de datos etiquetado, es decir, donde conocemos tanto las características (entrada) como las etiquetas (salida). En el caso de los SVM, las etiquetas corresponden a las clases que estamos intentando predecir (por ejemplo, spam vs no spam).

Los datos son divididos en un conjunto de entrenamiento (para entrenar el modelo) y un conjunto de prueba (para evaluar su rendimiento). El objetivo es entrenar al modelo de manera que pueda generalizar bien y hacer predicciones precisas sobre nuevos datos no vistos.

---
