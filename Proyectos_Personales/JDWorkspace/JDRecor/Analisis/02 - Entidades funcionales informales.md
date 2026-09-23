Este documento detalla los datos que necesitamos capturar y almacenar para que el sistema sea robusto, auditable y capaz de integrarse con el resto del JDWorkspace.

## 1. Entidad: Grabación (La "Nota")

Esta es la entidad principal. He refinado tu lista y añadido campos de control de estado y contexto.

- **ID Único (UUID):** Identificador global para evitar colisiones entre móvil y servidor.
    
- **Título:** Definido por el usuario o generado automáticamente por la IA (ej: "Nota sobre Python...").
    
- **Estado de la Nota:** `grabando`, `pendiente_subida`, `procesando_ia`, `completada`, `error`.
    
- **Tiempos y Duración:**
    
    - **Fecha/Hora Inicio:** ISO 8601 (incluyendo zona horaria).
        
    - **Fecha/Hora Fin:** Para calcular pausas reales.
        
    - **Duración Total:** En milisegundos (más preciso para el reproductor).
        
- **Categorización y Taxonomía:**
    
    - **Categoría Principal:** (Trabajo, Personal, Gym, Coche).
        
    - **Etiquetas (Tags):** Array de strings (ej: `["itv", "presupuesto", "importante"]`).
        
- **Metadatos de IA y Transcripción:**
    
    - **Método de Transcripción:** `local_whisper_mobile`, `google_cloud_stt`, `openai_whisper_api`.
        
    - **Modelo de IA utilizado:** `gemini-1.5-flash`, `local-llama-3`, etc.
        
    - **Confianza de Transcripción:** Un valor de 0 a 1 (útil para saber si la IA "duda" de lo que escuchó).
        
    - **Tiempo de Procesamiento:** Cuánto tardó la IA en responder (para métricas de rendimiento).
        
- **Almacenamiento y Rutas:**
    
    - **Ruta Local (Móvil):** Path en el almacenamiento interno del S23.
        
    - **Ruta Remota (S3):** URL o Key del objeto en el servidor.
        
    - **Peso del Archivo:** En bytes (para control de cuotas de almacenamiento).
        
    - **Checksum/Hash (MD5/SHA):** Para verificar que el archivo no se corrompió al subirlo.
        
- **Contexto Físico (Opcional pero recomendado):**
    
    - **Geolocalización:** Latitud/Longitud (¿Dónde estaba cuando grabé esto?).
        
    - **Dispositivo:** Modelo (S23 Ultra) y versión de la app.
        

## 2. Entidad: Resultado de Inteligencia Artificial (AI_Insights)

Aunque podría estar dentro de la "Nota", es mejor verla como una entidad que "enriquece" la grabación.

- **ID de Grabación Relacionada:** Vínculo con la nota original.
    
- **Transcripción Completa:** El texto bruto.
    
- **Resumen Ejecutivo:** Un párrafo corto generado por Gemini.
    
- **Lista de Tareas (Action Items):** Objetos con `descripción` y `estado` (pendiente/hecho).
    
- **Sentimiento:** (Positivo, Negativo, Neutral, Urgente).
    
- **Entidades Detectadas:** Nombres de personas, marcas, o fechas mencionadas en el audio.
    

## 3. Entidad: Vínculo de Integración (JD-Link)

Esta es la entidad "pegamento" que conecta esta nota con el resto de tu Workspace.

- **ID de Origen:** El ID de la nota.
    
- **Módulo Destino:** A qué app de tu workspace se conecta (`JD-Assets-Coche`, `JD-Calendar`, `JD-Tasks`).
    
- **ID Externo:** El ID del objeto en la otra aplicación (ej: el ID de tu coche en la tabla de vehículos).
    
- **Tipo de Relación:** `recordatorio_mantenimiento`, `evidencia_gasto`, `minuta_reunion`.
    

## 4. Entidad: Usuario y Preferencias (User_Profile)

Para que el sistema sepa cómo comportarse contigo.

- **ID de Usuario:** Único en JDWorkspace.
    
- **Modo de IA Preferido:** `siempre_local`, `siempre_nube`, `smart` (usa nube si hay Wi-Fi).
    
- **Idioma por Defecto:** Para ayudar al motor de STT.
    
- **Configuración de Auto-Título:** ¿Quieres que la IA le ponga título sola? (Boolean).
    
- **Cuota de Almacenamiento:** Límite de GB contratados/disponibles.
    

## 5. Entidad: Log de Procesamiento (Auditoría)

Crucial para cuando algo falla (ej: "la IA no me resumió la nota").

- **Timestamp:** Cuándo ocurrió el evento.
    
- **Nivel:** `INFO`, `WARNING`, `ERROR`.
    
- **Mensaje:** "Fallo de conexión con Gemini API", "Reintentando subida...".
    
- **Intento:** Número de reintento (1, 2, 3...).