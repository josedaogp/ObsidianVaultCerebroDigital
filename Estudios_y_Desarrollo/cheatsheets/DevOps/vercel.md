# ▲ Cheatsheet de Vercel

Este cheatsheet cubre los aspectos esenciales de Vercel, la plataforma en la nube para frontends estáticos y funciones serverless.

## 🔍 Conceptos Básicos

*   **Proyecto (Project):** 📂 Unidad básica en Vercel.  Representa una aplicación web o un conjunto de funciones serverless.  Se vincula a un repositorio Git.
*   **Despliegue (Deployment):** 🚀 Proceso de subir tu código a Vercel y hacerlo disponible en una URL.  Cada commit a tu repositorio Git puede generar un nuevo despliegue.
*   **Dominio (Domain):** 🌐 Nombre de dominio personalizado (ej. `mi-sitio.com`) que puedes asociar a tu proyecto.  Vercel también proporciona dominios gratuitos (`*.vercel.app`).
*   **Función Serverless (Serverless Function):** ⚡️ Función que se ejecuta en la nube en respuesta a una petición HTTP.  Escala automáticamente.  Se escriben en Node.js, Python, Go, Ruby, etc.
*   **Entorno de Ejecución (Runtime):** ⚙️ Entorno donde se ejecutan tus funciones serverless (Node.js, Python, etc.).
*   **Variables de Entorno (Environment Variables):** 🔐 Valores que se pueden configurar en Vercel y acceder desde tu código (ej. claves de API, URLs de bases de datos).
*   **Edge Network:** 🌍 Red global de servidores de Vercel que distribuyen tu contenido estático y ejecutan tus funciones serverless cerca de tus usuarios.
*  **Vercel CLI:** Herramienta de línea de comandos para interactuar con Vercel.
* **Framework Preset:** Configuración predefinida para frameworks como Next.js, React, Angular, Vue, etc. que optimiza el despliegue.

## 🚀 Despliegue

1.  **Conectar un repositorio Git:**

    *   Desde el panel de control de Vercel, haz clic en "New Project".
    *   Conecta tu cuenta de GitHub, GitLab o Bitbucket.
    *   Selecciona el repositorio que quieres desplegar.
    *   Vercel detectará automáticamente el framework (Next.js, Create React App, etc.) y configurará el despliegue.

2.  **Vercel CLI (línea de comandos):**

    ```bash
    npm install -g vercel  # Instalar Vercel CLI globalmente
    vercel login  # Iniciar sesión en Vercel
    vercel  # Desplegar el proyecto actual (desde el directorio del proyecto)
    vercel --prod  # Desplegar a producción
    ```

3. **Despliegues automáticos:**
    * Vercel despliega automáticamente cada commit a tu rama principal (normalmente `main` o `master`).
    * Puedes configurar *ramas de preview* para generar despliegues de vista previa en cada pull request/merge request.

## ⚙️ Configuración del Proyecto (`vercel.json`)

El archivo `vercel.json` (opcional) en la raíz de tu proyecto te permite configurar el comportamiento de Vercel.

```json
{
  "version": 2,
  "name": "mi-proyecto",
  "builds": [
    {
      "src": "package.json",
      "use": "@vercel/static-build",
      "config": { "distDir": "build" }
    },
    { "src": "api/*.js", "use": "@vercel/node" }
  ],
  "routes": [
    { "src": "/(.*)", "dest": "/public/$1" },
    { "src": "/api/(.*)", "dest": "/api/$1" }
  ],
  "env": {
    "MI_VARIABLE": "valor"
  },
  "rewrites": [
    { "source": "/about", "destination": "/public/about.html" }
  ],
  "redirects": [
    { "source": "/antigua-ruta", "destination": "/nueva-ruta", "statusCode": 301 }
  ],
  "headers": [
      {
        "source": "/(.*)",
        "headers" : [
          {
            "key" : "X-Custom-Header",
            "value" : "MiValor"
          }
        ]
      }
  ],
  "cleanUrls": true,
  "trailingSlash": false
}
```

*   **`version`:**  Versión de la configuración de Vercel (actualmente 2).
*   **`name`:**  Nombre del proyecto (opcional).
*   **`builds`:**  Define cómo se construye tu proyecto.
    *   `src`:  Patrón de archivos que se incluirán en el build.
    *   `use`:  Builder a utilizar (@vercel/node, @vercel/static-build, @vercel/python, etc.).
    *   `config`:  Opciones de configuración para el builder.  `distDir` especifica el directorio de salida (para aplicaciones estáticas).
*   **`routes`:**  Define reglas de enrutamiento (opcional).  Permite mapear rutas de entrada a archivos estáticos o funciones serverless.
*  **`rewrites`**: Redefine rutas, útil para SPAs.
* **`redirects`**: Redirecciones.
*  **`headers`**: Cabeceras HTTP personalizadas
*  **`cleanUrls`**: Elimina `.html` de las URLs.
* **`trailingSlash`**: Controla la barra final en las URLs.

*Importante:*  Para proyectos con frameworks como Next.js, Remix, SvelteKit, etc., Vercel suele inferir la configuración automáticamente, por lo que a menudo no necesitas un `vercel.json` complejo.

## ⚡️ Funciones Serverless

