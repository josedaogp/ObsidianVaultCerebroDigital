# ☁️ Cheatsheet de Vertex AI

Este cheatsheet cubre los aspectos esenciales de Vertex AI, la plataforma unificada de Google Cloud para machine learning (ML).

## 🔍 Conceptos Básicos

*   **Vertex AI:** Plataforma de ML unificada de Google Cloud que ofrece herramientas para todo el flujo de trabajo de ML, desde la preparación de datos hasta el despliegue y la gestión de modelos.
*   **Modelo (Model):** 🧠 Representación entrenada de un algoritmo de ML.  Puede ser un modelo personalizado (entrenado por ti) o un modelo pre-entrenado de Google (ej. modelos de AutoML, modelos de la API de Vision, etc.).
*   **Dataset:** 🗂️ Colección de datos utilizada para entrenar un modelo de ML.  Vertex AI admite varios tipos de datos (imágenes, texto, tablas, video).
*   **Entrenamiento (Training):** 💪 Proceso de aprendizaje de los parámetros de un modelo a partir de un dataset.
*   **Endpoint:** 🚀 Punto de acceso para realizar predicciones con un modelo desplegado.
*   **Predicción (Prediction):** 🔮 Proceso de obtener inferencias de un modelo a partir de nuevos datos.
*   **Batch Prediction:** Predicciones en lote (para grandes volúmenes de datos).
*   **Online Prediction:** Predicciones en tiempo real (baja latencia).
*   **Pipeline (Vertex AI Pipelines):** ⚙️ Flujo de trabajo de ML orquestado y automatizado (basado en Kubeflow Pipelines o TFX).
*   **Feature Store:** 🗄️ Repositorio centralizado para gestionar y compartir *features* (características) de ML.
*   **Metadata (Vertex AI Metadata):** 📝 Información sobre los artefactos de ML (datasets, modelos, endpoints, etc.).
*   **Workbench:** Entorno de desarrollo basado en JupyterLab.
*  **Matching Engine:** Para búsqueda de similitud vectorial a gran escala.
* **Vizier:** Servicio para optimización de hiperparámetros.
* **TensorBoard:** Para visualización de experimentos.
* **Explainable AI:** Para entender las predicciones.

## 🔑 APIs y SDKs

*   **API de Vertex AI:**  API REST para interactuar con Vertex AI.
*   **SDKs de cliente:**
    *   **Python (recomendado):**  `google-cloud-aiplatform` (instalación: `pip install google-cloud-aiplatform`).
    *   **Java:**  `google-cloud-vertexai`.
    *   **Node.js:**  `@google-cloud/aiplatform`.
    *   **Go:**  `cloud.google.com/go/vertexai`.
    *   **Otros:** C#, Ruby, PHP.
*   **Cloud SDK (gcloud CLI):**  Herramienta de línea de comandos de Google Cloud (`gcloud`).

## 💻 Configuración Inicial (Python)

1.  **Autenticación:**

    *   **Cuenta de servicio (recomendado para producción):**
        *   Crea una cuenta de servicio en la consola de Google Cloud (IAM & Admin > Service accounts).
        *   Otorga los roles necesarios (ej. `roles/aiplatform.user`, `roles/storage.objectAdmin`).
        *   Descarga el archivo JSON de la clave de la cuenta de servicio.
        *   Establece la variable de entorno `GOOGLE_APPLICATION_CREDENTIALS`:

        ```bash
        export GOOGLE_APPLICATION_CREDENTIALS="/ruta/a/tu/credencial.json"
        ```

    *   **Credenciales de usuario (para desarrollo local):**

        ```bash
        gcloud auth application-default login
        ```

2.  **Inicializar el SDK de Python:**

    ```python
    import vertexai

    PROJECT_ID = "tu-proyecto-id"  # Reemplaza con tu ID de proyecto
    REGION = "us-central1"  # Reemplaza con tu región (ej. us-central1, europe-west4)

    vertexai.init(project=PROJECT_ID, location=REGION)
    ```

## 🗂️ Datasets

