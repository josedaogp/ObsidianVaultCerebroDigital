## 1. Conexión a PostgreSQL

```
┌───────────────────┐
│ Configuración /   │
│ Variables de entorno │
└─────────┬─────────┘
          │
          ▼
┌───────────────────┐       ┌───────────────┐
│ Driver / Dialect  │──────▶│  Motor/Engine │
│ (psycopg2 / asyncpg) │    │ (create_engine /
│                     │    │  create_async_engine) │
└─────────┬─────────┘       └───────┬───────┘
          │                         │
          ▼                         │
┌───────────────────┐               │
│ Pool de conexiones│◀──────────────┘
└───────────────────┘

```

1. **Driver / Dialect**
    
    - Es la librería que sabe “hablar” nativo con PostgreSQL.
        
    - `psycopg2` (bloqueante) vs. `asyncpg` (no bloqueante).
        
    - Se instala con `pip` y aporta el protocolo de red, autenticación, envío/recepción de paquetes.
        
2. **Motor / Engine**
    
    - En SQLAlchemy (o SQLModel), el “Engine” encapsula la URL de la BBDD y la configuración de pool.
        
    - Cuando tu aplicación arranca, creas el Engine una sola vez:
        
        text
        
        CopiarEditar
        
        `Engine = create_engine( “postgresql://…” )`
        
    - El Engine administra internamente un **pool** de conexiones vivas para reutilizarlas, optimizando el rendimiento.
        
3. **Pool de conexiones**
    
    - Conjunto de conexiones abiertas y listas para usar.
        
    - Evita abrir y cerrar TCP/SSL con cada petición.
        
    - Parámetros configurables: tamaño mínimo/máximo, tiempo de vida, reciclaje, etc.
        
4. **Sesión o Contexto de Ejecución**
    
    - **Síncrono**: `Session` es una fábrica (`sessionmaker`) que, bajo demanda, toma una conexión del pool y la envuelve para hacer transacciones.
        
    - **Asíncrono**: se usa un dependency de FastAPI que, al comienzo de la petición, crea un objeto `AsyncSession` y, al terminar, hace `commit()` o `rollback()` y libera la conexión.
        

---

## 2. Modelado de la tabla (ORM)

```
┌──────────────────────┐
│   Clases Python      │
│  (Modelos ORM)       │
└─────────┬────────────┘
          │  heredan de
          ▼
┌──────────────────────┐
│   Base Declarativa   │
│ (declarative_base()) │
└─────────┬────────────┘
          │ metadata
          ▼
┌──────────────────────┐
│   Metadata / DDL     │
│ (CREATE TABLE, etc.) │
└──────────────────────┘

```

1. **Base Declarativa**
    
    - Objeto central que recopila todos los modelos.
        
    - Guarda en `Base.metadata` la información de tablas y columnas.
        
2. **Definición de Clases (Tablas)**
    
    - Cada clase Python = una tabla en la BBDD.
        
    - Atributos de clase = columnas (tipos, constrainsts).
        
    - Ejemplo de intención:
        
        text
        
        CopiarEditar
        
        `class Usuario(Base):     __tablename__ = "usuarios"     id = Column(Integer, primary_key=True)     nombre = Column(String)`
        
3. **Metadata**
    
    - Conjunto de objetos que describen la estructura de la BBDD.
        
    - Se usa para generar DDL (Data Definition Language) y, en desarrollo, hacer `Base.metadata.create_all(engine)`.
        
4. **Migraciones**
    
    - En producción NUNCA se usa `create_all`.
        
    - Se gestiona con Alembic:
        
        1. Se “detectan” cambios en las clases (nueva columna, tabla, etc.).
            
        2. Se genera un script de migración versionado.
            
        3. Al desplegar, se ejecutan las migraciones para actualizar la BBDD sin pérdida de datos.
            

---

## 3. Consulta y ciclo de vida en FastAPI
```
┌───────────────────────────┐
│ Cliente HTTP (cURL, JS…)  │
└───────────┬───────────────┘
            │
            ▼
┌───────────────────────────┐
│  FastAPI Endpoint         │
│  (ruta / dependencia DB)  │
└───────────┬───────────────┘
            │
            ▼
┌───────────────────────────┐
│ Sesión/AsyncSession       │
│ (inyección automática)    │
└───────────┬───────────────┘
            │ query(modelos)…
            ▼
┌───────────────────────────┐
│  Engine / Conexión real   │
│  └─ ejecuta SQL           │
└───────────┬───────────────┘
            │ recibe filas…
            ▼
┌───────────────────────────┐
│ Instancias de Modelos ORM │
│  (objetos Python)         │
└───────────┬───────────────┘
            │ validación /   \
            │ serialización  \
            ▼                ▼
┌────────────────┐    ┌─────────────────┐
│Pydantic Schema │    │  JSON Response  │
│  (output)      │    │  al cliente     │
└────────────────┘    └─────────────────┘

```