*   Se colocan en el directorio `api` en la raíz de tu proyecto.
*   Cada archivo en `api` se convierte en una función serverless.
*   Vercel soporta varios runtimes: Node.js, Python, Go, Ruby, etc.

**Ejemplo (Node.js - `api/hola.js`):**

```javascript
export default function handler(req, res) {
  res.status(200).json({ mensaje: 'Hola desde Vercel!' });
}
```

*   `req`:  Objeto de petición (request).
*   `res`:  Objeto de respuesta (response).

**Ejemplo (Python - `api/hola.py`):**
```python
def handler(req, res):
    res.status(200).send("Hola desde Vercel, con Python")
```
*   Para usar Python, asegúrate de tener un `requirements.txt` en la raíz con las dependencias.

## 🔐 Variables de Entorno

*   Se configuran en el panel de control de Vercel (Settings > Environment Variables).
*   Disponibles en tiempo de ejecución para tus funciones serverless y en tiempo de construcción para tu frontend.
*   Tipos:
    *   **Plaintext:**  Texto plano (visible en el panel de control).
    *   **Secret:**  Encriptado (no visible después de crearse).

*   **Acceso desde el código (Node.js):**

    ```javascript
    const miVariable = process.env.MI_VARIABLE;
    ```

* **Acceso desde el código (Python):**
    ```python
    import os
    mi_variable = os.environ.get("MI_VARIABLE")
    ```

## 🌐 Dominios

1.  **Dominio Vercel gratuito:**

    *   Cada despliegue recibe una URL única (`<nombre-proyecto>-<hash>.vercel.app`).
    *   El despliegue de producción tiene una URL más corta (`<nombre-proyecto>.vercel.app`).

2.  **Dominios personalizados:**

    *   Puedes agregar tus propios dominios (ej. `mi-sitio.com`).
    *   Vercel gestiona automáticamente los certificados SSL (HTTPS).
    *   Configuración:  Añade el dominio en el panel de control de Vercel (Settings > Domains) y configura los registros DNS en tu proveedor de dominio (A, AAAA, CNAME).  Vercel te indicará los valores correctos.

## 📊 Analíticas (Vercel Analytics)

*   Proporciona información sobre el tráfico, el rendimiento y el uso de tus funciones serverless.
*   Se activa en el panel de control (Analytics).
*  Web Analytics: Métricas de rendimiento web.
*  Functions: Uso y rendimiento de las funciones.
*  Edge Middleware: Información sobre el middleware.

## ➕ Integraciones

Vercel se integra con:

*   **Repositorios Git:** GitHub, GitLab, Bitbucket.
*   **Bases de datos:** Vercel Postgres, Vercel KV (Redis), Vercel Blob, MongoDB, Supabase, PlanetScale, Fauna, etc.
*   **CMS headless:** Sanity, Contentful, Strapi, DatoCMS, etc.
*   **Herramientas de monitoreo:** Sentry, Datadog, etc.
*   **Otras plataformas:** Shopify, Salesforce, etc.

## 💻 Vercel CLI (Comandos Comunes)

*   `vercel`: Despliega el proyecto actual.
*   `vercel --prod`: Despliega a producción.
*   `vercel dev`: Inicia un servidor de desarrollo local (emula el entorno de Vercel).
*   `vercel env`: Gestiona variables de entorno.
  *   `vercel env ls`: Lista las variables.
  * `vercel env add <nombre>`: Añade variable
  * `vercel env rm <nombre>`: Elimina una variable
  *  `vercel env pull`: Descarga las variables a un fichero `.env`.
*   `vercel logs <despliegue>`: Muestra los logs de un despliegue.
*   `vercel projects`: Gestiona proyectos.
    * `vercel projects ls`: Lista los proyectos
    * `vercel projects add <nombre>`: Crea un nuevo proyecto.
    * `vercel projects rm <nombre>`: Elimina un proyecto
*   `vercel domains`: Gestiona dominios.
    * `vercel domains ls`: Lista los dominios
    * `vercel domains add <dominio>`
    * `vercel domains rm <dominio>`
*   `vercel teams`: Gestiona equipos.
*  `vercel switch <equipo>`: Cambia entre cuentas/equipos.

## ✨ Características Avanzadas

* **Edge Functions**: Funciones que se ejecutan en la red perimetral (edge network) de Vercel, más cerca de los usuarios, para una latencia mínima.  Ideales para middleware, personalización, A/B testing, etc. (Se configuran en el `middleware.js` o `_middleware.js`)

* **Incremental Static Regeneration (ISR):**  Regenera páginas estáticas en segundo plano, sin reconstruir todo el sitio (especialmente útil para sitios con mucho contenido).  (Característica de Next.js en Vercel).

*  **Image Optimization**: Optimización de imágenes automática.

* **Cron Jobs:** Ejecuta funciones serverless en un horario programado. Se configuran en `vercel.json`.

*  **Vercel Blob:** Almacenamiento de objetos.

*   **Vercel KV:** Base de datos clave-valor (Redis).

*   **Vercel Postgres:** Base de datos PostgreSQL.

*   **Audiences:**  Define audiencias para personalización y A/B testing.

* **Webhooks:** Recibe notificaciones de eventos en Vercel.

Este cheatsheet cubre los aspectos esenciales de Vercel, desde el despliegue y la configuración hasta las funciones serverless, variables de entorno y características avanzadas. ¡Espero que te sea de gran utilidad!