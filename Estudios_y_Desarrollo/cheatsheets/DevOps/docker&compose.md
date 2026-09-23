#  Docker Cheatsheet: Contenedores para Desarrollo y Despliegue 🚀

## 🌟 **Conceptos Fundamentales**

*   **Docker:** Una plataforma para desarrollar, enviar y ejecutar aplicaciones en *contenedores*.
*   **Contenedor (Container):** Una unidad estándar de software que empaqueta el código y todas sus dependencias para que la aplicación se ejecute de forma rápida y fiable en diferentes entornos (desarrollo, pruebas, producción).  Son *ligeros* (comparten el kernel del sistema operativo anfitrión) y *aislados*.
*   **Imagen (Image):** Una plantilla de *solo lectura* que contiene las instrucciones para crear un contenedor.  Las imágenes se construyen a partir de un `Dockerfile`.
*   **Dockerfile:** Un archivo de texto que contiene las instrucciones para construir una imagen de Docker.
*   **Registro (Registry):** Un repositorio de imágenes de Docker.  El registro público más conocido es Docker Hub, pero también puedes usar registros privados (ej: Docker Registry, Amazon ECR, Google Container Registry, Azure Container Registry).
*   **Docker Hub:** El registro público de imágenes de Docker.
*   **Docker Engine:** El software que se ejecuta en el host y permite crear y ejecutar contenedores.
* **Volumen (Volume)**: Mecanismo para persistir datos generados por los contenedores.  Los datos de un volumen persisten incluso si el contenedor se elimina.
* **Red (Network)**:  Permite que los contenedores se comuniquen entre sí y con el mundo exterior.
* **Docker Compose**: Herramienta para definir y ejecutar aplicaciones multi-contenedor.  Usa un archivo `docker-compose.yml`.

## 💻 **Comandos Básicos de Docker**

*   **`docker build`:** Construye una imagen a partir de un `Dockerfile`.

    ```bash
    docker build -t mi-imagen:tag .  # -t: nombre y tag de la imagen, .: contexto (directorio actual)
    ```

*   **`docker run`:** Ejecuta un contenedor a partir de una imagen.

    ```bash
    docker run -d -p 8080:80 --name mi-contenedor mi-imagen:tag
    # -d: detached mode (ejecuta en segundo plano)
    # -p <host_port>:<container_port>: mapea un puerto del host a un puerto del contenedor
    # --name: nombre del contenedor
    ```

*   **`docker ps`:** Lista los contenedores en ejecución.
    *   `docker ps -a`:  Lista *todos* los contenedores (incluidos los detenidos).

*   **`docker images`:** Lista las imágenes.

*   **`docker stop <contenedor>`:** Detiene un contenedor.
*   **`docker start <contenedor>`:** Inicia un contenedor detenido.
*   **`docker restart <contenedor>`:** Reinicia un contenedor.
*   **`docker rm <contenedor>`:** Elimina un contenedor (debe estar detenido).
    *   `docker rm -f <contenedor>`:  Fuerza la eliminación de un contenedor en ejecución.
*   **`docker rmi <imagen>`:** Elimina una imagen.
*   **`docker exec`:** Ejecuta un comando dentro de un contenedor en ejecución.

    ```bash
    docker exec -it mi-contenedor bash  # Abre una shell interactiva dentro del contenedor
    ```

*   **`docker logs <contenedor>`:** Muestra los logs de un contenedor.
    *   `docker logs -f <contenedor>`:  Sigue los logs en tiempo real (como `tail -f`).

*   **`docker pull <imagen>`:** Descarga una imagen desde un registro.
*   **`docker push <imagen>`:** Sube una imagen a un registro.
*   **`docker login`:** Inicia sesión en un registro.
*   **`docker tag`:**  Etiqueta una imagen (crea un alias).
*   **`docker inspect <contenedor|imagen>`:** Muestra información detallada sobre un contenedor o imagen.
* **`docker cp`**: Copia archivos entre el host y un contenedor.
* **`docker commit`**: Crea una nueva imagen a partir de los cambios realizados en un contenedor.
* **`docker network ls`**: Listar redes
* **`docker network create`**: Crea una red
* **`docker volume ls`**: Listar volúmenes
* **`docker volume create`**: Crea un volumen

## 📝 **`Dockerfile` (Instrucciones)**

