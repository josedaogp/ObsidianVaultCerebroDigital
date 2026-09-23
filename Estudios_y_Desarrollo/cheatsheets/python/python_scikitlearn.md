#  Scikit-learn (sklearn) Cheatsheet: Machine Learning en Python 🚀

## 🌟 **Conceptos Fundamentales**

*   **Scikit-learn:** Biblioteca de Python de código abierto para *machine learning*.  Proporciona herramientas simples y eficientes para:
    *   Clasificación.
    *   Regresión.
    *   Clustering.
    *   Reducción de dimensionalidad.
    *   Selección de modelos.
    *   Preprocesamiento de datos.
*   **Estimadores (Estimators):**  Objetos que aprenden de los datos.  Tienen un método `fit()` para entrenar el modelo y un método `predict()` para hacer predicciones.  Ejemplos: `LinearRegression`, `LogisticRegression`, `SVC`, `KMeans`, etc.
*   **Transformadores (Transformers):**  Objetos que transforman los datos.  Tienen un método `fit()` para aprender los parámetros de la transformación y un método `transform()` para aplicar la transformación.  Algunos también tienen `fit_transform()`.  Ejemplos: `StandardScaler`, `MinMaxScaler`, `PCA`, etc.
*   **Pipelines:**  Encadenan varios estimadores y/o transformadores en un solo objeto.
*   **Métricas (Metrics):**  Funciones para evaluar el rendimiento de los modelos.
*   **Conjuntos de datos (Datasets):**  Scikit-learn incluye algunos conjuntos de datos de ejemplo (juguete) y funciones para cargar conjuntos de datos más grandes.

## 💻 **Instalación**

```bash
pip install scikit-learn
```

## 🔄 **Flujo de Trabajo General (Workflow)**

1.  **Cargar los datos:**
2.  **Preprocesar los datos:**
    *   Limpieza (manejo de valores faltantes, etc.).
    *   Transformación (escalado, codificación de variables categóricas, etc.).
    *   Reducción de dimensionalidad (si es necesario).
3.  **Dividir los datos:** En conjuntos de entrenamiento (training), validación (validation) y prueba (test).
4.  **Elegir un modelo:**
5.  **Entrenar el modelo:** Usar el método `fit()` con los datos de entrenamiento.
6.  **Evaluar el modelo:** Usar métricas apropiadas (accuracy, precision, recall, F1-score, MSE, R², etc.) con los datos de validación/prueba.
7.  **Ajustar los hiperparámetros:** Usar técnicas como *grid search* o *randomized search* para encontrar los mejores hiperparámetros del modelo.
8.  **Hacer predicciones:** Usar el método `predict()` con nuevos datos.

## 📚 **Módulos Principales**

*   **`sklearn.preprocessing`:**  Preprocesamiento de datos.
*   **`sklearn.model_selection`:**  Selección de modelos, validación cruzada, ajuste de hiperparámetros.
*   **`sklearn.linear_model`:**  Modelos lineales (regresión lineal, regresión logística, etc.).
*   **`sklearn.svm`:**  Support Vector Machines (SVM).
*   **`sklearn.tree`:**  Árboles de decisión.
*   **`sklearn.ensemble`:**  Métodos de ensemble (Random Forest, Gradient Boosting, etc.).
*   **`sklearn.neighbors`:**  Vecinos más cercanos (k-NN).
*   **`sklearn.naive_bayes`:**  Naive Bayes.
*   **`sklearn.cluster`:**  Clustering (K-Means, etc.).
*   **`sklearn.decomposition`:**  Reducción de dimensionalidad (PCA, etc.).
*   **`sklearn.metrics`:**  Métricas de evaluación.
*   **`sklearn.pipeline`:**  Pipelines.
*   **`sklearn.datasets`:**  Conjuntos de datos de ejemplo.
*  **`sklearn.impute`**: Imputación de valores perdidos.

## 💾 **Carga de Datos (Datasets)**

*   **Datasets de juguete (toy datasets):**
    *   `load_boston()` (regresión, *obsoleto*).
    *   `load_iris()` (clasificación).
    *   `load_diabetes()` (regresión).
    *   `load_digits()` (clasificación).
    *   `load_wine()` (clasificación).
    *   `load_breast_cancer()` (clasificación).

    ```python
    from sklearn.datasets import load_iris

    iris = load_iris()
    X = iris.data  # Características (features)
    y = iris.target  # Etiquetas (targets)
    feature_names = iris.feature_names
    ```