1. **Dependency Injection**
    
    - Definimos una función `get_db()` que abre la sesión antes de la petición y la cierra después.
        
    - FastAPI la inyecta en tu función de endpoint como parámetro.
        
2. **Construcción de la Consulta**
    
    - Con el ORM: usas métodos de la sesión:
        
        - `session.query(Modelo).filter(…)`
            
        - ó en SQLAlchemy 1.4+: `select(Modelo).where(…)` + `session.execute()`.
            
    - En Async: `await session.execute(select(...))`.
        
3. **Ejecución**
    
    - El Engine traduce el objeto Python a SQL estándar, lo envía al servidor, obtiene filas, las convierte en tuplas/objetos.
        
4. **Mapeo a Objetos**
    
    - Cada fila devuelve una instancia de tu clase ORM, con atributos ya poblados.
        
5. **Serialización con Pydantic**
    
    - Para devolver JSON, defines un schema Pydantic (puedes usar SQLModel que integra ambos).
        
    - Valida tipos y estructuras y genera el diccionario que FastAPI retorna al cliente.
        
6. **Cierre de contexto**
    
    - Tras `commit()` o `rollback()`, la sesión libera la conexión al pool.
        

---

### Resumen de responsabilidades

- **Driver / Engine**: gestión baja de red, pool, conexiones.
    
- **ORM (clases + metadata)**: representación de la BBDD en objetos Python, definición de la estructura.
    
- **Sesión / AsyncSession**: contexto transaccional donde se encierran todas las operaciones de lectura/escritura.
    
- **FastAPI + Dependencies**: orquestan inyección de sesión, rutas, validación de request/response.
    
- **Pydantic**: validación y serialización de datos de entrada y salida.
    
- **Migraciones (Alembic)**: control de versiones de esquema en producción.
    

Con esta arquitectura podrás ver claramente cómo fluye una petición desde que llega el HTTP hasta que sale el JSON, pasando por la BBDD y volviendo convertida en objetos Python. Cuando quieras, pasamos a ver ejemplos de configuración y código concreto para cada uno de estos componentes.

---
## 4. Esquema de directorios y ficheros
```
my_project/
├── app/
│   ├── main.py               # Punto de entrada de la aplicación (FastAPI instance)
│   ├── core/                 # Configuración general y constantes
│   │   ├── config.py         # Lectura de variables de entorno (DB URL, puertos…)
│   │   └── security.py       # Claves, OAuth2, hashing de contraseñas…
│   ├── db/                   # Todo lo relacionado con la BBDD
│   │   ├── base.py           # Base declarativa de SQLAlchemy (declarative_base)
│   │   ├── session.py        # engine, SessionLocal / AsyncSession factory
│   │   └── init_db.py        # Funciones de arranque para crear tablas, cargar datos iniciales
│   ├── models/               # Clases ORM (SQLAlchemy / SQLModel)
│   │   └── user.py, item.py… # Un fichero por modelo o grupo pequeño de modelos
│   ├── schemas/              # Pydantic models (requests & responses)
│   │   └── user.py, item.py…
│   ├── crud/                 # Funciones “CRUD” que aíslan la lógica de acceso a datos
│   │   └── user.py, item.py…
│   ├── api/                  # Definición de rutas y versionado
│   │   ├── deps.py           # Dependencias reutilizables (ej: get_db)
│   │   ├── v1/               # Versionado de la API
│   │   │   ├── routers/      # Cada conjunto de endpoints en su router
│   │   │   │   ├── users.py
│   │   │   │   └── items.py
│   │   │   └── __init__.py
│   │   └── __init__.py
│   └── utils/                # Utilidades generales
│       ├── pagination.py
│       └── mailing.py
│
├── alembic/                  # Migraciones de base de datos (si usas Alembic)
│   ├── versions/
│   └── env.py
│
├── tests/                    # Tests unitarios / de integración
│   ├── conftest.py           # Fixtures de pytest (p. ej. sesión DB de prueba)
│   └── test_users.py
│
├── .env                      # Variables de entorno (DB URL, secrets…)
├── requirements.txt          # Dependencias
├── Dockerfile                # Configuración de contenedor
├── docker-compose.yml        # (opcional) para levantar BBDD + app
├── README.md
└── pyproject.toml / setup.py # Metadatos del paquete (si lo distribuyes)

```