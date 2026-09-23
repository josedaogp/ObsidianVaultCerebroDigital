# ⚡ Cheatsheet de FastAPI: Tu Guía Rápida para APIs Modernas 🚀

## 🌟 **Conceptos Fundamentales**

*   **FastAPI:** Un framework web moderno, *rápido* (alto rendimiento), para construir APIs con Python 3.7+, basado en las anotaciones de tipo estándar de Python (type hints).
*   **Pydantic:** Biblioteca para validación y serialización/deserialización de datos.  FastAPI usa Pydantic para definir los modelos de datos (esquemas) de las solicitudes y respuestas.
*   **Starlette:** FastAPI está construido sobre Starlette (un framework ASGI ligero).  ASGI (Asynchronous Server Gateway Interface) es el sucesor de WSGI y permite manejar solicitudes de forma asíncrona.
*   **Uvicorn:** Servidor ASGI para ejecutar aplicaciones FastAPI (en desarrollo).  En producción, se suele usar Gunicorn con Uvicorn workers.
*   **Validación automática:** FastAPI valida automáticamente los datos de las solicitudes (parámetros de ruta, query parameters, body) según los modelos Pydantic que definas.
*   **Documentación automática:** FastAPI genera automáticamente documentación interactiva de la API (Swagger UI y ReDoc) a partir de tus anotaciones de tipo y modelos Pydantic.
*  **Asíncrono**: Soporta programación asíncrona con `async` y `await`.  Esto permite manejar muchas solicitudes concurrentes sin bloquear el hilo principal.

## 💻 **Instalación**

```bash
pip install fastapi uvicorn
```

