
Este documento describe la estructura de datos de la aplicación desde una perspectiva funcional, explicando qué información guardamos y por qué es relevante para el usuario.

## Tabla: Grabaciones (El Corazón de la Nota)

Es la ficha principal que identifica cada pensamiento o recordatorio guardado.

- **Identificador**: código # Referencia única para que el sistema no confunda esta nota con otra.
    
- **Usuario**: referencia # Indica a quién pertenece la nota dentro del JDWorkspace.
    
- **Título**: texto # Nombre descriptivo (puesto por ti o por la IA).
    
- **Estado**: lista de opciones # Indica si la nota se está grabando, si se está procesando en la nube o si ya está lista.
    
- **Categoría**: texto # El grupo principal (ej: Trabajo, Personal, Gimnasio).
    
- **Momento de Inicio**: fecha y hora # Cuándo pulsaste el botón de grabar.
    
- **Momento de Fin**: fecha y hora # Cuándo detuviste la grabación.
    
- **Duración Total**: tiempo # Cuánto duró el audio exactamente.
    

## Tabla: Archivos de Audio (El Almacén)

Datos específicos sobre el archivo físico para que nunca se pierda y siempre suene bien.

- **Ruta en la Nube**: dirección de internet # Dónde está guardado el audio para escucharlo en la web.
    
- **Ruta en el Móvil**: dirección local # Dónde está el archivo dentro de tu Samsung S23 Ultra.
    
- **Tamaño del Archivo**: peso # Cuánto espacio ocupa (en bytes).
    
- **Tipo de Audio**: formato # Si es un archivo .m4a, .wav o similar.
    
- **Firma de Seguridad**: código de verificación # Un código único para asegurar que el archivo no se ha corrompido al subirlo.
    

## Tabla: Detalles Técnicos y Contexto (El "Cómo" y "Dónde")

Información sobre el entorno y la tecnología utilizada durante la grabación.

- **Método de Transcripción**: opción # Si se usó la potencia del móvil (Local) o la de Google (Nube).
    
- **Cerebro Utilizado**: texto # El nombre de la IA que trabajó el audio (ej: Gemini 1.5, Whisper).
    
- **Tiempo de Espera**: tiempo # Cuánto tardó la IA en entregarte el resultado.
    
- **Nivel de Confianza**: porcentaje # Qué tan segura está la IA de que ha entendido bien el audio.
    
- **Dispositivo**: texto # El modelo del teléfono usado (S23 Ultra).
    
- **Ubicación GPS**: coordenadas # Dónde estabas físicamente cuando grabaste la nota.
    
- **Datos Extra**: baúl de información # Un espacio flexible para guardar cosas que se nos ocurran en el futuro.
    

## Tabla: Resultados de IA (El Contenido Útil)

Lo que la IA ha extraído y "entendido" de tus palabras.

- **Transcripción**: texto completo # Todo lo que dijiste, escrito palabra por palabra.
    
- **Resumen**: texto corto # Una síntesis de la nota para no tener que leerla entera.
    
- **Sentimiento**: texto # El tono detectado (ej: Urgente, Alegre, Preocupado).
    

## Tabla: Tareas Detectadas (Acciones a realizar)

Cosas que dijiste que tenías que hacer y que la IA ha convertido en una lista.

- **Descripción**: texto # Qué es lo que hay que hacer.
    
- **Prioridad**: nivel # Si es una tarea urgente o normal.
    
- **Estado**: sí/no # Si ya has completado la tarea.
    
- **Fecha Límite**: fecha # Cuándo debería estar terminada esta tarea.
    

## Tabla: Etiquetas (Organización)

Palabras clave para filtrar y encontrar notas rápidamente.

- **Nombre de Etiqueta**: texto # La palabra clave (ej: "Python", "ITV", "Ideas").
    

## Tabla: Enlaces del Workspace (Conectividad)

Lo que convierte esta nota en parte de un ecosistema más grande.

- **Módulo Destino**: nombre de aplicación # A qué otra app de tu workspace apunta (ej: App del Coche).
    
- **Objeto Destino**: identificador # El elemento exacto al que se refiere (ej: Tu coche específico).
    
- **Motivo del Enlace**: texto # Por qué están conectados (ej: "Esta nota es el presupuesto del taller").