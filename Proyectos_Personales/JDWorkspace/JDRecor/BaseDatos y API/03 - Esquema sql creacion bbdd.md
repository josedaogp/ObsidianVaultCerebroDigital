```sql
-- =============================================================================
-- JD-RECOR DATABASE SCHEMA (PostgreSQL)
-- Módulo de Grabación e Inteligencia de Voz para JDWorkspace.
-- Versión Refinada: Enfoque en Procesamiento y Sincronización.
-- =============================================================================

-- 0. Preparación del Entorno
CREATE SCHEMA IF NOT EXISTS jd_recor;
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

-- 1. Tipos Enumerados (Estados y Métodos)
DO $$ 
BEGIN
    -- recording_status: Se ha simplificado. 
    -- 'procesando': El audio se recibió y la IA está trabajando.
    -- 'completada': Todo el pipeline (STT + LLM) ha finalizado con éxito.
    -- 'error': Algo falló en la subida o en el análisis de IA.
    IF NOT EXISTS (SELECT 1 FROM pg_type WHERE typname = 'recording_status') THEN
        CREATE TYPE jd_recor.recording_status AS ENUM (
            'procesando', 'completada', 'error'
        );
    END IF;
    
    IF NOT EXISTS (SELECT 1 FROM pg_type WHERE typname = 'transcription_method') THEN
        CREATE TYPE jd_recor.transcription_method AS ENUM (
            'local_mobile', 'server_cloud'
        );
    END IF;
END $$;

-- 2. Tabla: recordings (Grabaciones)
-- Registro centralizado de las notas.
CREATE TABLE IF NOT EXISTS jd_recor.recordings (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id UUID NOT NULL, 
    title VARCHAR(255) NOT NULL, -- !! Cambio a NOT NULL. Obligatorio asignar título (manual o automático).
    status jd_recor.recording_status DEFAULT 'procesando' NOT NULL, -- !! Simplificado: Si entra en la BD, entra para procesarse.
    category VARCHAR(50) DEFAULT 'general',

    -- Tiempos y Duración
    -- !! Se elimina DEFAULT. La App Móvil DEBE enviar el timestamp real del evento.
    start_time TIMESTAMPTZ NOT NULL, 
    end_time TIMESTAMPTZ, -- Fecha y hora exacta del fin enviada por el móvil.
    duration_ms INTEGER DEFAULT 0, -- Duración exacta en milisegundos calculada en el cliente.
    
    -- Control de Auditoría del Registro (Tiempos del servidor)
    created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP, 
    updated_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP,

    CONSTRAINT check_duration_positive CHECK (duration_ms >= 0)
);

-- 3. Tabla: recording_files (Archivos y Almacenamiento)
CREATE TABLE IF NOT EXISTS jd_recor.recording_files (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    recording_id UUID NOT NULL REFERENCES jd_recor.recordings(id) ON DELETE CASCADE,
    
    s3_path TEXT, -- Ubicación en el almacenamiento remoto.
    local_path TEXT, -- Ruta original en el S23 Ultra (para debug y logs).
    file_size_bytes BIGINT,
    mime_type VARCHAR(100) DEFAULT 'audio/m4a',
    checksum_sha256 CHAR(64), -- Garantiza integridad tras la subida.
    
    UNIQUE(recording_id)
);

-- 4. Tabla: recording_metadata (Contexto y Hardware)
CREATE TABLE IF NOT EXISTS jd_recor.recording_metadata (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    recording_id UUID NOT NULL REFERENCES jd_recor.recordings(id) ON DELETE CASCADE,
    
    transcription_method jd_recor.transcription_method NOT NULL,
    model_name VARCHAR(100), 
    processing_time_ms INTEGER, 
    ai_confidence NUMERIC(3,2),
    
    device_id VARCHAR(100) DEFAULT 'Samsung-S23-Ultra',
    latitude NUMERIC(9,6),
    longitude NUMERIC(9,6),
    
    extra_metadata JSONB DEFAULT '{}', -- Datos de sensores adicionales.

    UNIQUE(recording_id)
);

-- 5. Tabla: ai_insights (Resultados de Inteligencia Artificial)
CREATE TABLE IF NOT EXISTS jd_recor.ai_insights (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    recording_id UUID NOT NULL REFERENCES jd_recor.recordings(id) ON DELETE CASCADE,
    
    full_transcription TEXT,
    summary TEXT,
    sentiment VARCHAR(50),
    
    search_vector tsvector, -- Para búsquedas tipo Google.

    UNIQUE(recording_id)
);

-- 6. Tabla: ai_action_items (Tareas Detectadas)
CREATE TABLE IF NOT EXISTS jd_recor.ai_action_items (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    recording_id UUID NOT NULL REFERENCES jd_recor.recordings(id) ON DELETE CASCADE,
    
    description TEXT NOT NULL,
    priority VARCHAR(20) DEFAULT 'media',
    is_completed BOOLEAN DEFAULT FALSE,
    due_date TIMESTAMPTZ,
    
    created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);

-- 7. Tablas de Etiquetas
CREATE TABLE IF NOT EXISTS jd_recor.tags (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    name VARCHAR(50) UNIQUE NOT NULL
);

CREATE TABLE IF NOT EXISTS jd_recor.recording_tags (
    recording_id UUID REFERENCES jd_recor.recordings(id) ON DELETE CASCADE,
    tag_id UUID REFERENCES jd_recor.tags(id) ON DELETE CASCADE,
    PRIMARY KEY (recording_id, tag_id)
);

-- 8. Tabla: jd_links (Integración con otros módulos)
CREATE TABLE IF NOT EXISTS jd_recor.jd_links (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    recording_id UUID NOT NULL REFERENCES jd_recor.recordings(id) ON DELETE CASCADE,
    
    target_module VARCHAR(50) NOT NULL, -- ej: 'jd-car-maintenance'
    target_id VARCHAR(100) NOT NULL,    -- ej: ID del vehículo
    link_type VARCHAR(50),
    
    created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP
);

-- 9. Índices de Optimización
CREATE INDEX idx_recordings_user_id ON jd_recor.recordings(user_id);
CREATE INDEX idx_recordings_status ON jd_recor.recordings(status);
CREATE INDEX idx_recordings_start_time ON jd_recor.recordings(start_time DESC);

-- Búsqueda de texto completo
CREATE INDEX idx_ai_insights_fts ON jd_recor.ai_insights USING GIN(search_vector);

-- 10. Triggers de Automatización
CREATE OR REPLACE FUNCTION jd_recor.update_modified_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = now();
    RETURN NEW;
END;
$$ language 'plpgsql';

CREATE TRIGGER update_recording_modtime
    BEFORE UPDATE ON jd_recor.recordings
    FOR EACH ROW
    EXECUTE PROCEDURE jd_recor.update_modified_column();

-- Actualización del vector de búsqueda
CREATE OR REPLACE FUNCTION jd_recor.update_search_vector()
RETURNS TRIGGER AS $$
BEGIN
    NEW.search_vector := setweight(to_tsvector('spanish', coalesce(NEW.full_transcription, '')), 'A') ||
                         setweight(to_tsvector('spanish', coalesce(NEW.summary, '')), 'B');
    RETURN NEW;
END;
$$ language 'plpgsql';

CREATE TRIGGER update_ai_insights_fts
    BEFORE INSERT OR UPDATE ON jd_recor.ai_insights
    FOR EACH ROW
    EXECUTE PROCEDURE jd_recor.update_search_vector();
```