## 🚀 **Ejemplo Básico**

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hola, mundo!"}
```

*   `FastAPI()`:  Crea una instancia de la aplicación FastAPI.
*   `@app.get("/")`:  Decorador que define un *path operation* (operación de ruta).
    *   `get`:  Método HTTP (GET, POST, PUT, DELETE, etc.).
    *   `/`:  Ruta (endpoint).
*   `async def root():`:  Función que maneja la solicitud (path operation function).  La palabra clave `async` indica que es una función asíncrona.
*   `return {"message": "Hola, mundo!"}`:  Respuesta de la API.  FastAPI convierte automáticamente el diccionario en JSON.

**Ejecutar la aplicación:**

```bash
uvicorn main:app --reload
```

*   `main`:  Nombre del archivo Python (sin la extensión `.py`).
*   `app`:  Nombre de la instancia de FastAPI.
*   `--reload`:  Habilita el reinicio automático del servidor cuando se detectan cambios en el código (solo para desarrollo).

## 🛣️ **Path Operations (Operaciones de Ruta)**

*   **Métodos HTTP:**
    *   `@app.get(...)`
    *   `@app.post(...)`
    *   `@app.put(...)`
    *   `@app.delete(...)`
    *   `@app.patch(...)`
    *   `@app.options(...)`
    *   `@app.head(...)`
    *   `@app.trace(...)`

*   **Path Parameters (Parámetros de Ruta):**

    ```python
    @app.get("/items/{item_id}")
    async def read_item(item_id: int):
        return {"item_id": item_id}
    ```
    *   `{item_id}`:  Define un parámetro de ruta llamado `item_id`.
    *   `item_id: int`:  Declara el parámetro en la función y *especifica su tipo*.  FastAPI valida automáticamente que el parámetro sea un entero.
*   **Query Parameters (Parámetros de Consulta):**

    ```python
    @app.get("/items/")
    async def read_items(skip: int = 0, limit: int = 10):
        return {"skip": skip, "limit": limit}
    ```
    *   `skip: int = 0`:  Parámetro de consulta `skip`, de tipo entero, con un valor por defecto de 0.
    *   `limit: int = 10`: Parámetro de consulta `limit`, de tipo entero, con un valor por defecto de 10.
    * Se accede a estos parámetros en la URL: `/items/?skip=20&limit=5`

*   **Request Body (Cuerpo de la Solicitud):**  Para enviar datos a la API (generalmente con POST, PUT, PATCH).  Se definen usando modelos Pydantic.

    ```python
    from pydantic import BaseModel

    class Item(BaseModel):
        name: str
        description: str | None = None
        price: float
        tax: float | None = None

    @app.post("/items/")
    async def create_item(item: Item):
        return item
    ```
    *  `Item`: Modelo Pydantic que define la estructura del cuerpo de la solicitud.
    *   `item: Item`:  Declara el parámetro `item` en la función, usando el modelo `Item`.  FastAPI valida automáticamente que el cuerpo de la solicitud coincida con el modelo.
* **Combinar Path, Query y Request Body**:

    ```python
        @app.put("/items/{item_id}")
        async def update_item(item_id: int, item: Item, q: str | None = None):
            # item_id: parámetro de ruta
            # item: cuerpo de la solicitud
            # q: parámetro de query
            result = {"item_id": item_id, **item.dict()}
            if q:
                result["q"] = q
            return result
    ```

## 🗂️ **Modelos Pydantic (Esquemas)**

*   **Definición de modelos:**

    ```python
    from pydantic import BaseModel

    class User(BaseModel):
        id: int
        name: str
        email: str | None = None  # Campo opcional (puede ser None)
        is_active: bool = True  # Campo con valor por defecto
        signup_ts: datetime | None = None
        friends: list[int] = []

    ```
    *   `id: int`:  Campo `id` de tipo entero.
    *   `name: str`:  Campo `name` de tipo cadena.
    * `email: str | None = None`:  Campo `email`, opcional (puede ser `None`), de tipo cadena.  En Python 3.10+, puedes usar `Optional[str]` en lugar de `str | None`.
    *   `is_active: bool = True`:  Campo `is_active` de tipo booleano, con un valor por defecto de `True`.
    * `friends: List[int] = []`: Campo friends de tipo lista de enteros, con un valor por defecto de lista vacía.
*   **Validación:** Pydantic valida automáticamente los datos según los tipos y restricciones definidos en el modelo.
*   **Serialización/Deserialización:**  Pydantic convierte automáticamente objetos Python en diccionarios (serialización) y viceversa (deserialización).
    *   `user.dict()`:  Convierte un objeto `User` en un diccionario.
    *   `User(**data)`:  Crea un objeto `User` a partir de un diccionario `data`.
*   **Campos especiales:**
    *   `Field(...)`:  Permite configurar opciones adicionales para un campo (validaciones, descripción, etc.).
        ```python
        from pydantic import BaseModel, Field

        class Item(BaseModel):
            name: str = Field(..., title="Nombre", description="El nombre del item", min_length=3, max_length=50)
            price: float = Field(..., gt=0, description="El precio debe ser mayor que cero")
            tax: float | None = Field(None, ge=0, le=20) # Mayor o igual a 0, menor o igual a 20
        ```
        *   `...` (Ellipsis):  Indica que el campo es obligatorio.
        *   `title`, `description`:  Para la documentación.
        *   `min_length`, `max_length`, `gt` (greater than), `ge` (greater than or equal), `lt` (less than), `le` (less than or equal):  Validaciones.
    *   `constr`, `conint`, `confloat`, `condecimal`, `conlist`: Tipos "constrained" (con restricciones).
*  **Modelos anidados:**
  ```python
    class Image(BaseModel):
        url: str
        name: str

    class Item(BaseModel):
        name: str
        price: float
        images: list[Image] = [] # Lista de imágenes
  ```

## 🛡️ **Validación de Datos**

*   **Validación automática:** FastAPI + Pydantic validan:
    *   Parámetros de ruta.
    *   Parámetros de consulta.
    *   Cuerpo de la solicitud (request body).
    *   Tipos de datos.
    *   Restricciones de campos (`min_length`, `max_length`, etc.).
*   **Errores de validación:** Si los datos no son válidos, FastAPI devuelve automáticamente un error 422 (Unprocessable Entity) con una descripción detallada del error.
*   **Validación personalizada:**
    *   **Validadores de campo (`@validator`):**  Funciones que validan un campo específico.

        ```python
        from pydantic import BaseModel, validator

        class User(BaseModel):
            email: str
            password: str

            @validator("password")
            def password_must_be_strong(cls, v):
                if len(v) < 8:
                    raise ValueError("La contraseña debe tener al menos 8 caracteres")
                return v
        ```

    *   **Validadores de raíz (`@root_validator`):**  Funciones que validan el modelo completo (varios campos a la vez).

        ```python
        from pydantic import BaseModel, root_validator

        class Rectangle(BaseModel):
            width: int
            height: int

            @root_validator
            def width_must_be_less_than_height(cls, values):
                if values.get("width") >= values.get("height"): # Usar get, para evitar errores si faltan campos
                    raise ValueError("El ancho debe ser menor que el alto")
                return values
        ```
    * `field_validator`: Valida antes de que se compruebe el tipo.

## 📝 **Documentación Automática**

*   **Swagger UI:**  Interfaz interactiva para explorar y probar la API.  Accesible en `/docs`.
*   **ReDoc:**  Documentación alternativa.  Accesible en `/redoc`.
*   **Personalización:**
    *   `title`, `description`, `version` en `FastAPI()`.
    *   `summary`, `description`, `tags`, `deprecated` en los path operations.
    * `title`, `description` en los campos de los modelos Pydantic (con `Field`).

## 🔑 **Seguridad y Autenticación**

*   **`fastapi.security`:**  Módulo con utilidades para la autenticación.
    *   `OAuth2PasswordBearer`:  Para autenticación con tokens JWT (JSON Web Tokens).
    *   `HTTPBasic`, `HTTPBearer`, etc.
*   **Dependencias:** FastAPI tiene un sistema de *dependencias* muy potente.  Las dependencias son funciones que se ejecutan *antes* de un path operation.  Se usan para:
    *   Autenticación.
    *   Validación de datos.
    *   Inyección de dependencias (bases de datos, etc.).
    *   Reutilización de código.
* **Ejemplo de autenticación con OAuth2 y JWT:**

```python
from fastapi import Depends, FastAPI, HTTPException, status
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm
from jose import JWTError, jwt # pip install python-jose
from datetime import datetime, timedelta

