# TL;DR (checklist mínimo)

1. **Clona la plantilla** (o “Use this template” en GitHub).
    
2. Mantén **src-layout**: código en `src/app/...`.
    
3. Ajusta `pyproject.toml` (nombre del proyecto) y **NO** borres las secciones de `setuptools`.
    
4. **Dockerfile**: que copie `src/` **antes** de `pip install -e .[dev]`.
    
5. `alembic.ini`: `script_location = src/app/db/migrations`.
    
6. Crea `src/app/db/migrations/versions/.gitkeep`.
    
7. `.env`: pon `ORIGINS` como **JSON** o añade validador.
    
8. **Alembic env**: importa modelos en `src/app/db/migrations/env.py` (no en `base.py`).
    
9. Genera migración inicial → `alembic revision --autogenerate -m "init"` → **revisa `down_revision = None`**.
    
10. `alembic upgrade head`, `docker compose up`.
    
11. Probar `/health`, registro, login y `/users/me`.
    

---

# Guía detallada paso a paso (con “por qué”)

## 1) Estructura: **src-layout**

`/your-project   /src     /app       main.py       api/...       auth/...       core/...       db/         base.py         session.py         migrations/           env.py           versions/.gitkeep   pyproject.toml   docker-compose.yml   Dockerfile   alembic.ini   .env.example   README.md`

**Por qué**: evita que `setuptools` detecte carpetas “ruido” como paquetes y hace que `pip install -e .` funcione limpio.

---

## 2) `pyproject.toml` listo para editable install y extras

- Mantén:
    
    - `[project]` con dependencias y `optional-dependencies.dev` (eso es `.[dev]`).
        
    - `[build-system]` con `setuptools`.
        
    - `[tool.setuptools]` y `[tool.setuptools.packages.find]` apuntando a `src`.
        

Ejemplo clave:

`[tool.setuptools] package-dir = {"" = "src"}  [tool.setuptools.packages.find] where = ["src"] include = ["app*"] exclude = ["tests*"]`

**Por qué**: `pip install -e .[dev]` te da imports confiables (no dependes de `PYTHONPATH`) y separa deps de dev.

---

## 3) **Dockerfile**: orden correcto

`FROM python:3.11-slim WORKDIR /app  RUN apt-get update && apt-get install -y --no-install-recommends \     build-essential libpq-dev && rm -rf /var/lib/apt/lists/*  # 👇 Copia metadata y código antes de instalar en editable COPY pyproject.toml README.md /app/ COPY src /app/src  RUN pip install --no-cache-dir --upgrade pip \  && pip install --no-cache-dir -e .[dev]  # Resto (alembic.ini, compose, etc.) COPY . /app  EXPOSE 8000 CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]`

**Por qué**: el editable install necesita ver `src/` y `README.md`; si no, fallará en build.

---

## 4) `docker-compose.yml` (DB + API + Mailpit)

- Servicio `db` (Postgres), `api` (FastAPI) y `mailpit` (SMTP de dev).
    
- En `api.command`, puedes mantener:
    
    `sh -c "alembic upgrade head && uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload"`
    

**Por qué**: `upgrade head` asegura que la DB tiene el esquema al arrancar.

---

## 5) Alembic: **ruta y versiones**

- En `alembic.ini`:
    
    `[alembic] script_location = src/app/db/migrations`
    
- Asegúrate de que existe `src/app/db/migrations/versions/.gitkeep` (o cualquier archivo) para que Docker la copie.
    

**Por qué**: Alembic escribirá las revisiones en `versions/`.

---

## 6) Modelos registrados **solo** en Alembic (no en runtime)

- **NO** importes modelos en `src/app/db/base.py`; déjalo así:
    
    `from sqlalchemy.orm import DeclarativeBase class Base(DeclarativeBase): pass`
    
- **SÍ** importa modelos en `src/app/db/migrations/env.py`:
    
    `from app.db.base import Base from app.auth.models import User, OAuthAccount, RefreshToken, AuthEvent  # noqa: F401 target_metadata = Base.metadata`
    

**Por qué**: evita **ciclos de import** en la app; Alembic sí necesita ver los modelos cuando autogenera.

---

## 7) Variables de entorno (`.env`)

