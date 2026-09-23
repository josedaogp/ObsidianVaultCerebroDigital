Vale, vamos a poner orden. Te explico **qué es este proyecto**, **qué piezas tiene**, **para qué sirve cada una**, **dónde está la base de datos**, **cómo se inicializan las tablas**, y **qué pasos exactos seguir** (con comandos) para montarlo desde cero y replicarlo en otros proyectos. Corto, claro y sin misterios.

---

# 1) ¿Qué es este proyecto?

Es una **plantilla** para arrancar APIs en tu servidor con:

- **FastAPI** (framework web)
    
- **PostgreSQL** (base de datos)
    
- **Autenticación** estilo “Supabase”: registro, login, verificación, reset password (con **fastapi-users**)
    
- **Migraciones** de esquema con **Alembic**
    
- **Docker Compose** para levantar **api + db + mailpit** en local
    
- **`src`-layout** (código en `src/`) para empaquetar/instalar limpio
    

La idea: clonas, cambias 4 cosas, levantas, y ya tienes **login funcionando** y **rutas protegidas**.

---

# 2) Las piezas y PARA QUÉ sirven

### Contenedores (Docker Compose)

- **`db`** → `postgres:16`  
    Guarda datos en el volumen `db_data` (persistente). Puerto expuesto: **5432**.
    
- **`api`** → tu app FastAPI  
    Arranca con Uvicorn y al iniciar ejecuta `alembic upgrade head` (aplica migraciones).
    
