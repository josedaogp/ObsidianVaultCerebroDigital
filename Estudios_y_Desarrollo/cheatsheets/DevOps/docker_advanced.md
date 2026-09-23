# 🐳 Docker Avanzado y Dockerfiles

Esta sección amplía el cheatsheet con conceptos y comandos más avanzados, incluyendo la creación detallada de Dockerfiles.

## 🛠️ Dockerfiles Avanzados

Un Dockerfile es un script que contiene instrucciones para construir una imagen Docker.  Vamos a profundizar en las instrucciones más importantes y algunas técnicas avanzadas.

**Instrucciones Clave de Dockerfile**

*   `FROM`:  Define la imagen base.  *Siempre* es la primera instrucción (excepto `ARG` si se usa antes).
    ```dockerfile
    FROM ubuntu:22.04
    ```

*   `RUN`: Ejecuta comandos dentro de la imagen durante la construcción.  Cada `RUN` crea una nueva capa en la imagen.  Se recomienda agrupar comandos relacionados con `&&` y `\` para reducir el número de capas.
    ```dockerfile
    RUN apt-get update && apt-get install -y \
        nginx \
        curl \
        && rm -rf /var/lib/apt/lists/*  # Limpieza para reducir el tamaño
    ```

*   `WORKDIR`:  Establece el directorio de trabajo para las siguientes instrucciones (`RUN`, `CMD`, `ENTRYPOINT`, `COPY`, `ADD`).  Es mejor que usar `RUN cd ...`.
    ```dockerfile
    WORKDIR /app
    ```

*   `COPY`: Copia archivos y directorios del contexto de construcción (donde está el Dockerfile) al sistema de archivos de la imagen.
    ```dockerfile
    COPY . /app  # Copia todo el contenido del directorio actual a /app en la imagen
    COPY requirements.txt /app/
    ```

*   `ADD`:  Similar a `COPY`, pero con algunas características adicionales:
    *   Puede copiar archivos desde URLs.
    *   Puede extraer archivos comprimidos (tar, gzip, bzip2, etc.) automáticamente.  *Por estas razones, a veces se prefiere `COPY` a menos que necesites estas características extra.*
    ```dockerfile
    ADD https://example.com/archivo.tar.gz /tmp/
    ```

*   `CMD`:  Define el comando *predeterminado* que se ejecutará cuando se inicie un contenedor a partir de la imagen.  Solo puede haber *una* instrucción `CMD` (si hay varias, solo se usa la última).
    *   **Forma de shell:**  Se ejecuta a través del shell (`/bin/sh -c`).
        ```dockerfile
        CMD echo "Hola Mundo"
        ```
    *   **Forma exec (preferida):**  Se ejecuta directamente, sin shell.  Más eficiente y segura.
        ```dockerfile
        CMD ["node", "server.js"]
        ```

*   `ENTRYPOINT`:  Similar a `CMD`, pero se usa para configurar el comando *principal* del contenedor, y los argumentos pasados a `docker run` se agregan *después* del `ENTRYPOINT`.  También tiene forma de shell y exec (se prefiere exec).
    ```dockerfile
    ENTRYPOINT ["top", "-b"]
    CMD ["-c"]  # -c será el argumento para top. docker run <imagen> -H  (-H sería otro argumento)
    ```
    *   `CMD` y `ENTRYPOINT` juntos:  `ENTRYPOINT` define el ejecutable, y `CMD` los argumentos predeterminados.

*   `ENV`:  Establece variables de entorno dentro de la imagen.  Estas variables estarán disponibles en los contenedores.
    ```dockerfile
    ENV NODE_ENV=production
    ENV DATABASE_URL=postgres://user:password@host:5432/db
    ```

*   `ARG`:  Define variables que se pueden pasar *durante la construcción* de la imagen (con `docker build --build-arg`).  No persisten en los contenedores (a diferencia de `ENV`).  Útil para parametrizar la construcción.
    ```dockerfile
    ARG VERSION=1.0
    RUN echo "Versión: $VERSION"
    ```
    *   `ARG` puede ir *antes* de `FROM` para parametrizar la imagen base.

*   `EXPOSE`:  Informa a Docker que el contenedor escuchará en los puertos especificados *en tiempo de ejecución*.  No *publica* los puertos (para eso se usa `-p` en `docker run`).  Sirve principalmente como documentación.
    ```dockerfile
    EXPOSE 80 443
    ```

*   `VOLUME`:  Crea un punto de montaje para un volumen.  Indica que el contenedor persistirá datos en esa ruta.
    ```dockerfile
    VOLUME /data
    ```

*   `USER`: Establece el usuario (o UID) y/o grupo (o GID) que se usará para ejecutar los comandos `RUN`, `CMD` y `ENTRYPOINT` siguientes.  Por seguridad, es mejor evitar ejecutar como root.
    ```dockerfile
    USER node  # Si existe el usuario 'node' en la imagen base
    USER 1000 # UID
    ```

*  `HEALTHCHECK`: Define un test de Salud, para saber si el container se considera sano o no.

*  `ONBUILD` : Añade instrucciones trigger, que se ejecutan *cuando* la imagen se use como base para otra. Muy útil para automatizar pasos.

**Buenas Prácticas y Técnicas Avanzadas**

1.  **Imágenes multi-etapa (multi-stage builds):**  Permite construir la aplicación en una etapa (con todas las herramientas de desarrollo) y copiar solo los artefactos necesarios a una imagen final más pequeña.

    ```dockerfile
    # Etapa de construcción
    FROM node:16 AS builder
    WORKDIR /app
    COPY package*.json ./
    RUN npm install
    COPY . .
    RUN npm run build

    # Etapa de producción
    FROM nginx:alpine
    COPY --from=builder /app/dist /usr/share/nginx/html
    ```
    *   `--from=builder`: Copia desde la etapa `builder`.

2.  **`.dockerignore`:**  Similar a `.gitignore`, este archivo especifica archivos y directorios que deben *excluirse* del contexto de construcción.  Esto reduce el tamaño del contexto y acelera la construcción de la imagen.  *Crea un archivo llamado `.dockerignore` en el mismo directorio que el Dockerfile.*

    ```
    node_modules/
    .git/
    *.log
    tmp/
    ```

3.  **Minimizar el número de capas:**  Cada instrucción `RUN`, `COPY`, `ADD` crea una nueva capa.  Agrupa comandos y usa multi-stage builds para reducir el tamaño final de la imagen.

4.  **Usar imágenes base oficiales y pequeñas (como Alpine Linux):**  Reduce el tamaño y la superficie de ataque.

5.  **Fijar versiones de las imágenes base y dependencias:**  Evita sorpresas por cambios inesperados.

6. **Variables de entorno para la configuración:** Utiliza ENV para hacer tu imagen configurable en distintos entornos.

7. **No ejecutes como root:** Usa USER para cambiar a un usuario sin privilegios.

### 🐳 Temas Avanzados de Docker

1.  **Docker Swarm (orquestación):**  Permite gestionar un clúster de nodos Docker (similar a Kubernetes, pero más simple).  Ya no se usa tanto como Kubernetes.
    *   `docker swarm init`: Inicializa un clúster Swarm.
    *   `docker service create`: Crea un servicio.
    *   `docker stack deploy`: Despliega una aplicación multi-contenedor (similar a Docker Compose).

2.  **Docker Security Scanning:**  Herramientas para analizar imágenes en busca de vulnerabilidades.
    *   `docker scan <imagen>`:  Usa Snyk para escanear una imagen (integrado en Docker Desktop).

3.  **BuildKit:**  Un backend de construcción más moderno y eficiente (activado por defecto en Docker Desktop y en versiones recientes de Docker Engine).  Ofrece mejor rendimiento, gestión de caché, y otras características.

4.  **Docker Contexts:**  Permite cambiar fácilmente entre diferentes entornos Docker (local, remoto, Swarm, etc.).
    *   `docker context ls`: Lista los contextos.
    *   `docker context use <contexto>`: Cambia al contexto especificado.
    *   `docker context create`: Crea un nuevo contexto.

5.  **Docker y CI/CD:**  Docker es fundamental para la integración continua y el despliegue continuo.  Se usa para construir, probar y desplegar aplicaciones en entornos consistentes.

    *   Integración con herramientas como Jenkins, GitLab CI, GitHub Actions, CircleCI, etc.
    *   Construcción de imágenes en cada commit/push.
    *   Despliegue de contenedores en entornos de prueba y producción.

6. **Plugins de Docker:** Amplían la funcionalidad de Docker, para volúmenes, redes, etc.

7.  **Docker Content Trust:**  Permite verificar la integridad y el origen de las imágenes mediante firmas digitales.
    * `export DOCKER_CONTENT_TRUST=1` (Activa la verificación)
    * `docker trust` (Comandos para gestionar la confianza)
Esta extensión cubre los aspectos más importantes de Dockerfiles y Docker avanzado. Como siempre, la práctica y la experimentación son clave. ¡Mucha suerte!