1.  **Tipos de datasets:**

    *   **Tabular:** Datos estructurados en filas y columnas (ej. CSV, BigQuery).
    *   **Image:** Imágenes.
        *   Clasificación de imágenes (Image Classification).
        *   Detección de objetos (Image Object Detection).
    *   **Text:** Texto.
        *   Clasificación de texto (Text Classification).
        *   Análisis de sentimiento (Text Sentiment Analysis).
        *   Extracción de entidades (Text Entity Extraction).
    *   **Video:** Videos.
        *   Clasificación de video (Video Classification).
        *   Detección de objetos en video (Video Object Tracking).
        *   Reconocimiento de acciones (Video Action Recognition).
    * **Otros:** Time series.

2.  **Crear un dataset (Python):**

    ```python
    from google.cloud import aiplatform

    # Dataset tabular
    dataset = aiplatform.TabularDataset.create(
        display_name="mi-dataset-tabular",
        gcs_source=["gs://mi-bucket/datos.csv"]  # O bigquery_source="bq://proyecto.dataset.tabla"
    )

    # Dataset de imágenes
    dataset = aiplatform.ImageDataset.create(
        display_name="mi-dataset-imagenes",
        gcs_source=["gs://mi-bucket/images.jsonl"],  # Archivo JSONL con la ubicación de las imágenes y etiquetas
        import_schema_uri=aiplatform.schema.dataset.metadata.image.classification #O detection, etc.
    )
    # TextDataset, VideoDataset...
    ```

3.  **Importar datos:** (Si no se hace al crear el dataset).

    ```python
     #Importar datos a un dataset ya creado
     dataset.import_data(
          gcs_source=gcs_source_uri,
          import_schema_uri=import_schema_uri
      )
    ```
4. **Listar datasets:**
    ```python
    datasets = aiplatform.TabularDataset.list() #O ImageDataset, etc.
    ```

5.  **Obtener un dataset:**

    ```python
    dataset = aiplatform.TabularDataset("projects/PROJECT_ID/locations/REGION/datasets/DATASET_ID")
    ```

6.  **Eliminar un dataset:**

    ```python
    dataset.delete()
    ```
7. **Exportar datos:**
    ```python
        dataset.export_data(
            gcs_destination_output_uri_prefix="gs://my_bucket",
            export_format="tf-record" # O "csv", etc, dependiendo del tipo de dato
        )
    ```

## 🧠 Modelos (Entrenamiento)

### 1. Modelos AutoML

*   Entrenamiento automatizado (no requiere escribir código de ML).
*   Admite datasets tabulares, de imágenes, de texto y de video.

```python
# Ejemplo: Entrenamiento de un modelo AutoML de clasificación de imágenes
from google.cloud import aiplatform

# Crear un Job de entrenamiento
job = aiplatform.AutoMLImageClassificationTrainingJob(
    display_name="mi-entrenamiento-automl-imagenes",
    model_type="CLOUD",  # O "MOBILE" para modelos Edge
    budget_milli_node_hours=1000, # Presupuesto (opcional, en milinodos-hora)
)

# Iniciar el entrenamiento
model = job.run(
    dataset=dataset,  # Dataset creado previamente
    model_display_name="mi-modelo-automl-imagenes",
    training_fraction_split=0.8, # Proporción de datos para entrenamiento
    validation_fraction_split=0.1, # Proporción de datos para validación
    test_fraction_split=0.1, # Proporción para test
)

print(f"Modelo entrenado: {model.resource_name}")
```

*   **`AutoMLTabularTrainingJob`:**  Para datos tabulares.
*   **`AutoMLImageClassificationTrainingJob` / `AutoMLImageObjectDetectionTrainingJob`:**  Para imágenes.
*   **`AutoMLTextClassificationTrainingJob` / `AutoMLTextSentimentAnalysisTrainingJob` / `AutoMLTextEntityExtractionTrainingJob`:**  Para texto.
*   **`AutoMLVideoClassificationTrainingJob` / `AutoMLVideoObjectTrackingTrainingJob` / `AutoMLVideoActionRecognitionTrainingJob`:**  Para video.

