## 1. Desplegar Drone en tu Synology

1. **Crear una red Docker dedicada**
    
    bash
    
    CopiarEditar
    
    `docker network create drone`
    
2. **Persistir datos**
    
    bash
    
    CopiarEditar
    
    `mkdir -p /volume1/docker/drone/{data,logs}`
    
3. **Ejecutar el servidor Drone**  
    Crea un fichero `drone-server.yml` con esto (ajusta rutas y variables):
    
    yaml
    
    CopiarEditar
    
    `version: '3'  services:   drone-server:     image: drone/drone:2     container_name: drone-server     networks:       - drone     ports:       - 80:80           # o 443 si quieres TLS     volumes:       - /volume1/docker/drone/data:/data     environment:       # Conecta con GitHub (o GitLab). Para GitHub:       - DRONE_GITHUB_CLIENT_ID=TU_CLIENT_ID       - DRONE_GITHUB_CLIENT_SECRET=TU_CLIENT_SECRET       - DRONE_RPC_SECRET=una_clave_secreta_aleatoria       - DRONE_SERVER_HOST=tu-dominio-o-ip       - DRONE_SERVER_PROTO=http       # Almacenamiento       - DRONE_DATABASE_DRIVER=sqlite3       - DRONE_DATABASE_DATASOURCE=/data/database.sqlite networks:   drone:     external: true`
    
    Y levántalo:
    
    bash
    
    CopiarEditar
    
    `docker-compose -f drone-server.yml up -d`
    
4. **Provisionar un Runner Docker**  
    Crea `drone-runner.yml`:
    
    yaml
    
    CopiarEditar
    
    `version: '3' services:   drone-runner:     image: drone/runner-docker:1     container_name: drone-runner     networks:       - drone     volumes:       - /var/run/docker.sock:/var/run/docker.sock     environment:       - DRONE_RPC_PROTO=http       - DRONE_RPC_HOST=drone-server       - DRONE_RPC_SECRET=la_misma_clave_secreta       - DRONE_RUNNER_CAPACITY=2       - DRONE_RUNNER_NAME=synology-runner networks:   drone:     external: true`
    
    Y:
    
    bash
    
    CopiarEditar
    
    `docker-compose -f drone-runner.yml up -d`
    
5. **Conectar Drone con GitHub/GitLab**
    
    - En tu organización/repositorio, instala la App Drone (o configura la integración OAuth).
        
    - Otorga permisos de lectura de repos y webhooks.
        

---

## 2. Configurar tu repositorio con `.drone.yml`

En la raíz de **cada** proyecto (frontend y backend) define un pipeline:

yaml

CopiarEditar

`kind: pipeline type: docker name: default  steps:   - name: build & push image     image: plugins/docker     settings:       repo: tu_usuario_dockerhub/${DRONE_REPO_NAME}       tags: latest       username:         from_secret: docker_username       password:         from_secret: docker_password    - name: deploy via SSH     image: appleboy/drone-ssh     settings:       host:         from_secret: ssh_host           # la IP de tu Synology       username:         from_secret: ssh_user       key:         from_secret: ssh_private_key       script:         - cd /ruta/en/host/al/proyecto/${DRONE_REPO_NAME}         - git pull origin main         - docker-compose up -d --build`

### Detalles claves

- **Secrets**  
    En la UI de Drone, añade estos secretos para cada repositorio:
    
    - `docker_username` y `docker_password` (para publicar imágenes).
        
    - `ssh_host`, `ssh_user`, `ssh_private_key` (el private key sin passphrase).
        
- **Repositorio de imágenes**  
    Si prefieres no usar Docker Hub, puedes usar GitHub Container Registry o tu propio registry privado.
    
- **Rutas y volúmenes**  
    Asegúrate de que en tu Synology el directorio `/ruta/en/host/al/proyecto/...` está montado en los contenedores de tus apps como volumen, de modo que `git pull` actualice el código que Docker compone.
    

---

## 3. Flujo de trabajo

1. Haces un push a `main` (o a la rama que configures).
    
2. **Drone** recibe el webhook, dispara el pipeline en tu runner.
    
3. Construye la imagen y la sube al registry.
    
4. Conéctate por SSH y ejecuta `docker-compose up -d --build`, que:
    
    - Reconstruye los contenedores con el código nuevo.
        
    - Los reinicia “en caliente” sin downtime significativo.
        

---

### Ventajas

- **Totalmente gratis**: Drone OSS + Docker + Synology OS.
    
- **Automático**: no hay que loguearse al DSM ni FTP.
    
- **Escalable**: puedes añadir más runners o ampliar capacidad.
    
- **Secreto centralizado**: gestión de credenciales segura en Drone.
    

Con esto, cada cambio en tus repos o rama configurada se desplegará solo, en segundos, sin desmontar manualmente nada. ¡Éxito con tu proyecto de afiliados!