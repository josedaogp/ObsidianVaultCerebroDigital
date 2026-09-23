# 🐳 Cheatsheet de Docker

Este cheatsheet cubre los comandos y conceptos más comunes de Docker, con explicaciones paso a paso y ejemplos.  ¡Usa los íconos para navegar rápidamente!

## 🔍 Conceptos Básicos

*   **Imagen (Image):** 🖼️ Plantilla de solo lectura que contiene las instrucciones para crear un contenedor.  Piensa en ella como una "foto" de un sistema de archivos y su configuración.
*   **Contenedor (Container):** 📦 Instancia en ejecución de una imagen.  Es un entorno aislado y ligero donde se ejecuta tu aplicación.
*   **Dockerfile:** 📄 Archivo de texto que contiene las instrucciones para construir una imagen Docker.
*   **Registro (Registry):** 🗄️ Repositorio de imágenes Docker (como Docker Hub).
*   **Volumen (Volume):** 💾 Mecanismo para persistir datos generados por los contenedores y compartirlos entre contenedores o con el host.
*    **Red (Network):** 🌐 Permite que los contenedores se comuniquen entre sí y con el mundo exterior.

## 🚀 Comandos Esenciales

### 🖼️ Imágenes

1.  **Buscar imágenes:**

    ```bash
    docker search <nombre_de_la_imagen>  # 🔎 Ej: docker search ubuntu
    ```

2.  **Descargar una imagen (pull):**

    ```bash
    docker pull <nombre_de_la_imagen>:<tag>  # ⬇️ Ej: docker pull ubuntu:22.04
    ```
     *Si no se especifica un `tag`, se descarga `latest` (última versión).*

3.  **Listar imágenes locales:**

    ```bash
    docker images  # 📝
    # O también:
    docker image ls
    ```

4.  **Construir una imagen a partir de un Dockerfile:**

    ```bash
    docker build -t <nombre_de_la_imagen>:<tag> <ruta_al_directorio_con_Dockerfile> # 🏗️
    ```
      *  `-t`:  Asigna un nombre y etiqueta a la imagen.
      *  `.` (punto) al final indica el directorio actual como contexto de construcción (donde está el Dockerfile).  *Ej: `docker build -t mi-app:1.0 .`*
      * Es muy común que el Dockerfile esté en la raíz de tu proyecto.

5.  **Eliminar una imagen:**

    ```bash
    docker rmi <nombre_de_la_imagen>:<tag>  # 🗑️ Ej: docker rmi ubuntu:22.04
    # O con el ID de la imagen:
    docker rmi <ID_de_la_imagen>
    ```
      *  Usa `docker images` para ver los IDs.
      *  `-f` (force): Fuerza la eliminación, incluso si hay contenedores usando la imagen (¡cuidado!).

6. **Etiquetar una Imagen**
    ```bash
        docker tag <id-imagen-origen> <nombre-usuario>/<nombre-imagen>:<tag>
    ```

### 📦 Contenedores

1.  **Crear y ejecutar un contenedor (run):**

    ```bash
    docker run [opciones] <nombre_de_la_imagen>:<tag> [comando] [argumentos]  # ▶️
    ```

    *Opciones comunes:*

    *   `-d`:  Ejecuta el contenedor en segundo plano (detached mode).
    *   `-it`:  Modo interactivo (te conecta a la terminal del contenedor).  Combínalo con `-d` para tener una terminal en segundo plano.
    *   `--name`:  Asigna un nombre al contenedor.
    *   `-p <puerto_host>:<puerto_contenedor>`:  Publica un puerto del contenedor en el host.  *Ej: `-p 8080:80` (el puerto 80 del contenedor estará disponible en el puerto 8080 del host).*
    *   `-v <ruta_host>:<ruta_contenedor>`:  Monta un volumen. *Ej: `-v /data/mi-app:/var/www/html`*
    *   `--rm`: Elimina automáticamente el contenedor cuando se detiene.
    *   `-e <VARIABLE=valor>`:  Establece variables de entorno.  *Ej: `-e MYSQL_ROOT_PASSWORD=secret`*
    * `--network <nombre_red>`: Conecta el contenedor a una red.

    *Ejemplos:*

    ```bash
    docker run -d -p 8080:80 --name mi-servidor nginx:latest  # Ejecuta Nginx en segundo plano
    docker run -it ubuntu:22.04 bash      # Inicia un contenedor Ubuntu interactivo con bash
    docker run --rm -it -v $(pwd):/app -w /app node:16 npm install #Ejecuta npm install con node en un volumen que monta el directorio actual.
    ```

2.  **Listar contenedores en ejecución:**

    ```bash
    docker ps  # 📋
    ```

    *   `-a`:  Muestra todos los contenedores (incluidos los detenidos).

