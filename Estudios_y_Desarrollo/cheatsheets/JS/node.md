#  Node.js Cheatsheet: JavaScript en el Servidor 🚀

## 🌟 **Conceptos Fundamentales**

*   **Node.js:** Un entorno de ejecución de JavaScript *basado en el motor V8 de Chrome*.  Permite ejecutar código JavaScript fuera del navegador (en el servidor, en la línea de comandos, etc.).
*   **Asíncrono y No Bloqueante (Asynchronous and Non-blocking):** Node.js está diseñado para manejar muchas operaciones de E/S (entrada/salida) de forma concurrente sin bloquear el hilo principal.  Usa un *event loop* (bucle de eventos) y *callbacks*, *promesas* o `async`/`await`.
*   **Event Loop:** El corazón de Node.js.  Es un bucle que se ejecuta continuamente y procesa eventos (conexiones de red, timers, operaciones de archivos, etc.).
*   **Módulos (Modules):**  Código organizado en archivos separados.  Node.js usa el sistema de módulos CommonJS (por defecto).  También soporta ES Modules (con la extensión `.mjs` o configurando `"type": "module"` en `package.json`).
*   **npm (Node Package Manager):** El gestor de paquetes de Node.js.  Permite instalar, gestionar y publicar paquetes (bibliotecas) de JavaScript.
*   **`package.json`:** Archivo que describe un proyecto Node.js (nombre, versión, dependencias, scripts, etc.).
*  **Callbacks**: Funciones que se pasan como argumentos a otras funciones y se ejecutan cuando se completa una operación asíncrona (el patrón más antiguo).
*  **Promesas (Promises)**: Objetos que representan el resultado eventual (éxito o fracaso) de una operación asíncrona.  Proporcionan una forma más estructurada de manejar la asincronía que los callbacks (más legibles).
*  **`async`/`await`**: Palabras clave que simplifican el trabajo con promesas.  Hacen que el código asíncrono se vea y se comporte como código síncrono (la forma más moderna y recomendada).

## 💻 **Instalación**