Usa el ejemplo y **pon ORIGINS como JSON** o añade validador:

`APP_NAME=FastAPI Template ENV=dev SECRET_KEY=change_me ACCESS_TOKEN_EXPIRE_MINUTES=60 REFRESH_TOKEN_EXPIRE_DAYS=30 DATABASE_URL=postgresql://postgres:postgres@db:5432/app SMTP_HOST=mailpit SMTP_PORT=1025 SMTP_USER= SMTP_PASSWORD= SMTP_TLS=false ORIGINS=["http://localhost:5173","http://localhost:3000"]`

**Por qué**: `pydantic-settings` espera lista real; la CSV simple rompe si no haces un validador.

_(Si prefieres CSV, mete este validador en `core/config.py`)_:

`from pydantic import field_validator import json class Settings(BaseSettings):     ORIGINS: list[str] = ["http://localhost:5173","http://localhost:3000"]     @field_validator("ORIGINS", mode="before")     def parse_origins(cls, v):         if isinstance(v, str):             v=v.strip()             return json.loads(v) if v.startswith("[") else [p.strip() for p in v.split(",") if p.strip()]         return v`

---

## 8) Generar **migración inicial**

1. **Build** y genera migración:
    

`docker compose build docker compose run --rm api alembic revision --autogenerate -m "init"`

2. Abre el archivo creado en `src/app/db/migrations/versions/..._init.py` y revisa:
    

`revision = "xxxxx" down_revision = None   # <-- sin comillas`

(Corrige si aparece `'None'` con comillas.)

**Por qué**: defines el “origen” del grafo de migraciones.

---

## 9) Aplicar migraciones y levantar

`docker compose run --rm api alembic upgrade head docker compose up`

**Por qué**: asegura que la DB ya tenga el esquema antes/ mientras sube la API.

---

## 10) Probar

**Health**

`curl http://localhost:8000/health`

**Registro**

`curl -X POST http://localhost:8000/api/v1/auth/register \   -H "Content-Type: application/json" \   -d '{"email":"joseda@example.com","password":"Test12345","full_name":"Joseda"}'`

**Login (cookies)**

`curl -i -c cookies.txt -X POST http://localhost:8000/api/v1/auth/jwt/login \   -H "Content-Type: application/x-www-form-urlencoded" \   -d "username=joseda@example.com&password=Test12345"`

**Ruta protegida**

`curl -b cookies.txt http://localhost:8000/api/v1/users/me`

**Ver tablas** (opcional):

`docker exec -it fastapi-login-db-1 psql -U postgres -d app -c "\dt"`

---

## 11) Cosas que suelen romper (y cómo evitarlas)

- **Daemon Docker** apagado → abre **Docker Desktop** (WSL2 activo).
    
- **`ORIGINS` mal formateado** → usa JSON o el validador.
    
- **Import circular** (`base.py` ↔ `models.py`) → solo importa modelos en **Alembic `env.py`**.
    
- **No existe `versions/`** → crea `.../versions/.gitkeep`.
    
- **Dockerfile instala antes de copiar `src/`** → cambia el orden como arriba.
    
- **`down_revision = 'None'`** → pon `None` (sin comillas).
    

---

## 12) Copiar a un proyecto nuevo (limpio y rápido)

1. En GitHub, marca este repo como **Template** (Settings → Template repository).
    
2. **Use this template** → nuevo repo.
    
3. Cambia en `pyproject.toml`:
    
    - `name`, `description`, `authors`.
        
4. Si renombraras el paquete (de `app` a otro), cambia imports (`from app...`) y **mantén src-layout**.
    
5. `cp .env.example .env`, revisa `DATABASE_URL` y `ORIGINS`.
    
6. Pasos **8–10** (migración init, upgrade, up) y listo.
    

---

## 13) (Opcional) Atajos de desarrollo

**Makefile** o `justfile`:

`up:          ## Levanta stack \tdocker compose up build:       ## Build de imagen \tdocker compose build migrate:     ## Genera migración autogenerada \tdocker compose run --rm api alembic revision --autogenerate -m "$(m)" upgrade:     ## Aplica migraciones \tdocker compose run --rm api alembic upgrade head psql:        ## Conectar a la DB \tdocker exec -it fastapi-login-db-1 psql -U postgres -d app`