# ... (configuración de la clave secreta, algoritmo, etc.)

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token") # Endpoint para obtener el token

# Función para crear un token
def create_access_token(data: dict, expires_delta: timedelta | None = None):
    to_encode = data.copy()
    # ... (agregar la expiración)
    encoded_jwt = jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)
    return encoded_jwt

# Función de dependencia para obtener el usuario actual
async def get_current_user(token: str = Depends(oauth2_scheme)):
    # ... (decodificar el token, verificarlo, obtener el usuario de la base de datos, etc.)
    # ... (lanzar HTTPException si hay algún error)
        return user

@app.post("/token")
async def login_for_access_token(form_data: OAuth2PasswordRequestForm = Depends()):
 # Autentica al usuario
  #Si está autenticado, crea el token
  return {"access_token": access_token, "token_type": "bearer"}

@app.get("/users/me/")
async def read_users_me(current_user: User = Depends(get_current_user)): #Dependencia
    return current_user

```

*   **CORS (Cross-Origin Resource Sharing):**  Permite que tu API sea accedida desde dominios diferentes.

    ```python
    from fastapi.middleware.cors import CORSMiddleware

    app = FastAPI()

    origins = [
        "http://localhost",
        "http://localhost:8080",
    ]

    app.add_middleware(
        CORSMiddleware,
        allow_origins=origins,
        allow_credentials=True,
        allow_methods=["*"],
        allow_headers=["*"],
    )
    ```

## 📦 **Dependencias (Dependency Injection)**

*   **`Depends()`:**  Indica una dependencia.

    ```python
    async def get_db():  # Dependencia (simula una conexión a la base de datos)
        db = {"items": []} # Simula una base de datos
        try:
            yield db
        finally:
            #Aquí iría db.close()
            pass

    @app.get("/items/")
    async def read_items(db = Depends(get_db)):
        return db["items"]

    ```

*   **Clases como dependencias:**

    ```python
    class CommonQueryParams:
        def __init__(self, q: str | None = None, skip: int = 0, limit: int = 100):
            self.q = q
            self.skip = skip
            self.limit = limit

    @app.get("/items/")
    async def read_items(commons: CommonQueryParams = Depends(CommonQueryParams)):
        # ...
    ```
* **Subdependencias**: Una dependencia puede tener a su vez otras dependencias.
* **Dependencias con `yield`**: Se usan para recursos que necesitan limpieza (conexiones a bases de datos, archivos, etc.).  El código después del `yield` se ejecuta después de que se completa el path operation (similar a `finally`).
* **Path Operation Decorators como dependencias**:
  ```python
    async def verify_token(x_token: str = Header(...)): #Verifica que se envíe un token
        if x_token != "fake-super-secret-token":
            raise HTTPException(status_code=400, detail="X-Token header invalid")

    app = FastAPI(dependencies=[Depends(verify_token)]) #Aplica a todos los path operations

    #También se puede aplicar a un path operation en específico
    @app.get("/items/", dependencies=[Depends(verify_token)])
    async def read_items():
      #...
  ```

## 🔄 **Programación Asíncrona (`async` / `await`)**

*   **`async def`:**  Define una función asíncrona (corutina).
*   **`await`:**  Espera a que una corutina se complete.  Solo se puede usar dentro de una función `async def`.
*   **Beneficios:**  Permite que tu aplicación maneje muchas solicitudes concurrentes de forma eficiente, sin bloquear el hilo principal.  Ideal para operaciones de E/S (bases de datos, red, archivos).
*   **Usar bibliotecas asíncronas:**  Si usas bases de datos, clientes HTTP, etc., asegúrate de usar bibliotecas asíncronas (ej: `databases` con SQLAlchemy Core, `httpx` en lugar de `requests`).
* **Si no usas `await`**:
  * Si la función es `async def`, FastAPI la ejecuta en un *threadpool*.
  * Si la función *no* es `async def`, FastAPI la ejecuta directamente (bloqueando el hilo principal).

## 🗄️ **Bases de Datos**

*   **SQLAlchemy (ORM):**  ORM (Object-Relational Mapper) popular para Python.  Permite interactuar con bases de datos relacionales (PostgreSQL, MySQL, SQLite, etc.) usando objetos Python.
    *  No es asíncrono por defecto.  Para usarlo de forma asíncrona, se suele usar SQLAlchemy Core (la parte de bajo nivel de SQLAlchemy) junto con una biblioteca como `databases`.
*   **`databases`:**  Biblioteca para interactuar con bases de datos de forma asíncrona.  Soporta SQLAlchemy Core, `asyncpg` (para PostgreSQL), `aiomysql` (para MySQL).
*   **ORM asíncronos:**  Hay ORMs asíncronos para Python (ej: `Tortoise ORM`, `ormar`).
* **Ejemplo (SQLAlchemy Core + `databases`):**

```python
from databases import Database
from sqlalchemy import create_engine, Column, Integer, String, MetaData, Table