*   **`FROM`:**  Especifica la imagen base.
*   **`RUN`:**  Ejecuta un comando dentro de la imagen (ej: instalar paquetes, crear directorios, etc.).  Cada `RUN` crea una nueva capa (layer) en la imagen.
*   **`CMD`:**  Define el comando que se ejecutará *por defecto* cuando se inicie el contenedor.  Solo puede haber un `CMD`.
*   **`ENTRYPOINT`:**  Similar a `CMD`, pero más flexible.  Permite configurar el contenedor como un ejecutable.
*   **`WORKDIR`:**  Establece el directorio de trabajo para los comandos `RUN`, `CMD`, `ENTRYPOINT`, `COPY` y `ADD`.
*   **`COPY`:**  Copia archivos y directorios desde el contexto de construcción al contenedor.
*   **`ADD`:**  Similar a `COPY`, pero también puede extraer archivos comprimidos y descargar archivos desde URLs.
*   **`ENV`:**  Define variables de entorno dentro del contenedor.
*   **`EXPOSE`:**  Informa a Docker de que el contenedor escucha en un puerto específico (no lo publica automáticamente).
*   **`VOLUME`:**  Crea un punto de montaje para un volumen.
*   **`USER`:**  Establece el usuario (o UID) que se usará para ejecutar los comandos `RUN`, `CMD` y `ENTRYPOINT`.
*   **`ARG`:**  Define variables que se pueden pasar en tiempo de construcción (`docker build --build-arg`).
* **`LABEL`**: Añade metadatos a la imagen.
* **`ONBUILD`**: Define instrucciones que se ejecutarán cuando la imagen se use como base para otra imagen.
*  **`HEALTHCHECK`**: Define un comando para comprobar la salud del contenedor.

```dockerfile
# Ejemplo de Dockerfile
FROM ubuntu:latest  # Imagen base

LABEL maintainer="Tu Nombre <tu@email.com>"

WORKDIR /app  # Directorio de trabajo

COPY . .     # Copia los archivos del proyecto al directorio de trabajo

RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/* # Instala dependencias (buenas prácticas)

ENV PYTHONUNBUFFERED=1 # Variable de entorno

RUN pip3 install --no-cache-dir -r requirements.txt # Instala dependencias de Python

EXPOSE 8000  # Puerto en el que escucha la aplicación

CMD ["python3", "app.py"]  # Comando por defecto
```

## 🐳 **Docker Compose Cheatsheet**

```markdown
# Docker Compose Cheatsheet: Aplicaciones Multi-Contenedor 🐳

## 🌟 **Conceptos Fundamentales**

* **Docker Compose:** Una herramienta para definir y ejecutar aplicaciones multi-contenedor.  Usa un archivo `docker-compose.yml` (o `compose.yaml`) para configurar los servicios, redes y volúmenes de la aplicación.
* **Servicio (Service):**  Un contenedor que forma parte de la aplicación.  Cada servicio se define en el archivo `docker-compose.yml`.
* **`docker-compose.yml` (o `compose.yaml`):**  Archivo YAML que define la configuración de la aplicación (servicios, redes, volúmenes).

## 💻 **Comandos de Docker Compose**

*   **`docker compose up`:**  Construye (si es necesario), crea e inicia los contenedores de la aplicación.
    *   `docker compose up -d`:  Ejecuta en segundo plano (detached mode).
    *   `docker compose up --build`:  Reconstruye las imágenes antes de iniciar los contenedores.
    *   `docker compose up --scale servicio=N`: Escala un servicio a N contenedores.
*   **`docker compose down`:**  Detiene y elimina los contenedores, redes, volúmenes e imágenes creadas por `docker compose up`.
    * `docker compose down --volumes`: Elimina también los volúmenes.
    * `docker compose down --rmi all`: Elimina también las imágenes
*   **`docker compose ps`:**  Lista los contenedores de la aplicación.
*   **`docker compose start`:** Inicia los servicios
*   **`docker compose stop`:** Detiene los servicios
*   **`docker compose restart`:** Reinicia los servicios.
*   **`docker compose logs`:**  Muestra los logs de los contenedores.
    *   `docker compose logs -f`:  Sigue los logs en tiempo real.
*   **`docker compose build`:**  Construye (o reconstruye) las imágenes de los servicios.
*   **`docker compose pull`:**  Descarga las imágenes de los servicios.
*   **`docker compose push`:**  Sube las imágenes de los servicios.
*   **`docker compose exec`:**  Ejecuta un comando dentro de un contenedor en ejecución.
    *   `docker compose exec servicio bash`:  Abre una shell interactiva dentro del contenedor del servicio.
*   **`docker compose run`:** Ejecuta un comando *one-off* (puntual) en un servicio.  Crea un nuevo contenedor para el comando.
*   **`docker compose config`:** Valida y muestra la configuración de `docker-compose.yml`.
* **`docker compose kill`**: Envía una señal a los contenedores.
* **`docker compose pause`, `docker compose unpause`**: Pausa y reanuda los servicios.

## 📝 **`docker-compose.yml` (Ejemplo y Claves)**

```yaml
version: "3.9"  # Versión del formato del archivo