3.  **Detener un contenedor:**

    ```bash
    docker stop <nombre_o_ID_del_contenedor>  # 🛑
    ```

4.  **Iniciar un contenedor detenido:**

    ```bash
    docker start <nombre_o_ID_del_contenedor>  # 🟢
    ```

5.  **Reiniciar un contenedor:**

    ```bash
    docker restart <nombre_o_ID_del_contenedor> # 🔁
    ```

6.  **Eliminar un contenedor:**
    ```bash
    docker rm <nombre_o_ID_del_contenedor> # 🗑️
    ```
      *  `-f`:  Fuerza la eliminación de un contenedor en ejecución (¡cuidado!).  Equivale a `docker stop` + `docker rm`.

7.  **Ver los logs de un contenedor:**

    ```bash
    docker logs <nombre_o_ID_del_contenedor>  # 🪵
    ```
    * `-f`: Muestra los logs en tiempo real (como `tail -f`).
    * `--tail <número>`: Muestra las últimas N líneas de los logs.

8. **Ejecutar un comando dentro de un contenedor en ejecución:**

    ```bash
    docker exec [opciones] <nombre_o_ID_del_contenedor> <comando> [argumentos] # ✍️
    ```
    * `-it`:  Modo interactivo.
    *Ejemplo: Acceder al bash de un contenedor que se llama "mi-contenedor":*
     ```bash
     docker exec -it mi-contenedor bash
     ```

9. **Copiar archivos entre el host y el contenedor (o viceversa)**

```bash
# Copiar del host al contenedor
docker cp <ruta_en_host> <nombre_contenedor>:<ruta_en_contenedor>

# Copiar del contenedor al host
docker cp <nombre_contenedor>:<ruta_en_contenedor> <ruta_en_host>
```

### 💾 Volúmenes

1.  **Crear un volumen:**

    ```bash
    docker volume create <nombre_del_volumen> # ➕
    ```

2.  **Listar volúmenes:**

    ```bash
    docker volume ls  # 📃
    ```

3.  **Inspeccionar un volumen:**

    ```bash
    docker volume inspect <nombre_del_volumen> # 🔍
    ```

4.  **Eliminar un volumen:**

    ```bash
    docker volume rm <nombre_del_volumen>  # 🗑️
    ```
     *  *¡Cuidado!  Esto elimina los datos del volumen.*

### 🌐 Redes
1. **Crear una red**
    ```bash
    docker network create <nombre_red>
    ```
2. **Listar las redes**
    ```bash
    docker network ls
    ```
3. **Conectar un contenedor a una red**
    ```
    docker network connect <nombre_red> <nombre_contenedor>
    ```
4. **Desconectar un contenedor de una red**
    ```
    docker network disconnect <nombre_red> <nombre_contenedor>
    ```

### 🐳 Docker Compose (Bonus)

Docker Compose te permite definir y gestionar aplicaciones multi-contenedor usando un archivo YAML (`docker-compose.yml`).

1.  **Iniciar la aplicación:**

    ```bash
    docker-compose up  # ⬆️ (o docker compose up, desde la versión v2 de compose)
    ```
      *  `-d`:  Ejecuta los contenedores en segundo plano.
      *  `--build`:  Reconstruye las imágenes antes de iniciar los contenedores.

2.  **Detener la aplicación:**

    ```bash
    docker-compose down  # ⬇️
    ```
      *  `--volumes`: Elimina los volúmenes definidos en el archivo `docker-compose.yml`.

3. **Listar los servicios**
```
docker-compose ps
```
4. **Ejecutar comandos en los servicios**
```
docker-compose exec <nombre_servicio> <comando>
```

###  ☁️ Docker Hub y Otros Registros

1.  **Iniciar sesión en Docker Hub (u otro registro):**

    ```bash
    docker login [opciones] [SERVIDOR]  # 🔑
    ```
    * Te pedirá tu nombre de usuario y contraseña.

2.  **Subir una imagen a Docker Hub:**

    ```bash
    docker push <nombre_de_usuario>/<nombre_de_la_imagen>:<tag>  # 📤
    ```
     *  *Primero debes etiquetar la imagen correctamente (ver `docker tag` más arriba).*

3. Cerrar sesión de Docker Hub
    ```bash
    docker logout
    ```

## ✨ Consejos Adicionales

*   Usa el completado de comandos de tu terminal (normalmente con la tecla Tab) para autocompletar nombres de imágenes, contenedores, etc.
*   Consulta la documentación oficial de Docker:  [https://docs.docker.com/](https://docs.docker.com/)
*   ¡Experimenta y diviértete!  Docker es una herramienta poderosa, pero la mejor forma de aprender es usándola.
*   Usa `--help` para obtener ayuda sobre cualquier comando de Docker (por ejemplo: `docker run --help`).