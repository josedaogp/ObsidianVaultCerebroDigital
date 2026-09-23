## Version 1
### 1. Gestión de Grabaciones (Core)

Estos endpoints gestionan la entidad principal de la nota de voz.

|Endpoint|Función|Cuándo se utiliza|
|---|---|---|
|`POST /recordings/`|Crear registro de grabación|El móvil lo llama al iniciar o terminar la grabación para reservar el ID y guardar metadatos básicos (título, duración, start_time).|
|`GET /recordings/`|Listar grabaciones|Para mostrar el histórico en el móvil o en el Dashboard Web.|
|`GET /recordings/{id}`|Obtener detalle|Al pulsar en una grabación específica para ver su transcripción, resumen y estado.|
|`PATCH /recordings/{id}`|Actualizar metadatos|Para cambiar el título, la categoría o corregir datos manualmente desde la web.|
|`DELETE /recordings/{id}`|Eliminar grabación|Borra el registro, el archivo físico y todos los análisis asociados (insights, tareas).|

Exportar a Hojas de cálculo

### 2. Gestión de Archivos y Audio

Separamos la creación del registro de la subida del archivo para mayor robustez.

|Endpoint|Función|Cuándo se utiliza|
|---|---|---|
|`PUT /recordings/{id}/file`|Subir archivo de audio|Una vez creado el registro, el móvil sube el binario (.m4a/.wav) al servidor o S3.|
|`GET /recordings/{id}/audio-url`|Obtener URL de reproducción|Genera una URL temporal para que el reproductor de la Web o del móvil pueda cargar el sonido.|

Exportar a Hojas de cálculo

### 3. Pipeline de Inteligencia Artificial

Endpoints para controlar el procesamiento pesado de datos.

|Endpoint|Función|Cuándo se utiliza|
|---|---|---|
|`POST /recordings/{id}/analyze`|Disparar análisis de IA|Se llama cuando el archivo ya está subido. El servidor ejecuta la transcripción (si no venía del móvil) y genera el resumen, sentimientos y tareas.|
|`GET /recordings/{id}/status`|Consultar estado de IA|El móvil lo llama mediante _polling_ para saber si la IA ha terminado de "pensar" y cambiar el estado de `procesando` a `completada`.|

Exportar a Hojas de cálculo

### 4. Tareas Detectadas (Action Items)

Gestión individual de las tareas que la IA extrajo del audio.

|Endpoint|Función|Cuándo se utiliza|
|---|---|---|
|`GET /recordings/action-items`|Listar todas las tareas|Para el widget de "Tareas Pendientes" global del Dashboard (filtrando por usuario).|
|`PATCH /recordings/action-items/{item_id}`|Marcar tarea como hecha|Cuando el usuario completa una acción detectada por la IA desde la lista de tareas.|

Exportar a Hojas de cálculo

### 5. Organización y Etiquetas (Tags)

Para la categorización cruzada de notas.