### 2. Modelos Personalizados (Custom Training)

*   Mayor control sobre el proceso de entrenamiento.
*   Permite usar cualquier framework de ML (TensorFlow, PyTorch, scikit-learn, XGBoost, etc.).
*   Se define una *tarea de entrenamiento* (training job).
*  Se puede usar un contenedor pre-construido, o uno personalizado.

```python
# Ejemplo: Entrenamiento personalizado con un script de Python (usando un contenedor pre-construido)
from google.cloud import aiplatform

# Definir la especificación del worker pool (qué tipo de máquina usar)
worker_pool_specs = [
    {
        "machine_spec": {
            "machine_type": "n1-standard-4",  # Tipo de máquina
            "accelerator_type": "NVIDIA_TESLA_T4", # Opcional, GPU
            "accelerator_count": 1,
        },
        "replica_count": 1,
        "container_spec": { # Contenedor pre-construido
            "image_uri": "us-docker.pkg.dev/vertex-ai/training/tf-cpu.2-12:latest", # Imagen de contenedor
            "command": ["python", "-m", "trainer.task"],  # Comando a ejecutar
            "args": [  # Argumentos para el script
                "--data-dir",
                "gs://tu-bucket/datos",
                "--output-dir",
                "gs://tu-bucket/modelo",
                "--epochs",
                "10",
            ],
        },
    }
]

# Crear el job de entrenamiento
job = aiplatform.CustomJob(
    display_name="mi-entrenamiento-personalizado",
    worker_pool_specs=worker_pool_specs,
    staging_bucket="gs://tu-bucket/staging", # Bucket para archivos temporales
)


# Ejecutar el job (sin blocking)
model = job.run(sync = False) # O, job.submit()

```

* **`CustomJob`:**  Para trabajos de entrenamiento personalizados más genéricos.
*  **`PythonPackageTrainingJob`**: Empaqueta el código Python.
*  **`CustomContainerTrainingJob`**: Usa un contenedor personalizado.
* **`worker_pool_specs`:**  Define la configuración de las máquinas virtuales (VMs) para el entrenamiento.
    *   `machine_spec`:  Tipo de máquina, acelerador (GPU/TPU), etc.
    *   `replica_count`:  Número de réplicas (para entrenamiento distribuido).
    *   `container_spec` (para contenedores pre-construidos):  Imagen de contenedor, comando a ejecutar, argumentos.
    *   `python_package_spec` (para PythonPackageTrainingJob):  Especifica el script de Python, el paquete, y dependencias.
*  **`staging_bucket`**: Bucket de Cloud Storage para archivos temporales.

### 3. Modelos pre-entrenados
* Vertex AI Model Garden: Colección de modelos.
* Model Registry: Permite registrar modelos desde distintos orígenes.

## 🚀 Despliegue de Modelos (Endpoints y Predicciones)

1.  **Crear un Endpoint:**

    ```python
    endpoint = aiplatform.Endpoint.create(
        display_name="mi-endpoint",
        project=PROJECT_ID,
        location=REGION,
    )
    ```

2.  **Desplegar un modelo en un Endpoint:**

    ```python
    model.deploy(
        endpoint=endpoint,
        deployed_model_display_name="mi-modelo-desplegado",
        machine_type="n1-standard-4",  # Tipo de máquina
        traffic_percentage=100, # Porcentaje de tráfico
         min_replica_count=1,
        max_replica_count=1, #Para autoescalado
        # accelerator_type="NVIDIA_TESLA_T4",  # Opcional (GPU)
        # accelerator_count=1,
    )
    ```

3.  **Realizar predicciones online:**

    ```python
    predictions = endpoint.predict(instances=[
        {"feature1": valor1, "feature2": valor2},  # Instancia 1
        {"feature1": valor3, "feature2": valor4},  # Instancia 2
    ])

    print(predictions.predictions)
    ```

