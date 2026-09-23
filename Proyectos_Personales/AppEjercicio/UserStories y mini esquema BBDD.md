### **Contexto del Proyecto: Performance OS**

**1. Definición del Producto:** Performance OS es un **Dashboard de Alto Rendimiento** diseñado para atletas con perfil técnico (ingeniería/software) que requieren una gestión del entrenamiento basada estrictamente en datos. No es una app de "fitness social", sino una **herramienta de optimización biomecánica y fisiológica**.

**2. El Usuario (Persona):** Un arquitecto de software y ex-ciclista de élite (XCO). Tiene un motor cardiovascular de alto nivel (VO2máx elevado) pero un chasis (músculos/articulaciones) desadaptado por el sedentarismo laboral. El usuario entiende de métricas (FC, Ritmos, Cadencia, RPE) y busca eficiencia mecánica.

**3. Propuesta de Valor para el Frontend:**

- **Visualización de Datos Crudos:** Integración de gráficas complejas que cruzan telemetría de carrera (archivos CSV de Polar/Garmin) con métricas de fuerza.
    
- **Gestión Dinámica de KPIs:** El sistema permite al usuario definir sus propias "Key Metrics" (ej. perímetros musculares, marcas personales de fuerza, cadencia media).
    
- **Interfaz de Entrenamiento Contextual:** Un calendario que no solo muestra tareas, sino que permite un feedback bidireccional mediante un chat inteligente que tiene "memoria" de las sesiones anteriores.
### Módulo 1: Perfil y Biometría (The Baseline)

- **F01:** Registro de Parámetros Iniciales (Benchmarks de Fuerza).
    
- **F02:** Galería Evolutiva de Fotos (Ayunas/Estado Frío).
    
- **F03:** Log de Medidas Antropométricas (Cinta métrica).
    
- **F04:** Historial Clínico y Deportivo (Antecedentes XCO, lesiones).
    

### Módulo 2: Planificación (The Architecture)

- **F05:** Visualizador de Macrociclo y Mesociclos (Vista de pájaro).
    
- **F06:** Calendario Semanal Detallado (Microciclo).
    
- **F07:** Gestor de Objetivos (Smart Goals: ej. 1h 30m en Media Maratón).
    
- **F08:** Repositorio de Ejercicios y Progresiones (Vídeos/Gifs de técnica).
    
- **F09:** Asignación de Zonas de Entrenamiento (FC y Ritmos).
    

### Módulo 3: Ejecución y Feedback (The Log)

- **F10:** Formulario de Feedback Post-Entreno (RPE, Sensaciones, Agujetas).
    
- **F11:** Sincronización/Subida de Archivos de Telemetría (CSV, FIT, TCX).
    
- **F12:** Registro de Carga Semanal Percibida (TL = Duración x RPE).
    
- **F13:** Marcado de Sesiones Completadas/Fallidas.
    

### Módulo 4: Análisis de Datos (The Insights)

- **F14:** Gráfica de Eficiencia Aeróbica (Relación Ritmo/FC).
    
- **F15:** Seguimiento de Volumen de Fuerza (Carga Total Acumulada).
    
- **F16:** Calculadora de Desacoplamiento Cardíaco (Cardiac Drift).
    
- **F17:** Monitor de Ratio Cintura/Pecho (Recomposición Corporal).
    
- **F18:** Análisis de Cadencia Media (Corrección del "Bug" de los 145 ppm).
    

### Módulo 5: Comunicación y Soporte (The Hub)

- **F19:** Chat Directo Entrenador-Atleta.
    
- **F20:** Sistema de Notificaciones de Ajuste de Plan (Alertas de cambios).
    
- **F21:** Módulo de Notas del Entrenador (Análisis de los CSVs).

- **F21 - Chat de Hilo Único con Contexto de Sesión:** Capacidad de iniciar conversaciones asociadas a un `session_id` específico (ej. "¿Por qué me dolía la axila en este entreno?").
    
- **F22 - Tagging Automático de Consultas:** Clasificación de mensajes por categorías (Técnica, Nutrición, Salud, Planificación) para que el agente recupere contexto rápido.
    
- **F23 - Historial de Feedback Integrado:** Interfaz donde el chat muestra pequeñas "cards" de los entrenamientos referenciados en la conversación.
    
- **F24 - Extracción de Insights de Voz/Texto:** Registro de notas rápidas que la IA procesa para actualizar variables de estado (ej. "Hoy me siento muy cansado" -> Actualiza el `fatigue_score`).
    

### Módulo 6: Estilo de Vida (The External Variables)

- **F22:** Registro de Calidad de Sueño y Estrés Laboral (Arquitecto sentado).
    
- **F23:** Contador de "Unidades de Cerveza" (Viernes de recompensa).
    
- **F24:** Log de Paseos/Actividad con Nico (Descanso Activo).
- **F25 - Almacén de Long-term Memory:** Tabla de "Lecciones Aprendidas" del usuario (ej. "A David no le gusta el gimnasio", "Bebe cerveza los viernes").
    
- **F26 - Vector Store de Sesiones:** (Opcional en Supabase con `pgvector`) Para que la IA busque sesiones similares en el pasado y compare el rendimiento.


### Propuesta de Esquema de Base de Datos (Estructura CRUD dinámica)

Para que los objetivos y medidas sean dinámicos, te sugiero esta arquitectura en Postgres:

1. **`metrics_definition`**: Define qué se mide (id, nombre, unidad, tipo: 'fuerza', 'biometría', 'running').
    
2. **`user_metrics`**: El valor real (user_id, metric_id, valor, timestamp). _Aquí irían tus fotos y medidas de mañana._
    
3. **`training_plans`**: Macrociclos y objetivos (dinámicos).
    
4. **`sessions`**: El bloque diario (ejercicios, series, reps, link al CSV).
    
5. **`chat_messages`**: (id, role, content, context_metadata). El `context_metadata` es un JSONB donde guardarás el `session_id` o el `objective_id` al que se refiere la charla.