services:
  web:  # Nombre del servicio (puedes elegir cualquier nombre)
    build: ./web  # Ruta al directorio con el Dockerfile (o a un archivo Dockerfile específico)
    # image: mi-imagen:tag # También se puede usar una imagen preconstruida, en vez de build
    ports:
      - "8000:8000"  # Mapeo de puertos (host:contenedor)
    volumes:
      - ./web:/app  # Monta un volumen (host:contenedor)
    depends_on:
      - db  # Este servicio depende del servicio 'db'
    environment:
      - DATABASE_URL=postgres://usuario:contraseña@db:5432/basedatos # Variables de entorno
      - DEBUG=True
    # command: python manage.py runserver 0.0.0.0:8000 # Sobreescribe el comando por defecto
    # entrypoint: /app/entrypoint.sh # Sobreescribe el entrypoint
    networks:
      - mi-red # Conecta el servicio a la red 'mi-red'

  db:
    image: postgres:15 # Usa una imagen de Docker Hub
    ports:
      - "5432:5432"
    volumes:
      - db-data:/var/lib/postgresql/data # Usa un volumen con nombre
    environment:
      - POSTGRES_USER=usuario
      - POSTGRES_PASSWORD=contraseña
      - POSTGRES_DB=basedatos
    networks:
      - mi-red

volumes:  # Define los volúmenes con nombre
  db-data: {}

networks:  # Define las redes
  mi-red: {}
    # driver: bridge # Por defecto
    # external: true  # Si la red ya existe (creada fuera de Compose)
```

*   **`version`:**  Versión del formato del archivo Compose.
*   **`services`:**  Define los servicios (contenedores) de la aplicación.
*   **`build`:**  Especifica la ruta al directorio con el `Dockerfile` (o a un archivo `Dockerfile` específico).  También puede ser un objeto con opciones:
    * `context`: Ruta al directorio de construcción.
    * `dockerfile`: Nombre del archivo `Dockerfile` (si no es `Dockerfile`).
    *  `args`:  Argumentos de construcción (`build-arg` en `docker build`).
    *  `cache_from`: Imágenes para usar como caché.
    * `target`: Construye hasta un stage específico (multi-stage builds).
*   **`image`:**  Especifica la imagen que se usará para el servicio.  Puede ser una imagen local o una imagen de un registro (ej: Docker Hub).
*   **`ports`:**  Mapea puertos del host a puertos del contenedor.
*   **`volumes`:**  Monta volúmenes (para persistir datos).
    *   Volúmenes con nombre (named volumes):  Se definen en la sección `volumes` del archivo `docker-compose.yml`.
    *   Bind mounts:  Montan un directorio o archivo del host en el contenedor.
*   **`depends_on`:**  Especifica las dependencias entre servicios.  Docker Compose iniciará los servicios en el orden correcto.
*   **`environment`:**  Define variables de entorno para el contenedor.
*   **`command`:**  Sobreescribe el comando por defecto definido en la imagen.
*   **`entrypoint`:**  Sobreescribe el `ENTRYPOINT` definido en la imagen.
*   **`networks`:**  Conecta el servicio a una o más redes.
*   **`volumes` (nivel superior):**  Define volúmenes con nombre.
*   **`networks` (nivel superior):**  Define redes.
    *   `driver`:  Driver de red (por defecto: `bridge`).  Otros drivers: `host`, `overlay`, `none`.
    *   `external`:  Indica que la red ya existe (creada fuera de Compose).
*  `restart`: Política de reinicio (`no`, `always`, `on-failure`, `unless-stopped`).
* `healthcheck`:  Define un comando para comprobar la salud del contenedor.
*  `deploy`:  Opciones de despliegue (para Docker Swarm y Kubernetes).