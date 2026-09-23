## Documentación guay de FastAPI
https://fastapi.tiangolo.com/es/tutorial/first-steps/
## Cómo arrancar entorno virtual python
python3 -m venv nombreEntorno

## Activar entorno virtual python
#### En la cmd de WINDOWS
```
.\'nombreEntorno'\Scripts\activate.bat
```

ej:
```
.\entorno-virtual\Scripts\activate.bat
```

#### En la terminal de visual studio
```
.\'nombreEntorno'\Scripts\activate
```

ej
```
.\entorno-virtual\Scripts\activate
```

(Sin el .bat)
## Instalar FastAPI y Uvicorn
pip install fastapi
pip install uvicorn
pip install psycopg2

(Ojo, instalarlo dentro del entorno virtual)

## Instalar otras dependencias
pip install psycopg2-binary
pip install SQLAlchemy Flask-SQLAlchemy

## Arrancar servidor FastAPI
*Recordar solo tener un servidor arrancado a la vez, o se pisará la ip por defecto. O eso o cambiarla (no se cómo en este momento)*

uvicorn main:app --reload

Donde main es el fichero principal sin el .py y app es el nombre del objeto instanciado a FastAPI. El --reload es para que al cambiar algo en el código se recargue solo el servidor. No usar esa opción en producción.

## Ruta por defecto del servidor
http://127.0.0.1:8000
## Docker-Compose para la base de datos
version: '3.8'

services:
  postgres:
    image: postgres:latest
    container_name: postgres
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: postgres
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:

### Para levantarlo:
1. Tener Docker arrancado (Abrir el programa Docker Desktop y ya está)
2. docker-compose up -d

### Comandos importantes de docker-compose
#### Levantar:
docker-compose up -d
#### Parar un contenedor
docker-compose stop 'nombre' --> Si no se pone el nombre, se paran todos
#### Iniciar y reinciar
docker-compose start 'nombre'  
docker-compose restart 'nombre'

#### Recompilar contenedor
docker-compose up --build -d 'nombre'

#### Logs del contenedor
docker-compose logs -f --tail=100 'nombre'

#### Detener el contenedor
docker-compose down

## Cómo abrir la documentación que autogenera FastAPI
http:rutaServidor/docs 
ó
http:rutaServdiro/redoc

## Ver el esquema json de TODA la aplicación de FastAPI
http://127.0.0.1:8000/openapi.json

## Path instalación sdk android
C:\Users\josed\AppData\Local\Android\Sdk