*   **Cargar datos desde archivos:**  Generalmente se usa pandas para cargar datos desde archivos CSV, Excel, etc.

    ```python
    import pandas as pd

    df = pd.read_csv("mi_archivo.csv")
    X = df[["columna1", "columna2"]]  # Seleccionar columnas de características
    y = df["etiqueta"]  # Seleccionar la columna de etiquetas
    ```
* **`sklearn.datasets.fetch_*`**: Funciones para descargar y cargar datasets más grandes.

## 🧹 **Preprocesamiento (`sklearn.preprocessing`)**

*   **Escalado:**
    *   **`StandardScaler`:**  Estandariza los datos (media 0, desviación estándar 1).
    *   **`MinMaxScaler`:**  Escala los datos a un rango (por defecto, [0, 1]).
    *   **`RobustScaler`:**  Escalado robusto a outliers (usa la mediana y el rango intercuartílico).
    *   **`MaxAbsScaler`:**  Escala los datos dividiendo por el valor máximo absoluto.
    *   **`Normalizer`:** Normaliza cada *muestra* (fila) a la unidad de norma (útil para texto).

    ```python
    from sklearn.preprocessing import StandardScaler

    scaler = StandardScaler()
    X_scaled = scaler.fit_transform(X)  # Ajusta y transforma
    # X_scaled = scaler.transform(X) # Si ya se ajustó con otros datos

    ```

*   **Codificación de variables categóricas:**
    *   **`OneHotEncoder`:**  Crea variables dummy (one-hot encoding).
    *   **`OrdinalEncoder`:**  Codifica variables categóricas como enteros (para variables ordinales).
    * **`LabelEncoder`**: Codifica las etiquetas *y* como enteros (generalmente no se usa para las *X*).

    ```python
    from sklearn.preprocessing import OneHotEncoder

    enc = OneHotEncoder(handle_unknown='ignore') # handle_unknown='ignore' para evitar errores con valores nuevos
    X_encoded = enc.fit_transform(X_categorical) # X_categorical: matriz con variables categóricas.

    ```

*   **Imputación de valores faltantes:**
    *   **`SimpleImputer`:**  Imputa valores faltantes usando la media, la mediana, el valor más frecuente o un valor constante.
    *   **`KNNImputer`:**  Imputa valores faltantes usando el algoritmo k-NN.
    *   **`IterativeImputer`:** Imputación multivariante (usa un modelo para predecir los valores faltantes).

    ```python
    from sklearn.impute import SimpleImputer

    imputer = SimpleImputer(strategy="mean")  # Reemplaza los valores faltantes por la media
    X_imputed = imputer.fit_transform(X)

    ```
* **`PolynomialFeatures`**: Genera características polinómicas e interacciones.
* **`FunctionTransformer`**: Aplica una función arbitraria a los datos.
*  **Discretización:**
    *   **`KBinsDiscretizer`:**  Convierte variables numéricas en variables categóricas (discretización).

## ✂️ **División de Datos (`sklearn.model_selection`)**

*   **`train_test_split`:**  Divide los datos en conjuntos de entrenamiento y prueba.

    ```python
    from sklearn.model_selection import train_test_split

    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)  # 80% entrenamiento, 20% prueba
    # stratify=y: Para mantener la proporción de clases en problemas de clasificación.
    ```

* **Validación cruzada (Cross-validation):**
    *   **`cross_val_score`:**  Evalúa un modelo usando validación cruzada.

        ```python
        from sklearn.model_selection import cross_val_score
        from sklearn.linear_model import LogisticRegression

        model = LogisticRegression()
        scores = cross_val_score(model, X, y, cv=5)  # Validación cruzada de 5 folds
        print(f"Accuracy: {scores.mean()} (+/- {scores.std() * 2})")

        ```

    *   **`KFold`:**  Iterador de validación cruzada K-Fold.
    *   **`StratifiedKFold`:**  K-Fold estratificado (mantiene la proporción de clases en cada fold).
    *   **`ShuffleSplit`:**  Genera divisiones aleatorias de entrenamiento/prueba.
    *   **`LeaveOneOut`:**  Deja un ejemplo fuera (Leave-One-Out).
    *   **`GroupKFold`:**  K-Fold con grupos (para datos donde las muestras de un mismo grupo no deben estar en diferentes folds).

