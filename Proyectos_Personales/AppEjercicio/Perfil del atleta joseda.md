# PERFIL DEL ATLETA: JOSE DAVID ORTIZ

## 1. DIMENSIÓN BIOGRÁFICA Y PSICOGRÁFICA

- **Identidad Profesional:** Ingeniero de Software / Arquitecto de Software. Mentalidad _data-driven_, obsesión por la optimización de procesos, lógica cuantitativa y análisis de telemetría.
    
- **Historial Deportivo de Élite:** Ex-ciclista de montaña de alta competición (XCO) desde los 14 hasta los 21 años. Campeón provincial de Huelva y Top 5-6 de Andalucía.
    
- **Capacidad Cardiovascular (Motor):** Posee un "motor" de élite residual. VO2máx estimado históricamente alto (Polar estima 77 actualmente). Umbral anaeróbico histórico en torno a 190 ppm y máximas de 200 ppm. Recuperación cardíaca excepcionalmente rápida.
    
- **Situación Actual:** Reentrada deportiva tras periodo de sedentarismo laboral (trabajo sentado). Presenta desadaptación funcional: el "chasis" (músculos y articulaciones) no está a la altura del "motor" (corazón/pulmones).
    
- **Limitaciones y Preferencias:** Odia el gimnasio convencional. Prefiere entrenar con peso corporal o material mínimo en casa. Tolera metodología Tabata. Usuario recreativo de pádel (2 partidos/semana).
    
- **Hábitos de Estilo de Vida:** Consumo regular de alcohol (6-8 cervezas los viernes) como variable no negociable. Objetivo de recomposición corporal (eliminar grasa abdominal "cervecera" y ganar hipertrofia en tren superior).
    

## 2. ESTADO FÍSICO INICIAL (LÍNEA BASE - DÍA 0)

- **Morfología:** Ectomorfo ("flacucho"), 1.74m, 66kg.
    
- **Puntos Críticos:** Debilidad en cadena anterior (pectoral/tríceps) y falta de adaptación al impacto (running). Acortamiento de psoas e inhibición de glúteo por ergonomía laboral.
    
- **Objetivo Maestro:** Media Maratón (21km) en < 1h 30min (Ritmo 4:15 min/km) y tonificación visible de pectorales, bíceps y abdominales.
    

---

# BITÁCORA DE PROGRESO: SPRINT DE REENTRADA (72 HORAS)

## DÍA 1: TEST DE ESTRÉS CARDIOVASCULAR (RUNNING)

- **Actividad:** Primera sesión de carrera a pie tras largo periodo de inactividad.
    
- **Protocolo:** Entrenamiento por sensaciones intercalando correr y andar rápido.
    
- **Métricas Clave:** Ritmos entre 5:00 y 6:00 min/km. Picos de intensidad con "jadeo" fuerte.
    
- **Feedback del Usuario:** Notó "la vejez" (dolores de espalda repentinos, fatiga muscular prematura).
    
- **Análisis del Entrenador:** Se detecta que el sistema cardiovascular puede empujar mucho más de lo que las piernas soportan. Riesgo de lesión por impacto.
    

## DÍA 2: AUDITORÍA DE TELEMETRÍA Y BIOMECÁNICA (TRAIL)

- **Actividad:** Sesión de Running/Trail con sensor de banda de pecho (Polar).
    
- **Datos de Sesión:** 8.22 km | 1h 21min | FC Media 125 bpm | FC Máx 181 bpm | Ritmo medio 9:51 min/km (incluyendo paradas/paseo con perro).
    
- **Hitos Técnicos:** * **Cadencia Crítica:** 145 ppm (pasos por minuto). Diagnóstico de _overstriding_ (zancada muy larga y traumática).
    
    - **Eficiencia:** Picos de 3:46 min/km en _strides_.
        
    - **Desacoplamiento Cardíaco:** Ligera deriva tras el minuto 55, indicando fatiga neuromuscular.
        
- **Estado Físico:** Aparición de agujetas intensas (DOMS) en cuádriceps y gemelos. Sensación de "piernas de hormigón".
    
- **Nutrición:** Ajuste de merienda (paso de 2 manzanas a tostadas con pavo/huevo para síntesis proteica).
    

## DÍA 3: BENCHMARK DE FUERZA Y RECUPERACIÓN ACTIVA

- **Actividad 1:** Paseo en bici con perro (Nico) en Z1/Z2 para limpieza metabólica.
    
- **Actividad 2:** Test de Benchmarks de Fuerza (Baseline).
    
- **Resultados de Fuerza (Data):**
    
    - **Test A (Flexiones):** 12 repeticiones (Fallo técnico por debilidad en tríceps/serrato).
        
    - **Test B (Plancha Abdominal):** 1 min 29 seg (Core sólido, buena resistencia isométrica).
        
- **Entrenamiento Realizado:** 2 rondas de circuito antagónico (Flexiones diamante con rodillas, Planchas laterales, Flexiones arqueras, Hollow Body).
    
- **Feedback del Usuario:** Frustración por falta de potencia en tren superior. Dolor en zona de axila/serrato (estabilizadores escápulares despertando). Sensación de "brazos de plomo".
    
- **Logística:** Compra de set de mancuernas ajustables (30kg) en AliExpress (40€) para iniciar fase de hipertrofia.
    

---

# CONFIGURACIÓN DEL SISTEMA PARA EL AGENTE (CONTEXTO IA)

### REGLAS DE NEGOCIO DEL ENTRENAMIENTO

1. **Prioridad Estructural:** El volumen de carrera debe subir de forma muy gradual para compensar la baja cadencia (145 ppm) hasta que se corrija técnicamente a +170 ppm.
    
2. **Enfoque de Hipertrofia:** El usuario busca "estética de flacucho fibrado". Foco en Pectoral, Bíceps y Abdomen.
    
3. **Gestión de Carga:** El usuario saca al perro 1h diaria. Esta actividad es una variable fija que debe computar como "Descanso Activo" o "Calentamiento".
    
4. **Modelo de Datos:** El agente debe priorizar el análisis de CSVs de Polar Flow para ajustar los ritmos de carrera basados en la FC Real y no en ritmos teóricos.
    

### VARIABLES DINÁMICAS A MONITORIZAR (KPIs)

- `ratio_cintura_pecho`: Indicador de éxito en recomposición corporal.
    
- `eficiencia_aerobica`: Relación Ritmo/FC (Mejora del motor).
    
- `cadencia_media`: Corrección del bug biomecánico.
    
- `volumen_fuerza_semanal`: Carga acumulada con las nuevas pesas de 30kg.
    

---

**Nota para el Agente:** David es un perfil altamente inteligente y técnico. No aceptará sugerencias genéricas. Todas las recomendaciones deben estar respaldadas por datos de sus propios entrenamientos o principios de fisiología del ejercicio. Odia perder el tiempo; si el descanso es largo o corto, debe haber una explicación técnica tras ello.