- **`mailpit`** → SMTP de desarrollo  
    Captura emails de verificación/reset en [http://localhost:8025](http://localhost:8025) (no necesitas servidor SMTP real en dev).
    

### Librerías clave (backend)

- **FastAPI**: framework para definir rutas, dependencias, etc.
    
- **Uvicorn**: servidor ASGI que ejecuta FastAPI.
    
- **SQLAlchemy 2**: ORM/conexión a PostgreSQL.
    
- **Alembic**: **versiona el esquema** de la BBDD (crear/editar tablas de forma reproducible).
    
- **psycopg2-binary** (o `psycopg[binary]`): driver de PostgreSQL para Python.
    
- **fastapi-users**: te da **registro/login/verify/reset**, sesión con **cookies JWT**, esquemas y rutas listas.
    
- **passlib[bcrypt]**: hashing seguro de contraseñas.
    
- **email-validator**: valida emails.
    
- **pydantic / pydantic-settings**: modelos y **config por `.env`** (SECRET_KEY, DATABASE_URL, etc.).
    

### Ficheros importantes

- **`pyproject.toml`**: manifiesto del proyecto (deps, extras `.[dev]`, configuración de setuptools, linters).
    
- **`Dockerfile`**: define la imagen de la API (instala deps, copia código).
    
- **`docker-compose.yml`**: orquesta `db`, `api`, `mailpit`.
    
- **`alembic.ini`**: configuración de Alembic (`script_location = src/app/db/migrations`).
    
- **`src/app/db/migrations/`**: migraciones de la BBDD (código Python con `upgrade()`/`downgrade()`).
    
- **`.env.example`** → copia a **`.env`** con tus valores.
    
- **`database/schema.sql`**: SQL para crear tablas base **por si quieres desbloquear rápido** (alternativa a migraciones en dev).
    

### Estructura de carpetas (resumen)

`src/   app/     main.py                # crea la app FastAPI, CORS y monta routers     api/v1/routes.py       # agrupa routers (/auth, /users, ...)     auth/                  # auth con fastapi-users       models.py            # User, OAuthAccount, RefreshToken, AuthEvent       schemas.py           # DTOs de usuario       deps.py              # backends y current_user       routes.py            # /auth/*     core/       config.py            # Settings via .env (SECRET_KEY, DB, ORIGINS…)       security.py          # utilidades JWT     db/       base.py              # Base = DeclarativeBase (metadatos del ORM)       session.py           # engine + SessionLocal + get_db()       migrations/         env.py             # Alembic: aquí importas MODELOS para autogenerate         versions/          # aquí se guardan las migraciones (archivos .py)     users/routes.py        # /users/me (ruta protegida) alembic.ini docker-compose.yml Dockerfile .env.example pyproject.toml README.md`

---

# 3) ¿Dónde está la base de datos y cómo se crean las tablas?

- En local, **PostgreSQL** corre en el contenedor **`db`** (puerto **5432**).  
    Credenciales por defecto (compose): `postgres/postgres`, DB `app`.
    
- **Las tablas NO se crean “mágicamente”**: se crean cuando **aplicas migraciones con Alembic**.
    

## ¿Qué es una migración?

Un **archivo Python** (en `src/app/db/migrations/versions/…`) que dice  
**qué crear/cambiar/borrar** en el esquema (ej. `CREATE TABLE users …`).  
Se aplica con:

`alembic upgrade head`

**Primer uso (migración inicial “init”)**:  
Alembic compara **tus modelos** con la **DB vacía** y genera un archivo  
que crea `users`, `oauth_accounts`, `refresh_tokens`, `auth_events`, etc.

_(Si vas con prisas en dev, puedes usar `database/schema.sql` para crear las tablas “a mano”. Pero lo bueno es aprender a usar migraciones: es como git para el esquema.)_

---

# 4) Montarlo desde cero (Windows / PowerShell)

## A) Preparación

1. **Instala y abre Docker Desktop** (WSL2 activo).
    
2. **Clona** tu repo plantilla y **ábrelo en VS Code**.
    
3. Copia el entorno:
    

`copy .env.example .env`

4. **.env** → pon valores reales:
    
    - `SECRET_KEY` (cadena aleatoria)
        
    - `DATABASE_URL=postgresql://postgres:postgres@db:5432/app` (apunta al servicio `db`)
        
    - `ORIGINS` como **JSON** (o usa el validador del código):
        
        `ORIGINS=["http://localhost:5173","http://localhost:3000"]`
        
5. Recomendado para dev: monta el repo dentro del contenedor (así las migraciones que generes **se guardan en tu host**).  
    En `docker-compose.yml`, dentro de `api`:
    
    `volumes:   - .:/app`
    
6. Asegura que existe la carpeta de versiones de Alembic (si no, créala para que se copie):
    

`mkdir src\app\db\migrations\versions 2>$null ni src\app\db\migrations\versions\.gitkeep 2>$null`

7. Comprueba estos dos archivos:
    
    - `alembic.ini` → `script_location = src/app/db/migrations`
        
    - `src/app/db/migrations/env.py` → **importa tus modelos**, así:
        
        `from app.db.base import Base from app.auth.models import User, OAuthAccount, RefreshToken, AuthEvent  # noqa target_metadata = Base.metadata`
        
        _(No importes modelos en `base.py` para evitar ciclos.)_
        

## B) Generar migración inicial (si el repo no la trae ya)

`docker compose build docker compose run --rm api alembic revision --autogenerate -m "init"`

- Se crea un archivo en `src/app/db/migrations/versions/…_init.py`.
    
- Abre ese archivo y confirma:
    
    `revision = "XXXXXXXXXXXX" down_revision = None   # ← SIN comillas # dentro de upgrade() debe haber op.create_table(...), índices, etc.`
    

## C) Aplicar migración y arrancar

`docker compose run --rm api alembic upgrade head docker compose up`

- API: [http://localhost:8000](http://localhost:8000)
    
- Docs: [http://localhost:8000/docs](http://localhost:8000/docs)
    
- Mailpit: [http://localhost:8025](http://localhost:8025)
    

## D) Verificar que hay tablas

`docker exec -it fastapi-login-db-1 psql -U postgres -d app -c "\dt"`

Deberías ver `users`, `oauth_accounts`, `refresh_tokens`, `auth_events`, etc.

## E) Probar login real

``# Registro curl -X POST http://localhost:8000/api/v1/auth/register `   -H "Content-Type: application/json" `   -d '{"email":"joseda@example.com","password":"Test12345","full_name":"Joseda"}'  # Login (guarda cookie) curl -i -c cookies.txt -X POST http://localhost:8000/api/v1/auth/jwt/login `   -H "Content-Type: application/x-www-form-urlencoded" `   -d "username=joseda@example.com&password=Test12345"  # Ruta protegida curl -b cookies.txt http://localhost:8000/api/v1/users/me``

---

# 5) ¿Qué hace CADA fichero del proyecto?

- **`src/app/main.py`**: crea la app FastAPI, CORS y monta el **router** principal (`/api/v1`).
    
- **`src/app/api/v1/routes.py`**: agrega sub-routers (`/auth`, `/users`).
    
- **`src/app/auth/*`**:
    
    - `models.py`: tablas de usuarios y relacionadas.
        
    - `deps.py`: configuración de fastapi-users (backend de cookies JWT, `current_user`).
        
    - `routes.py`: todas las rutas `/auth/*` (login, register, verify, reset).
        
    - `schemas.py`: Pydantic de entrada/salida.
        
- **`src/app/users/routes.py`**: `/users/me` (prueba de ruta protegida).
    
- **`src/app/core/config.py`**: **lee `.env`** con pydantic-settings (SECRET_KEY, DB, ORIGINS…).
    
- **`src/app/db/session.py`**: crea `engine` (Postgres) y sesiones `SessionLocal`.
    
- **`src/app/db/base.py`**: declara `Base = DeclarativeBase` (metadatos del ORM).
    
- **`src/app/db/migrations/env.py`**: runtime de Alembic → **importa modelos** aquí para que `--autogenerate` los vea.
    
- **`src/app/db/migrations/versions/*.py`**: migraciones con `upgrade()`/`downgrade()`.
    
- **`alembic.ini`**: configura dónde están las migraciones.
    
- **`docker-compose.yml`**: define y enlaza contenedores (api, db, mailpit).
    
- **`Dockerfile`**: cómo construir la imagen de la API.
    
- **`.env.example`**: plantilla de variables.
    
- **`pyproject.toml`**: dependencias, extras `.[dev]`, configs de linters y empaquetado.
    

---

# 6) Añadir tablas nuevas (ej. `tasks`) – flujo correcto

1. Crea el modelo (ej. `src/app/tasks/models.py` con `Task`).
    
2. **Importa ese modelo en `src/app/db/migrations/env.py`** (para autogenerate).
    
3. Genera migración:
    
    `docker compose run --rm api alembic revision --autogenerate -m "add tasks"`
    
4. Revisa el archivo generado (que tenga `op.create_table("tasks", ...)`).
    
5. Aplica:
    
    `docker compose run --rm api alembic upgrade head`
    
6. Listo: la tabla existe y está versionada.
    

---

# 7) Errores típicos (y cómo se arreglan)

- **Docker no arranca** → abre Docker Desktop (WSL2), `docker version` debe responder.
    
- **`ORIGINS` peta** → en `.env` ponlo en **JSON** (`["http://…"]`) o usa el validador del código.
    
- **Import circular** (`base.py` ↔ `models.py`) → **no** importes modelos en `base.py`. Impórtalos en **`migrations/env.py`**.
    
- **No existe `versions/`** → crea `src/app/db/migrations/versions/.gitkeep`.
    
- **`down_revision = 'None'`** → pon `down_revision = None` (sin comillas).
    
- **Migración generada “desaparece”** → estabas dentro de un contenedor **sin montar el código**. Añade a `compose.yml`:
    
    `services:   api:     volumes:       - .:/app`
    
- **Dockerfile falla en `pip install -e .`** → copia `src/` **antes** de instalar:
    
    `COPY pyproject.toml README.md /app/ COPY src /app/src RUN pip install -e .[dev]`
    

---

# 8) Replicar esta plantilla para OTRO proyecto

1. En GitHub, marca el repo como **Template** y pulsa **“Use this template”**.
    
2. Cambia en `pyproject.toml` el `name`, `description` y `authors`.
    
3. Si renombraras el paquete `app` a otro nombre, ajusta imports (`from app...`) y **mantén el `src`-layout**.
    
4. `copy .env.example .env` y ajusta `SECRET_KEY`, `ORIGINS`, `DATABASE_URL`.
    
5. Si el nuevo proyecto **no trae migración** inicial, genera `init` (pasos 5B y 5C).
    
6. `docker compose up` y prueba login.