## 🤖 **Modelos (Estimadores)**

### 📈 **Regresión**

*   **`LinearRegression`:**  Regresión lineal ordinaria.
*   **`Ridge`:**  Regresión lineal con regularización L2 (ridge regression).
*   **`Lasso`:**  Regresión lineal con regularización L1 (lasso regression).
*   **`ElasticNet`:**  Regresión lineal con regularización L1 y L2 (elastic net).
*   **`LogisticRegression`:**  Regresión logística (para clasificación, a pesar del nombre).
*   **`SGDRegressor`:**  Regresión lineal con descenso de gradiente estocástico (SGD).
*  `SVR`: Support Vector Regressor.
*  `DecisionTreeRegressor`
* `RandomForestRegressor`, `GradientBoostingRegressor`, `HistGradientBoostingRegressor` (Ensembles)
* `KNeighborsRegressor`

```python
from sklearn.linear_model import LinearRegression

model = LinearRegression()
model.fit(X_train, y_train)
y_pred = model.predict(X_test)
print(f"Coeficientes: {model.coef_}")
print(f"Intercepto: {model.intercept_}")

```

### 🏷️ **Clasificación**

*   **`LogisticRegression`:**  Regresión logística.
*   **`SVC` (Support Vector Classifier):**  Máquina de vectores de soporte (SVM).
*   **`DecisionTreeClassifier`:**  Árbol de decisión.
*   **`RandomForestClassifier`:**  Random Forest.
*   **`GradientBoostingClassifier`:**  Gradient Boosting.
*  `HistGradientBoostingClassifier`: Gradient Boosting basado en histogramas (más rápido para datasets grandes).
*   **`KNeighborsClassifier`:**  k-NN.
*   **`GaussianNB`:**  Naive Bayes Gaussiano.
*   **`SGDClassifier`:**  Clasificador lineal con descenso de gradiente estocástico (SGD).

```python
from sklearn.svm import SVC

model = SVC(kernel="rbf", C=1.0, gamma="scale")
model.fit(X_train, y_train)
y_pred = model.predict(X_test)
print(f"Accuracy: {model.score(X_test, y_test)}")

```

### 🌳 **Clustering**

*   **`KMeans`:**  K-Means clustering.
*   **`MiniBatchKMeans`**: Versión más rápida de KMeans (para datasets grandes).
*   **`DBSCAN`:**  DBSCAN clustering.
*   **`AgglomerativeClustering`:**  Clustering aglomerativo.
*   **`MeanShift`:** Mean Shift clustering.
*  `AffinityPropagation`
* `SpectralClustering`

```python
from sklearn.cluster import KMeans

model = KMeans(n_clusters=3, random_state=42)
model.fit(X) # KMeans no supervisado, no necesita y
labels = model.labels_
centroids = model.cluster_centers_

```

### 📉 **Reducción de Dimensionalidad**

*   **`PCA` (Principal Component Analysis):**  Análisis de componentes principales.
*   **`TruncatedSVD`:**  Descomposición en valores singulares truncada (SVD).  Útil para matrices dispersas.
*   **`NMF` (Non-negative Matrix Factorization):**  Factorización de matrices no negativas.
* `FastICA`: Independent Component Analysis.

```python
from sklearn.decomposition import PCA

pca = PCA(n_components=2)  # Reduce a 2 dimensiones
X_reduced = pca.fit_transform(X)
print(f"Varianza explicada: {pca.explained_variance_ratio_}")
```

## 📏 **Métricas de Evaluación (`sklearn.metrics`)**

### Clasificación