|Endpoint|Función|Cuándo se utiliza|
|---|---|---|
|`GET /tags/`|Listar etiquetas|Para sugerir etiquetas existentes en el móvil mientras el usuario escribe.|
|`POST /recordings/{id}/tags/{tag_name}`|Vincular etiqueta|Para añadir una etiqueta (ej: #reunión) a una grabación específica.|
|`DELETE /recordings/{id}/tags/{tag_name}`|Desvincular etiqueta|Para quitar una categoría errónea de una nota.|

Exportar a Hojas de cálculo

### Resumen de implementación

Con esta estructura, cubres tanto el **flujo automático** (subida y análisis disparado por el móvil) como el **flujo manual**(gestión de etiquetas y tareas desde la web). Tu siguiente paso será implementar la lógica de estos métodos en `app/api/routes/recor.py` y las funciones correspondientes en `app/crud/crud_recor.py`.
## Versión 0 (incompleto)
```python
from fastapi import FastAPI, File, UploadFile, Form, HTTPException, Depends
from pydantic import BaseModel, UUID4
from typing import List, Optional
from datetime import datetime
from enum import Enum

app = FastAPI(
    title="JD-Recor API",
    description="API para la gestión de grabaciones de audio e IA del ecosistema JDWorkspace",
    version="1.0.0"
)

# --- ENUMS Y SCHEMAS (Pydantic) ---

class RecordingStatus(str, Enum):
    procesando = "procesando"
    completada = "completada"
    error = "error"

class TranscriptionMethod(str, Enum):
    local_mobile = "local_mobile"
    server_cloud = "server_cloud"

class AIInsights(BaseModel):
    full_transcription: str
    summary: Optional[str]
    sentiment: Optional[str]
    action_items: List[dict] # Lista de objetos con descripción y prioridad

class RecordingMetadata(BaseModel):
    transcription_method: TranscriptionMethod
    model_name: str
    device_id: str = "Samsung-S23-Ultra"
    latitude: Optional[float]
    longitude: Optional[float]

# --- ENDPOINTS ---

@app.post("/recordings", response_model=dict, tags=["Captura"])
async def upload_recording(
    audio_file: UploadFile = File(...),
    id: UUID4 = Form(...), # El ID se genera en el móvil para asegurar consistencia
    user_id: UUID4 = Form(...),
    title: str = Form(...),
    start_time: datetime = Form(...),
    end_time: datetime = Form(...),
    duration_ms: int = Form(...),
    local_transcription: Optional[str] = Form(None),
    processing_mode: TranscriptionMethod = Form(TranscriptionMethod.server_cloud)
):
    """
    Endpoint principal de subida. 
    Maneja la recepción inicial del binario y el inicio del procesamiento.
    """
    return {
        "id": id,
        "status": "procesando" if not local_transcription else "completada",
        "message": "Registro y archivo recibidos."
    }

@app.get("/recordings/{recording_id}", tags=["Consulta"])
async def get_recording_detail(recording_id: UUID4):
    """
    Consulta el detalle completo de una grabación. 
    Utilizado por el Dashboard Web para renderizar la UI.
    """
    return {"id": recording_id, "data": "..."}

@app.patch("/recordings/{recording_id}/sync-local", tags=["Sincronización"])
async def sync_local_transcription(
    recording_id: UUID4, 
    local_transcription: str,
    model_name: str
):
    """
    Sincroniza una transcripción generada localmente después de que el audio ya fue subido.
    """
    return {"status": "updated", "recording_id": recording_id}

@app.post("/recordings/{recording_id}/process-cloud", tags=["IA"])
async def trigger_cloud_processing(recording_id: UUID4):
    """
    Fuerza el re-procesamiento de una nota en la nube (ej: desde la Web).
    """
    return {"status": "processing_started"}

@app.get("/dashboard", tags=["Consulta"])
async def get_dashboard(
    user_id: UUID4, 
    category: Optional[str] = None, 
    search: Optional[str] = None
):
    """
    Listado principal de grabaciones con soporte para búsqueda.
    """
    return {"results": []}

# =============================================================================
# REGLAS DE NEGOCIO Y GESTIÓN DE CASOS DE BORDE (DOCUMENTACIÓN TÉCNICA)
# =============================================================================

"""
1. IDENTIDAD Y CONSISTENCIA (MÓVIL-SERVIDOR)
--------------------------------------------
- El UUID es generado por la App Móvil (S23 Ultra). 
- Si la subida se interrumpe, el móvil reintentará con el MISMO ID.
- El Servidor implementa lógica de "upsert" (si el ID existe, actualiza el registro 
  en lugar de duplicarlo).

2. CASO: "LATE ARRIVAL" (Sincronización Tardía)
-----------------------------------------------
Escenario: El móvil sube el audio rápido (4G), pero tarda 1 minuto más en procesar 
la transcripción local por falta de potencia momentánea.
- El servidor recibe el audio y puede empezar su propia transcripción (Cloud).
- Cuando el móvil termina su tarea local, llama a /sync-local.
- REGLA DE JERARQUÍA: Si el servidor ya generó una transcripción Cloud, esta se 
  mantiene como 'principal' por su mayor precisión, pero la transcripción local 
  se guarda en 'extra_metadata' para fines de auditoría del usuario.

3. CASO: PETICIÓN DESDE WEB SIN AUDIO SUBIDO
--------------------------------------------
Escenario: El usuario abre el Dashboard Web mientras el móvil está subiendo un audio.
- Si el registro existe en BD pero el archivo en S3 aún no está disponible:
  * La API devuelve status 'procesando'.
  * El Dashboard Web muestra un 'skeleton' o estado de carga.
  * No se permite reproducir el audio hasta que el checksum en BD coincida con el de S3.

4. CASO: SOBREESCRITURA DE DATOS
--------------------------------
- Si la Web edita el título de una nota mientras el móvil intenta sincronizar, 
  prevalece la acción más reciente (basada en el campo 'updated_at').
- La API rechazará actualizaciones si el registro está bloqueado por un proceso 
  crítico de IA.

5. GESTIÓN DE ERRORES DE IA
---------------------------
- Si el motor Cloud falla (ej: Gemini offline), el sistema intentará usar la 
  transcripción local enviada por el móvil (si existe).
- Si no hay ninguna, la nota queda en estado 'error' y se permite al usuario 
  "Reintentar Procesamiento" desde el Dashboard Web.

6. SEGURIDAD Y PRIVACIDAD
-------------------------
- Cada llamada debe verificar la propiedad del recurso: `user_id` del token JWT 
  debe ser igual al `user_id` de la tabla `recordings`.
- El acceso a las rutas de S3 es mediante URLs firmadas de corta duración (15 min).
"""
```