*   **Descarga:** Descarga el instalador de Node.js desde el sitio web oficial ([https://nodejs.org/](https://nodejs.org/)).  Se recomienda la versión LTS (Long Term Support).
*   **nvm (Node Version Manager):**  Recomendado para gestionar múltiples versiones de Node.js en el mismo sistema.

## 🚀 **Ejecutar Código Node.js**

*   **REPL (Read-Eval-Print Loop):**  Ejecuta `node` en la terminal sin argumentos para entrar en el REPL de Node.js (una consola interactiva).
*   **Ejecutar un archivo:**

    ```bash
    node mi_archivo.js
    ```

## 📦 **Módulos (Modules)**

*   **CommonJS (por defecto):**
    *   **`require()`:**  Importa un módulo.

        ```javascript
        const fs = require('fs'); // Importa el módulo 'fs' (file system)
        const miModulo = require('./mi-modulo'); // Importa un módulo local
        ```

    *   **`module.exports`:**  Exporta un valor (función, objeto, clase, etc.) desde un módulo.

        ```javascript
        // mi-modulo.js
        module.exports = function saludar(nombre) {
          console.log("Hola, " + nombre);
        };

        // O también
        module.exports = {
            sumar: (a, b) => a + b,
            PI: 3.14159,
        }
        ```
    *   **`exports`:**  Un atajo para `module.exports`.

*   **ES Modules (ECMAScript Modules):**
    *   Usa archivos con extensión `.mjs` o configura `"type": "module"` en `package.json`.
    *   **`import`:**  Importa módulos.

        ```javascript
        import fs from 'fs'; // .mjs
        import { sumar, PI } from './mi-modulo.mjs';
        ```

    *   **`export`:**  Exporta valores.

        ```javascript
        // mi-modulo.mjs
        export function saludar(nombre) {
          console.log("Hola, " + nombre);
        }

        export const PI = 3.14159;

        // Exportación por defecto:
        export default function miFuncion() { /* ... */ }
        ```

## ⚙️ **`package.json`**

*   Describe un proyecto Node.js.
*   **Crear un `package.json`:**

    ```bash
    npm init  # Te guía paso a paso
    # o
    npm init -y  # Crea un package.json con valores por defecto
    ```

*   **Campos importantes:**
    *   `name`:  Nombre del proyecto.
    *   `version`:  Versión del proyecto.
    *   `description`:  Descripción.
    *   `main`:  Punto de entrada principal (archivo principal).
    *   `scripts`:  Define scripts que se pueden ejecutar con `npm run <nombre_script>`.

        ```json
        {
          "scripts": {
            "start": "node index.js",
            "dev": "nodemon index.js",  // nodemon: reinicia el servidor automáticamente
            "test": "jest"
          }
        }
        ```

    *   `dependencies`:  Dependencias necesarias para que la aplicación funcione en producción.
    *   `devDependencies`:  Dependencias necesarias solo para desarrollo (ej: herramientas de testing, linters, etc.).
    *   `keywords`:  Palabras clave (para búsquedas en npm).
    *   `author`:  Autor.
    *   `license`:  Licencia.
    *  `type`: `"module"` para usar ES Modules por defecto.

## 📦 **npm (Node Package Manager)**

*   **`npm install <paquete>`:**  Instala un paquete (y lo guarda en `dependencies` en `package.json`).
    *   `npm install <paquete> -g`:  Instala un paquete globalmente (accesible desde cualquier lugar).
    *   `npm install <paquete> --save-dev`:  Instala un paquete como dependencia de desarrollo (`devDependencies`).
    *   `npm install`:  Instala *todas* las dependencias listadas en `package.json`.
*   **`npm uninstall <paquete>`:**  Desinstala un paquete.
*   **`npm update <paquete>`:**  Actualiza un paquete.
    *   `npm update`:  Actualiza todos los paquetes a sus últimas versiones (respetando las restricciones de versiones en `package.json`).
*   **`npm outdated`:**  Muestra los paquetes que tienen versiones más recientes disponibles.
*   **`npm run <script>`:**  Ejecuta un script definido en `package.json`.
*   **`npm start`:**  Atajo para `npm run start` (si existe un script "start").
*   **`npm test`:**  Atajo para `npm run test` (si existe un script "test").
*   **`npm init`:**  Crea un nuevo archivo `package.json`.
*   **`npm publish`:**  Publica un paquete en el registro de npm.
*   **`npx <comando>`:**  Ejecuta un comando de un paquete sin necesidad de instalarlo globalmente (útil para herramientas que se usan ocasionalmente).  Ej: `npx create-next-app`
*  **`npm audit`**: Busca vulnerabilidades de seguridad en las dependencias.
* **`npm ci`**: Instalación "limpia".  Usa el `package-lock.json` para instalar las *exactas* versiones de las dependencias.  Ideal para entornos de CI/CD.

## 🔄 **Asincronía (Asynchronous Programming)**

*   **Callbacks (Patrón más antiguo):**

    ```javascript
    fs.readFile('mi_archivo.txt', 'utf8', (err, data) => {
      if (err) {
        console.error("Error al leer el archivo:", err);
        return;
      }
      console.log("Contenido del archivo:", data);
    });
    ```

*   **Promesas (Promises):**

    ```javascript
    const fs = require('fs/promises'); // Usa la versión de promesas de fs

    fs.readFile('mi_archivo.txt', 'utf8')
      .then(data => {
        console.log("Contenido del archivo:", data);
      })
      .catch(err => {
        console.error("Error al leer el archivo:", err);
      });
    ```

*   **`async`/`await` (Forma recomendada):**

    ```javascript
    const fs = require('fs/promises');

    async function leerArchivo() {
      try {
        const data = await fs.readFile('mi_archivo.txt', 'utf8');
        console.log("Contenido del archivo:", data);
      } catch (err) {
        console.error("Error al leer el archivo:", err);
      }
    }

    leerArchivo();
    ```

## 🌐 **Módulos Nativos (Built-in Modules)**

Node.js tiene muchos módulos integrados.  Aquí tienes algunos de los más importantes:

*   **`fs` (File System):**  Para interactuar con el sistema de archivos (leer, escribir, crear archivos y directorios, etc.).
    *   `fs.readFile()`, `fs.writeFile()`, `fs.mkdir()`, `fs.readdir()`, `fs.stat()`, `fs.unlink()` (eliminar archivo), etc.
    *   Versiones síncronas (ej: `readFileSync`) y asíncronas (ej: `readFile`).  ¡Evita las versiones síncronas en el hilo principal!
    *   `fs/promises`:  Versión de promesas (recomendada).

*   **`path`:**  Para manipular rutas de archivos y directorios.
    *   `path.join()`, `path.resolve()`, `path.basename()`, `path.dirname()`, `path.extname()`, etc.

*   **`http` / `https`:**  Para crear servidores HTTP y realizar peticiones HTTP.
    *   `http.createServer()`, `http.request()`, etc.
    *   Frameworks como Express simplifican la creación de servidores web.

*   **`url`:**  Para analizar y construir URLs.
    *   `url.parse()`, `url.format()`, etc.

*   **`querystring`:**  Para analizar y construir query strings (parámetros de URL).
    *   `querystring.parse()`, `querystring.stringify()`, etc.

*   **`os` (Operating System):**  Para obtener información sobre el sistema operativo.
    *   `os.platform()`, `os.arch()`, `os.cpus()`, `os.totalmem()`, `os.freemem()`, `os.hostname()`, etc.

*   **`events`:**  Para trabajar con eventos (event emitter).

    ```javascript
    const EventEmitter = require('events');

    class MiEmisor extends EventEmitter {}

    const miEmisor = new MiEmisor();

    miEmisor.on('miEvento', (arg1, arg2) => {
      console.log("Se disparó miEvento:", arg1, arg2);
    });

    miEmisor.emit('miEvento', 'Hola', 'Mundo');
    ```

*   **`util`:**  Utilidades varias.
    *   `util.promisify()`:  Convierte una función que usa callbacks en una función que devuelve una promesa.
    *   `util.inspect()`:  Inspecciona un objeto (útil para debugging).
    * `util.types`: Utilidades para comprobar tipos.
*   **`stream`:**  Para trabajar con flujos de datos (streams).
*   **`crypto`:**  Para criptografía (hashing, cifrado, etc.).
*   **`zlib`:**  Para compresión y descompresión (gzip, deflate, etc.).
*   **`child_process`:**  Para ejecutar procesos hijos (otros programas).
    *  `child_process.exec()`, `child_process.spawn()`, `child_process.fork()`, etc.
*   **`cluster`:** Para crear aplicaciones multi-proceso (aprovechar todos los núcleos de la CPU).
* **`net`**: Para crear servidores y clientes TCP.
* **`dgram`**: Para trabajar con datagramas UDP.
*  **`dns`**: Para resolución de nombres de dominio.
* **`timers`**: `setTimeout`, `setInterval`, `setImmediate`, `clearTimeout`, `clearInterval`, `clearImmediate`.
*  **`assert`**: Para realizar aserciones (útil para testing).

## 🌐 **Servidor Web con `http` (Ejemplo Básico)**
```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  // req: Objeto de la solicitud (IncomingMessage)
  // res: Objeto de la respuesta (ServerResponse)

  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hola, mundo!\n'); // Envía la respuesta y finaliza
});

const port = 3000;
server.listen(port, () => {
  console.log(`Servidor escuchando en el puerto ${port}`);
});
```
* **Frameworks web (Express, Koa, Fastify, etc.)**: Simplifican enormemente la creación de servidores web y APIs.

## ➕ **Temas Avanzados**

*   **Buffering:**  Trabajar con datos binarios.
*   **Streams:**  Procesar grandes cantidades de datos de forma eficiente (leyendo/escribiendo en fragmentos).
*   **Workers Threads:**  Ejecutar código JavaScript en hilos separados (para tareas intensivas en CPU).
*   **Testing:**  Escribir pruebas unitarias, de integración, etc. (Jest, Mocha, etc.).
*   **Debugging:**  Usar el debugger de Node.js (integrado en VS Code y otros IDEs).
*   **Seguridad:**  Proteger tu aplicación contra vulnerabilidades comunes (inyección de código, XSS, CSRF, etc.).
*   **Despliegue (Deployment):**  Desplegar tu aplicación en servidores (AWS, Google Cloud, Azure, Heroku, etc.), usando Docker, etc.
* **Performance Profiling**:  Identificar cuellos de botella en el rendimiento.