*   **`accuracy_score`:**  Exactitud (accuracy).
*   **`precision_score`:**  Precisión.
*   **`recall_score`:**  Recall (sensibilidad).
*   **`f1_score`:**  F1-score (media armónica de precisión y recall).
*   **`roc_auc_score`:**  Área bajo la curva ROC (AUC).
*   **`confusion_matrix`:**  Matriz de confusión.
*   **`classification_report`:**  Informe de clasificación (precision, recall, F1-score, support).
*  `roc_curve`: Calcula la curva ROC.
*  `average_precision_score`:  Calcula el área bajo la curva Precision-Recall.
*  `log_loss`: Pérdida logarítmica (log loss).

```python
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score, confusion_matrix

print(f"Accuracy: {accuracy_score(y_test, y_pred)}")
print(f"Precision: {precision_score(y_test, y_pred)}")
print(f"Recall: {recall_score(y_test, y_pred)}")
print(f"F1-score: {f1_score(y_test, y_pred)}")
print(f"Matriz de confusión:\n{confusion_matrix(y_test, y_pred)}")

```

### Regresión

*   **`mean_squared_error` (MSE):**  Error cuadrático medio.
*   **`mean_absolute_error` (MAE):**  Error absoluto medio.
*   **`r2_score`:**  R² (coeficiente de determinación).
*   **`explained_variance_score`:**  Varianza explicada.
* `max_error`: Error máximo.
* `mean_squared_log_error`: Error cuadrático logarítmico medio.

```python
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score

print(f"MSE: {mean_squared_error(y_test, y_pred)}")
print(f"MAE: {mean_absolute_error(y_test, y_pred)}")
print(f"R²: {r2_score(y_test, y_pred)}")

```
### Clustering

* `adjusted_rand_score`: Adjusted Rand Index.
* `silhouette_score`: Silhouette Coefficient.
* `completeness_score`, `homogeneity_score`, `v_measure_score`: Homogeneidad, completitud y V-measure.

## ⚙️ **Ajuste de Hiperparámetros (`sklearn.model_selection`)**

*   **`GridSearchCV`:**  Búsqueda exhaustiva en una cuadrícula de hiperparámetros.
*   **`RandomizedSearchCV`:**  Búsqueda aleatoria de hiperparámetros.

```python
from sklearn.model_selection import GridSearchCV
from sklearn.svm import SVC

param_grid = {
    "C": [0.1, 1, 10],
    "kernel": ["linear", "rbf"],
    "gamma": ["scale", "auto"]
}

model = SVC()
grid_search = GridSearchCV(model, param_grid, cv=5)
grid_search.fit(X_train, y_train)

print(f"Mejores parámetros: {grid_search.best_params_}")
print(f"Mejor puntuación: {grid_search.best_score_}")
best_model = grid_search.best_estimator_ # Mejor modelo

```

## ⛓️ **Pipelines (`sklearn.pipeline`)**

*   **`Pipeline`:**  Encadena varios pasos (transformadores y un estimador final).

    ```python
    from sklearn.pipeline import Pipeline
    from sklearn.preprocessing import StandardScaler
    from sklearn.linear_model import LogisticRegression

    pipeline = Pipeline([
        ("scaler", StandardScaler()),  # Paso 1: Escalado
        ("model", LogisticRegression())  # Paso 2: Regresión logística
    ])

    pipeline.fit(X_train, y_train)
    y_pred = pipeline.predict(X_test)

    ```

*   **`make_pipeline`:**  Función para crear pipelines de forma más concisa (no es necesario nombrar los pasos).

* **`FeatureUnion`**: Combina los resultados de varios *transformadores*.

## ➕ **Otros Módulos y Funciones Útiles**

*   **`sklearn.feature_selection`:**  Selección de características.
    *   `SelectKBest`, `SelectPercentile`:  Seleccionan las mejores características según una métrica.
    *   `RFE` (Recursive Feature Elimination):  Eliminación recursiva de características.
    *   `VarianceThreshold`: Elimina características con baja varianza.
*  **`sklearn.compose`**: Herramientas para aplicar transformaciones a diferentes columnas.
    *   **`ColumnTransformer`:**  Aplica diferentes transformadores a diferentes columnas.
*   **`sklearn.dummy`:**  Estimadores "dummy" (para comparar con modelos reales).
    *   `DummyClassifier`, `DummyRegressor`.
* **`sklearn.calibration`**: Calibración de probabilidades.
* **`sklearn.utils`**: Funciones de utilidad.