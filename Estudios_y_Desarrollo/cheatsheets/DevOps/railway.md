# 🚂 Cheatsheet de Railway

Este cheatsheet cubre los aspectos esenciales de Railway, la plataforma en la nube que simplifica el despliegue y la gestión de aplicaciones y bases de datos.

## 🔍 Conceptos Básicos

*   **Proyecto (Project):** 📂 Unidad básica en Railway. Contiene servicios (aplicaciones, bases de datos, etc.).
*   **Servicio (Service):** ⚙️ Componente individual dentro de un proyecto (ej. una aplicación web, una base de datos PostgreSQL, un worker en segundo plano).
*   **Entorno (Environment):** 🌍 Conjunto de variables de entorno y configuraciones específicas (ej. desarrollo, producción). Railway crea automáticamente entornos para cada rama de Git.
*   **Plantilla (Template):** 📝 Configuración predefinida para un tipo de servicio (ej. una aplicación Node.js, una base de datos PostgreSQL).
*   **Despliegue (Deployment):** 🚀 Proceso de subir tu código a Railway y ponerlo en funcionamiento.  Railway se integra con GitHub para despliegues automáticos.
*   **Volumen (Volume):** 💾 Almacenamiento persistente para tus servicios (ej. para bases de datos).
*   **Variable de Entorno (Environment Variable):** 🔐 Valor que se puede configurar en Railway y acceder desde tu código (ej. claves de API, URLs de bases de datos).
*   **Railway CLI:** Herramienta de línea de comandos para interactuar con Railway.
* **Nixpacks:** Sistema de construcción de imágenes de contenedores que usa Railway.

## 🚀 Despliegue

1.  **Conectar un repositorio Git (GitHub):**

    *   Desde el panel de control de Railway, haz clic en "New Project".
    *   Selecciona "Deploy from GitHub repo".
    *   Conecta tu cuenta de GitHub.
    *   Selecciona el repositorio que quieres desplegar.
    *   Railway detectará automáticamente el tipo de proyecto (si es posible) y te sugerirá una plantilla.

2.  **Despliegue desde una plantilla (Starter):**

    *   En el panel de control, haz clic en "New Project".
    *   Selecciona "Start with a Template".
    *   Elige una plantilla (ej. Node.js, Python, PostgreSQL, MongoDB, Redis, etc.).
    *   Configura las opciones (si es necesario) y haz clic en "Deploy".

3. **Despliegue de una Imagen de Docker:**

    * En "New Service", selecciona "Deploy from Docker Image"
    * Especifica la imagen.

4.  **Railway CLI (línea de comandos):**

    ```bash
    npm install -g @railway/cli  # Instalar Railway CLI globalmente
    railway login  # Iniciar sesión en Railway
    railway init # Inicializa un proyecto, creando el archivo railway.toml
    railway link # Vincula el directorio actual con un proyecto
    railway up  # Desplegar el proyecto actual
    railway run <comando> # Ejecuta un comando en el entorno remoto
    ```

5.  **Despliegues automáticos:**

    *   Railway despliega automáticamente cada commit a tu rama principal (normalmente `main` o `master`).
    *   Puedes configurar ramas de *staging* para entornos de prueba.

## ⚙️ Configuración de Servicios

*   **Variables de Entorno:**  Se configuran en el panel de control (Service > Variables).  Se pueden definir a nivel de proyecto o de servicio.

*   **Volúmenes:**  Se configuran en el panel de control (Service > Settings > Volumes).  Permiten persistir datos entre despliegues.

*  **Puertos:** Los puertos que expone tu aplicación. Se configuran en el servicio.

* **Recursos (CPU/Memoria):** Puedes ajustar los recursos asignados a cada servicio.

* **Réplicas:** Escalar horizontalmente tu aplicación (más instancias).

*  **Salud (Healthchecks):** Comprueba si tu servicio está funcionando correctamente. Se configuran en el servicio.

* **Dominios personalizados**

* **`railway.toml` (opcional):** Archivo de configuración en la raíz de tu proyecto.

    ```toml
    [service]
    name = "mi-servicio"  # Nombre del servicio
    startCommand = "npm start"  # Comando para iniciar la aplicación
    buildCommand = "npm install"  # Comando para construir la aplicación (si es necesario)
    # Opciones adicionales (opcional)
    ports = [80, 443] # Puertos
    healthcheckPath = "/health"
    healthcheckTimeout = 30
    ```

## 🗄️ Bases de Datos y Otros Servicios

Railway ofrece *starters* (plantillas) para:

*   **PostgreSQL:**  Base de datos relacional.
*   **MySQL:**  Base de datos relacional.
*   **MongoDB:**  Base de datos NoSQL (documental).
*   **Redis:**  Base de datos en memoria (caché, colas de mensajes).
*  **Otras:** Despliegues desde Docker, workers, etc.

## 🔐 Variables de Entorno

*   Se configuran en el panel de control de Railway (Project > Variables o Service > Variables).
*   Disponibles en tiempo de ejecución para tus servicios.
*   Se pueden definir a nivel de proyecto o de servicio (las de servicio tienen prioridad).

*   **Acceso desde el código (Node.js):**

    ```javascript
    const miVariable = process.env.MI_VARIABLE;
    ```

* **Acceso desde el código (Python):**

    ```python
    import os
    mi_variable = os.environ.get("MI_VARIABLE")

    ```
* **Acceso con Railway CLI:**
    ```bash
        railway variables
        railway variables list
        railway variables set <NOMBRE>=<VALOR>
        railway variables delete <NOMBRE>
    ```

## 💻 Railway CLI (Comandos Comunes)

*   `railway init`: Inicializa un nuevo proyecto de Railway.
*   `railway link`: Vincula un directorio local a un proyecto de Railway existente.
*   `railway up`: Despliega el proyecto actual.
*   `railway run <comando>`: Ejecuta un comando en el entorno remoto (ej. `railway run python manage.py migrate`).
*   `railway logs`: Muestra los logs de un servicio.
*   `railway open`: Abre el proyecto en el navegador.
*   `railway status`: Muestra el estado del proyecto y sus servicios.
*   `railway env`:  Gestiona variables de entorno (similar a `vercel env`).
*   `railway whoami`:  Muestra el usuario actual.
*   `railway add`: Añade un plugin.
*  `railway connect <servicio>`: Conecta a un servicio (ej. base de datos)
*   `railway down`: Elimina el despliegue actual (pero no el proyecto).
*   `railway unlink`: Desvincula el directorio local del proyecto de Railway.
*   `railway logout`: Cierra la sesión.

## ✨ Características Avanzadas

*   **Public Networking:**  Expone tus servicios a Internet (con una URL pública).  Por defecto, los servicios solo son accesibles dentro del proyecto.
*   **Private Networking:**  Comunicación segura entre servicios dentro del mismo proyecto.
*   **Cron Jobs:**  Ejecuta tareas programadas (ej. backups, actualizaciones de datos). Se configuran desde el panel.
*   **Webhooks:**  Recibe notificaciones de eventos en Railway (ej. despliegues exitosos, errores).
* **Plugins:** Amplían la funcionalidad de Railway.
*  **API Tokens:** Para acceso programático a Railway.
* **Equipos (Teams):** Para colaboración.

Este cheatsheet cubre los aspectos más importantes de Railway, desde el despliegue y la configuración de servicios hasta el uso de la CLI y características avanzadas. ¡Espero que te sea muy útil!