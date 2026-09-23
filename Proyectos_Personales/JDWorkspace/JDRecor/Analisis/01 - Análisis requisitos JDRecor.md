Este documento constituye la especificación técnica y funcional completa para el ecosistema **JDWorkspace** y su primer módulo operativo, **JD-Recor**.

## I. Visión Estratégica: JDWorkspace

### 1. Propósito y Propuesta de Valor

**JDWorkspace** es un **Ecosistema de Vida Digital** unificado. No es una colección de apps aisladas, sino un sistema integrado donde la información fluye entre módulos.

### 2. Arquitectura de Software: Monolito Modular

Para evitar la sobrecarga de gestionar múltiples servidores, adoptaremos una arquitectura de **Monolito Modular**:

- **Backend Único (FastAPI):** Un solo servicio que aloja diferentes módulos (`/recor`, `/assets`, `/auth`). Comparten la capa de seguridad y utilidades.
    
- **Base de Datos Única (PostgreSQL):** Una sola instancia organizada por **Esquemas Lógicos**:
    
    - `jd_core`: Usuarios, sesiones, permisos y configuración global.
        
    - `jd_recor`: Todo lo referente a grabaciones e IA de voz.
        
    - `jd_assets`: Gestión de activos (coche, hogar, finanzas).
        
- **Bus de Eventos Interno:** Permite que cuando `jd_recor` detecta una tarea, se notifique internamente al módulo correspondiente sin llamadas de red externas.
    

## II. Especificación Funcional: JD-Recor

### 1. Modos de Funcionamiento (Estrategia Dual)

La aplicación debe ser capaz de operar bajo dos paradigmas según el contexto del usuario:

|   |   |   |
|---|---|---|
|**Característica**|**Modo 1: IA Local**|**Modo 2: IA Servidor (Cloud)**|
|**Ubicación STT**|On-device (S23 Ultra NPU)|Backend (Whisper/Google API)|
|**Privacidad**|Máxima (procesado en el chip)|Estándar (encriptado en tránsito)|
|**Conectividad**|Funciona 100% Offline|Requiere Wi-Fi/5G|
|**Análisis LLM**|Resumen básico local|Análisis profundo con Gemini 1.5 Pro|

### 2. Casos de Uso a FUTURO

- **Captura Manos Libres:** Integración con asistentes externos (Fase futura).
    
- **Transcripción en Tiempo Real:** Visualización del texto mientras se graba.
    
- **Enriquecimiento Automático:** El sistema detecta "entidades" y sugiere crear un vínculo en otros módulos del workspace.
    

## III. Modelo de Datos y Entidades (JD-Recor)

### 1. Entidad: Grabación de audio (`recordings`)

_(Ver esquema SQL detallado para nombres técnicos exactos)_

- **ID (UUID):** Identificador global generado en el móvil.
    
- **Título:** Obligatorio (Manual o Auto).
    
- **Estado:** `procesando`, `completada`, `error`.
    
- **Tiempos:** Inicio (enviado por móvil), Fin, Duración (ms).
    
- **Método:** `transcrito_con` (Local/Server) y `modelo_utilizado`.
    

### 2. Entidad: AI Insights (`ai_insights`)

- **Transcripción:** Texto completo.
    
- **Resumen:** Síntesis generada por LLM.
    
- **Action Items:** Tareas detectadas.
    

## IV. Requerimientos Técnicos Unificados

### Infraestructura Común

- **Lenguaje:** Python (FastAPI).
    
- **Auth:** OAuth2 con JWT (Gestionado por el módulo `jd_core`).
    
- **Storage:** S3 Compatible (Bucket único con carpetas por módulo: `/recor/audios/`, `/assets/docs/`).
    

## V. Roadmap de Desarrollo Refinado

1. **Fase 1 (JD-Core):** Setup de FastAPI y Postgres con esquemas. Implementación de usuarios/auth.
    
2. **Fase 2 (JD-Recor Backend):** API de grabaciones, integración con S3 y Pipeline de IA (Cloud).
    
3. **Fase 3 (JD-Recor Mobile):** App Flutter (Grabación + Subida + STT Local).
    
4. **Fase 4 (Interconectividad):** Creación del primer vínculo (`jd_links`) entre una nota y un objeto simulado.
    
5. **Fase 5 (Dashboard Web):** UI centralizada en React para visualizar todo el ecosistema.