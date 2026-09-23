Para trabajar con el proyecto después de un tiempo, es importante distinguir entre lo que ocurre **dentro de Docker** y lo que ocurre en tu **editor de código (local)**.

Aquí tienes la guía clara para tu flujo de trabajo:

## 1. ¿Cuándo activar el `.venv`?

Solo necesitas activar el entorno virtual (`.venv`) en tu terminal local en estos casos:

- **Si vas a ejecutar comandos de Python localmente**: Por ejemplo, si quieres usar `fastapi dev` para depurar rápido sin Docker.
    
- **Para que tu editor (VS Code/Cursor) funcione bien**: Si el `.venv` está activo y seleccionado en tu editor, no verás errores falsos de "import no encontrado" y tendrás autocompletado.
    
- **Instalar nuevas librerías**: Si añades una dependencia, debes hacerlo con el entorno activo para que se registre en tu `pyproject.toml` o `uv.lock`.
    

**Si solo vas a arrancar el proyecto en Docker y no vas a escribir código nuevo, NO necesitas activar el `.venv` en tu terminal.**

---

## 2. ¿Cómo funciona el "Hot Reload"?

La buena noticia es que esta plantilla ya viene configurada con **Hot Reload** (recarga automática) por defecto mediante volúmenes de Docker.

- **Lo que "pilla" automáticamente**: Cuando cambias algo en un archivo dentro de `backend/app/`, el contenedor de Docker detecta el cambio y reinicia el servidor de FastAPI al instante. No tienes que hacer nada.
    
- **Cómo verificarlo**: Puedes ver cómo se reinicia el servidor en tiempo real con este comando:
    
    Bash
    
    ```
    docker compose logs -f backend
    ```
    
    _(Verás que el log dice "StatReload detected changes" cada vez que guardes un archivo)._
    

---

## 3. ¿Qué hacer siempre que toques código?

Dependiendo de **qué** toques, el proceso cambia ligeramente:

### A. Si solo tocas la lógica (archivos `.py`)

1. Modificas el código.
    
2. Guardas el archivo.
    
3. **Docker lo pilla solo**. Revisa `localhost/docs` y verás el cambio aplicado.
    

### B. Si tocas los Modelos (`models_recor.py`)

Si añades campos o tablas nuevas:

1. Guardas el archivo.
    
2. **Obligatorio**: Debes crear y aplicar la migración de base de datos (como hicimos el otro día) para que la tabla en Postgres se actualice:
    
    Bash
    
    ```
    docker compose exec backend alembic revision --autogenerate -m "descripción del cambio"
    docker compose exec backend alembic upgrade head
    ```
    

### C. Si instalas nuevas librerías

Si añades algo al `pyproject.toml`:

1. **Obligatorio**: Debes reconstruir la imagen para que Docker instale la nueva librería:
    
    Bash
    
    ```
    docker compose build backend
    docker compose up -d
    ```
    

---

## Resumen de inicio rápido hoy:

1. Abre Docker Desktop.
    
2. En la terminal: `docker compose up -d`.
    
3. ¡A programar! (El hot-reload se encarga del resto).