DATABASE_URL = "sqlite:///./test.db"  # SQLite (archivo)

database = Database(DATABASE_URL)
metadata = MetaData()

# Definición de la tabla (SQLAlchemy Core)
users = Table(
    "users",
    metadata,
    Column("id", Integer, primary_key=True),
    Column("name", String),
    Column("email", String),
)

engine = create_engine(DATABASE_URL)
metadata.create_all(engine) # Crea la tabla

@app.on_event("startup") # Evento que se ejecuta al iniciar
async def startup():
    await database.connect()

@app.on_event("shutdown") # Evento que se ejecuta al finalizar
async def shutdown():
    await database.disconnect()

@app.post("/users/")
async def create_user(user: UserIn): # UserIn: Modelo Pydantic para la entrada
    query = users.insert().values(**user.dict())
    last_record_id = await database.execute(query)
    return {**user.dict(), "id": last_record_id}

@app.get("/users/")
async def read_users():
    query = users.select()
    return await database.fetch_all(query)

```

## ⚙️ **Configuración y Entornos**

*   **Variables de entorno:**  Usa variables de entorno para configurar tu aplicación (URL de la base de datos, clave secreta, etc.).  Paquetes como `python-dotenv` facilitan la carga de variables de entorno desde un archivo `.env`.
*   **`pydantic.BaseSettings`:**  Crea una clase que herede de `BaseSettings` para gestionar la configuración de tu aplicación.  Pydantic puede cargar automáticamente la configuración desde variables de entorno.

    ```python
    from pydantic import BaseSettings

    class Settings(BaseSettings):
        app_name: str = "Mi API"
        admin_email: str
        items_per_user: int = 50

        class Config:
            env_file = ".env" #Carga desde un archivo .env

    settings = Settings()

    @app.get("/")
    async def read_root():
      return {"message": f"Bienvenido a {settings.app_name}"}

    ```

## 🧪 **Testing**

*   **`TestClient` (de Starlette):**  Cliente para probar tu aplicación FastAPI.  Permite enviar solicitudes a tu API y verificar las respuestas.

    ```python
    from fastapi.testclient import TestClient
    from main import app  # Importa tu instancia de FastAPI

    client = TestClient(app)

    def test_read_root():
        response = client.get("/")
        assert response.status_code == 200
        assert response.json() == {"message": "Hola, mundo!"}

    def test_create_item():
      response = client.post(
          "/items/",
          json={"name": "Foo", "price": 50.2}
      )
      assert response.status_code == 200
      assert response.json() == {
          "name": "Foo",
          "price": 50.2,
          "description": None,
          "tax": None,
      }

    ```
* **`pytest`**: Framework de testing para Python (muy popular).

## 🚀 **Despliegue (Deployment)**

*   **Gunicorn:**  Servidor WSGI para ejecutar aplicaciones Python en producción.  Se suele usar junto con Uvicorn workers (para aprovechar las capacidades ASGI de FastAPI).
*   **Uvicorn workers:**  Permite que Gunicorn ejecute tu aplicación FastAPI de forma asíncrona.
*   **Contenedores (Docker):**  Empaqueta tu aplicación y sus dependencias en un contenedor Docker para un despliegue consistente y reproducible.
*   **Servicios en la nube:**  Despliega tu aplicación en plataformas como AWS, Google Cloud, Azure, Heroku, etc.
* **HTTPS**: Usa un proxy inverso (como Nginx o Traefik) para servir tu aplicación a través de HTTPS.

## ➕ **Temas Avanzados**

*   **WebSockets:**  Comunicación bidireccional en tiempo real entre el cliente y el servidor.  FastAPI tiene soporte para WebSockets.
*   **GraphQL:**  Lenguaje de consulta para APIs.  Bibliotecas como `Strawberry` y `Ariadne` se integran bien con FastAPI.
*   **Tareas en segundo plano (Background Tasks):**  Ejecuta tareas que tardan mucho tiempo sin bloquear la respuesta de la API.
    ```python
        from fastapi import BackgroundTasks
        def write_notification(email: str, message=""):
            with open("log.txt", mode="w") as email_file:
                content = f"notification for {email}: {message}"
                email_file.write(content)

        @app.post("/send-notification/{email}")
        async def send_notification(email: str, background_tasks: BackgroundTasks):
            background_tasks.add_task(write_notification, email, message="some notification")
            return {"message": "Notification sent in the background"}
    ```
*   **Middlewares:**  Funciones que se ejecutan antes y/o después de cada solicitud.  Se usan para logging, autenticación, CORS, etc.
* **Eventos de startup y shutdown**: Ejecuta código al iniciar o finalizar la app.
*   **Streaming responses:**  Envía respuestas grandes en fragmentos (chunks).
*   **Custom responses:**  Devuelve respuestas personalizadas (HTML, archivos, etc.).
* **Sub-aplicaciones**: Monta una app FastAPI dentro de otra.