4.  **Realizar predicciones en lote (Batch Prediction):**
    * Crea un `BatchPredictionJob`.

    ```python
     batch_prediction_job = model.batch_predict(
        job_display_name="mi-prediccion-batch",
        instances_format="jsonl", # O "csv", "tfrecord", etc
        predictions_format="jsonl",
        gcs_source=["gs://mi-bucket/datos_entrada.jsonl"],  # Datos de entrada
        gcs_destination_prefix="gs://mi-bucket/predicciones",  # Dónde guardar los resultados
        machine_type="n1-standard-4",
    )
    ```
5. **Listar endpoints:**
    ```python
        endpoints = aiplatform.Endpoint.list()
    ```
6. **Obtener un endpoint**
    ```python
        endpoint = aiplatform.Endpoint(endpoint_name="projects/.../locations/.../endpoints/ENDPOINT_ID")
    ```

7. **Eliminar un endpoint**
   ```python
        endpoint.delete(force=True) # force=True elimina también los modelos desplegados
   ```

## ⚙️ Vertex AI Pipelines

*   Permiten orquestar flujos de trabajo de ML (entrenamiento, evaluación, despliegue, etc.).
*   Se basan en Kubeflow Pipelines (KFP) o TensorFlow Extended (TFX).
*   Se definen como un grafo de componentes.

```python
# Ejemplo (KFP)
from kfp.v2 import dsl
from kfp.v2.dsl import component
from kfp.v2.google.client import AIPlatformClient

# Definir componentes (cada uno es una tarea en el pipeline)
@component
def componente1(parametro1: str, parametro2: int):
    # ... código del componente ...

@component
def componente2(entrada: Input[Dataset], salida: Output[Model]):
    # ... código del componente ...

# Definir el pipeline
@dsl.pipeline(name="mi-pipeline")
def mi_pipeline(parametro1: str, parametro2: int):
    tarea1 = componente1(parametro1=parametro1, parametro2=parametro2)
    tarea2 = componente2(entrada=tarea1.outputs['salida']) # Conectar salidas a entradas

# Compilar el pipeline
from kfp.v2 import compiler
compiler.Compiler().compile(mi_pipeline, 'pipeline.json')

# Ejecutar el pipeline
api_client = AIPlatformClient(project_id=PROJECT_ID, region=REGION)

response = api_client.create_run_from_job_spec(
    'pipeline.json', # Archivo compilado
    pipeline_root="gs://tu-bucket/pipelines", # Dónde guardar artefactos
    parameter_values={"parametro1": "valor", "parametro2": 123}
)
```

## 🗄️ Feature Store

*   Almacena, gestiona y sirve *features* (características) de ML.
*   **Entity Type:**  Representa una entidad del mundo real (ej. Usuario, Producto).
*   **Feature:**  Atributo de una entidad (ej. edad del usuario, precio del producto).
*   **Featurestore:**  Contenedor de entity types y features.
*   **Online Serving:**  Sirve valores de features para predicciones en tiempo real.
*   **Offline Serving:**  Sirve valores de features para entrenamiento o predicciones en lote.

## 📝 Metadata (Vertex AI Metadata)

*   Registra metadatos sobre los artefactos de ML (datasets, modelos, endpoints, etc.).
*   Permite rastrear el linaje de los datos y modelos.
*  Se usa el servicio `MetadataServiceClient`.

## 🧰 Otras Herramientas y Características

*   **Vertex AI Workbench:**  Entornos de desarrollo basados en JupyterLab.
    *   User-managed notebooks.
    *   Managed notebooks.
*   **Vizier:**  Servicio de optimización de hiperparámetros (black-box optimization).
*   **TensorBoard:**  Herramienta de visualización para experimentos de ML.
*   **Explainable AI:**  Técnicas para entender las predicciones de los modelos.
*   **Matching Engine:**  Búsqueda de similitud vectorial a gran escala.
* **Experimentos (Vertex AI Experiments):** Para organizar y comparar experimentos de ML.

Este cheatsheet cubre una amplia gama de servicios y características de Vertex AI. ¡Guárdalo y consúltalo a menudo!