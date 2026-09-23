#  Next.js Cheatsheet: El Framework de React para Producción 🚀

## 🌟 **Conceptos Fundamentales**

*   **Next.js:** Un framework de React para construir aplicaciones web *server-side rendered* (SSR), *statically generated* (SSG), y *client-side rendered* (CSR).  Proporciona:
    *   Routing basado en el sistema de archivos.
    *   Optimización de imágenes.
    *   API routes (para crear APIs).
    *   Soporte para CSS Modules, Sass, styled-components, etc.
    *   Fast Refresh (re-renderizado rápido en desarrollo).
    *   Y mucho más...
*   **Server-Side Rendering (SSR):**  Las páginas se renderizan en el servidor y se envían al cliente como HTML completo.  Mejora el SEO y el tiempo de carga inicial.
*   **Static Site Generation (SSG):**  Las páginas se generan como HTML estático en tiempo de compilación.  Ideal para sitios web con contenido que no cambia con frecuencia.
*   **Client-Side Rendering (CSR):**  La renderización tradicional de React (en el navegador).
*   **Hybrid Approach:** Next.js te permite combinar SSR, SSG y CSR en la misma aplicación.
*   **Routing:** El sistema de enrutamiento se basa en la estructura de archivos dentro del directorio `pages`.
* **API Routes**: Crea endpoints de API dentro de tu aplicación Next.js.
*  **`next/link`**: Componente para la navegación entre páginas (optimizado).
*  **`next/image`**: Componente para la optimización de imágenes.
* **`next/head`**: Componente para modificar el `<head>` de la página (título, metadatos, etc.).
* **Incremental Static Regeneration (ISR)**: Permite actualizar páginas estáticas *después* de la compilación, sin necesidad de reconstruir todo el sitio.

## 💻 **Instalación y Configuración**

1.  **Crear una nueva aplicación Next.js:**

    ```bash
    npx create-next-app mi-app
    # o
    yarn create next-app mi-app
    # o
    pnpm create next-app mi-app
    ```

2.  **Ejecutar la aplicación en modo de desarrollo:**

    ```bash
    cd mi-app
    npm run dev
    # o
    yarn dev
    # o
    pnpm dev
    ```
    La aplicación estará disponible en `http://localhost:3000`.

3. **Compilar para producción**

```bash
    npm run build
```

4. **Iniciar el servidor de producción**
```bash
    npm run start
```
## 🗂️ **Estructura de Directorios**

```
mi-app/
├── pages/            # Rutas de la aplicación (basado en el sistema de archivos)
│   ├── index.js      # Ruta principal (/)
│   ├── about.js      # Ruta /about
│   ├── posts/
│   │   ├── [id].js   # Ruta dinámica /posts/:id
│   │   └── index.js  # Ruta /posts
│   └── api/          # API routes
│       └── hello.js  # API route /api/hello
├── public/           # Archivos estáticos (imágenes, fuentes, etc.)
├── styles/           # Estilos globales (opcional)
├── components/       # Componentes de React (opcional)
├── .next/           # Archivos generados por Next.js (no modificar)
├── node_modules/
├── package.json
├── tsconfig.json    # (si usas TypeScript)
└── next.config.js   # (opcional) Configuración personalizada de Next.js
```

## 🛣️ **Routing (Enrutamiento)**

*   **Basado en el sistema de archivos:**  Cada archivo `.js`, `.jsx`, `.ts` o `.tsx` dentro del directorio `pages` se convierte en una ruta.
*   **Rutas dinámicas:**  Usa corchetes `[]` en el nombre del archivo para crear rutas dinámicas.

    *   `pages/posts/[id].js`:  Ruta `/posts/:id` (ej: `/posts/1`, `/posts/2`).  El valor de `id` se pasa como *query parameter* a la página.
    *   `pages/blog/[...slug].js`: Ruta `/blog/*` (catch-all route).  Captura todos los segmentos de la URL después de `/blog/`.
*   **`Link` (de `next/link`):**  Componente para la navegación entre páginas.  Optimiza la carga de las páginas (prefetching).

    ```jsx
    import Link from 'next/link';

    function HomePage() {
      return (
        <div>
          <h1>Inicio</h1>
          <Link href="/about">
            <a>Acerca de</a>
          </Link>
          <Link href="/posts/1">
            Ir al post 1
          </Link>
        </div>
      );
    }
    ```

*   **`useRouter` (hook):**  Proporciona información sobre la ruta actual y permite la navegación programática.

    ```jsx
    import { useRouter } from 'next/router';

    function MyComponent() {
      const router = useRouter();

      const handleClick = () => {
        router.push('/about'); // Navega a /about
      };

      return (
        <div>
          <p>Ruta actual: {router.pathname}</p>
          <p>Query params: {JSON.stringify(router.query)}</p>
          <button onClick={handleClick}>Ir a Acerca de</button>
        </div>
      );
    }
    ```
*  **Rutas anidadas:** Crea subcarpetas dentro de `pages` para crear rutas anidadas.

## 📄 **Obtención de Datos (Data Fetching)**

*   **`getStaticProps` (SSG):**  Se ejecuta en *tiempo de compilación*.  Obtiene datos y los pasa como props a la página.  Ideal para contenido estático.
*   **`getStaticPaths` (SSG - Rutas dinámicas):**  Se usa con `getStaticProps` en páginas con rutas dinámicas para especificar qué rutas se deben generar en tiempo de compilación.
*   **`getServerSideProps` (SSR):**  Se ejecuta en *cada solicitud*.  Obtiene datos y los pasa como props a la página.  Ideal para contenido que cambia con frecuencia o depende de la solicitud (ej: datos del usuario).
*  **Client-side fetching**: También puedes obtener datos en el lado del cliente (como en una aplicación React tradicional) usando `useEffect` y `fetch` o una biblioteca como `SWR` o `React Query`.

```jsx
// pages/index.js (SSG)
export async function getStaticProps() {
  // Obtener datos de una API, base de datos, sistema de archivos, etc.
  const res = await fetch('https://api.example.com/posts');
  const posts = await res.json();

  return {
    props: {
      posts, // Los datos se pasan como props a la página
    },
    revalidate: 60, // Incremental Static Regeneration (ISR): Revalida cada 60 segundos
  };
}

function HomePage({ posts }) {
  return (
    <div>
      <h1>Últimos Posts</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}

export default HomePage;
```

```jsx
// pages/posts/[id].js (SSG con rutas dinámicas)
export async function getStaticPaths() {
  // Obtener la lista de IDs de los posts (para pre-renderizar las rutas)
  const res = await fetch('https://api.example.com/posts');
  const posts = await res.json();

  const paths = posts.map((post) => ({
    params: { id: post.id.toString() }, // Los parámetros de la ruta
  }));

  return {
    paths,
    fallback: false, // o 'blocking' o true.  Si es false, 404 si la ruta no existe.
                      // 'blocking': espera a que se genere la página.
                      // true: muestra una página de fallback mientras se genera la página en segundo plano.
  };
}

export async function getStaticProps({ params }) {
  // Obtener los datos del post con el ID especificado
  const res = await fetch(`https://api.example.com/posts/${params.id}`);
  const post = await res.json();

  return {
    props: {
      post,
    },
  };
}

function PostPage({ post }) {
  // ...
}

export default PostPage;

```

```jsx
// pages/profile.js (SSR)
export async function getServerSideProps(context) {
  // Obtener datos del usuario (ej: desde una cookie, sesión, etc.)
  const { req, res, params, query } = context; // Acceso a la solicitud y respuesta

  const userId = getUserIdFromCookie(req); // Función hipotética para obtener el ID del usuario
  const user = await fetchUser(userId); // Función hipotética para obtener los datos del usuario

  return {
    props: {
      user,
    },
  };
}

function ProfilePage({ user }) {
  // ...
}

export default ProfilePage;
```

## 🖼️ **Optimización de Imágenes (`next/image`)**

*   **`Image` (de `next/image`):**  Componente para optimizar imágenes.  Proporciona:
    *   Redimensionamiento automático.
    *   Generación de diferentes tamaños para diferentes dispositivos (responsive images).
    *   Carga diferida (lazy loading).
    *   Formato WebP (si el navegador lo soporta).
    *  Placeholder blur.

    ```jsx
    import Image from 'next/image';

    function MyComponent() {
      return (
        <Image
          src="/images/mi-imagen.jpg" // Ruta a la imagen (dentro de public) o URL
          alt="Descripción de la imagen"
          width={500} // Ancho original de la imagen
          height={300} // Alto original de la imagen
          layout="responsive" // o "intrinsic" o "fixed" o "fill"
          // priority={true} // Para imágenes importantes (above the fold)
        />
      );
    }
    ```

## 🧠 **API Routes**

*   Crea endpoints de API dentro de tu aplicación Next.js.
*   Los archivos dentro del directorio `pages/api` se convierten en API routes.
*   Reciben un objeto `req` (solicitud) y un objeto `res` (respuesta).

```javascript
// pages/api/hello.js
export default function handler(req, res) {
  if (req.method === 'GET') {
    res.status(200).json({ message: 'Hola desde la API!' });
  } else {
    res.status(405).end(); // Method Not Allowed
  }
}
```

*   Puedes usar cualquier biblioteca de Node.js en las API routes (ej: para conectarte a una base de datos).
*  Puedes usar middlewares.

## 📝 **Componente `<Head>` (`next/head`)**

*   Permite modificar el `<head>` de la página (título, metadatos, etc.).

    ```jsx
    import Head from 'next/head';

    function MyPage() {
      return (
        <div>
          <Head>
            <title>Mi Página</title>
            <meta name="description" content="Descripción de mi página" />
            <link rel="icon" href="/favicon.ico" />
          </Head>
          <h1>Contenido de la página</h1>
        </div>
      );
    }
    ```

## 🎨 **Estilos (Styling)**

*   **CSS Modules:**  Archivos `.module.css` (recomendado).  Los nombres de clase se generan automáticamente para evitar conflictos.

    ```css
    /* styles/Button.module.css */
    .button {
      background-color: blue;
      color: white;
      padding: 10px;
    }
    ```

    ```jsx
    import styles from '../styles/Button.module.css';

    function Button({ children }) {
      return <button className={styles.button}>{children}</button>;
    }
    ```

*   **CSS Global (Global Styles):**  Importa un archivo CSS global en `pages/_app.js`.

    ```jsx
    // pages/_app.js
    import '../styles/globals.css'; // Importa el CSS global

    function MyApp({ Component, pageProps }) {
      return <Component {...pageProps} />;
    }

    export default MyApp;
    ```

*   **Sass/SCSS:**  Instala `sass`: `npm install sass`.  Usa archivos `.scss` o `.sass`.  Next.js tiene soporte integrado para Sass.
*   **styled-components:**  Biblioteca para escribir CSS-in-JS.
*   **Tailwind CSS:**  Framework CSS de utilidad.
*  **Emotion**: Otra biblioteca de CSS-in-JS

## ⚙️ **Configuración Personalizada (`next.config.js`)**

*   Crea un archivo `next.config.js` en la raíz del proyecto para personalizar la configuración de Next.js.

    ```javascript
    // next.config.js
    module.exports = {
      reactStrictMode: true, // Habilita el modo estricto de React
      images: {
        domains: ['example.com'], // Dominios permitidos para next/image
      },
      env: { // Variables de entorno
        MY_VARIABLE: 'valor',
      },
      // webpack: (config, { buildId, dev, isServer, defaultLoaders, webpack }) => {
      //  // Modificar la configuración de Webpack (avanzado)
      //   return config;
      // },
      // Otras opciones: basePath, compress, headers, redirects, rewrites, trailingSlash, etc.
    };
    ```

## ➕ **Temas Avanzados**

*   **`_app.js`:**  Componente personalizado para inicializar las páginas.  Útil para:
    *   Layouts persistentes.
    *   Estado global.
    *   Estilos globales.
    *   Manejo de errores personalizado.

    ```jsx
    // pages/_app.js
    function MyApp({ Component, pageProps }) {
      return (
        <Layout>
          <Component {...pageProps} />
        </Layout>
      );
    }

    export default MyApp;
    ```

*   **`_document.js`:**  Componente personalizado para modificar la estructura HTML de la página (raramente necesario).  Se usa para:
    *   Añadir atributos a `<html>` y `<body>`.
    *   Incluir scripts o estilos que deben estar presentes en *todas* las páginas.

    ```jsx
    // pages/_document.js
    import Document, { Html, Head, Main, NextScript } from 'next/document';

    class MyDocument extends Document {
      render() {
        return (
          <Html lang="es">
            <Head />
            <body>
              <Main />
              <NextScript />
            </body>
          </Html>
        );
      }
    }
    export default MyDocument
    ```
* **`_error.js`**: Página de error personalizada.
*   **Internacionalización (i18n):**  Next.js tiene soporte integrado para i18n routing.
*   **Variables de entorno:**  Usa variables de entorno para configurar tu aplicación (claves de API, URLs, etc.).
*   **Preview Mode:**  Permite previsualizar contenido que aún no está publicado (útil para CMS).
*   **Middleware**: Ejecuta código antes de que se complete una solicitud (experimental).
* **Tracing**: Integración con herramientas de tracing (Jaeger, Zipkin, etc.).

## 🚀 **Despliegue (Deployment)**

*   **Vercel:**  Plataforma de Vercel (creadores de Next.js).  La forma más fácil de desplegar aplicaciones Next.js.
*   **Netlify:**  Otra plataforma popular para desplegar aplicaciones web estáticas y serverless.
*   **AWS Amplify:**  Servicio de AWS para desplegar aplicaciones web full-stack.
*   **Servicios en la nube (AWS, Google Cloud, Azure, etc.):**  Puedes desplegar aplicaciones Next.js en cualquier servidor que soporte Node.js.
*  **Docker**: Conteneriza tu aplicación.
* **Exportación estática (`next export`):**  Genera una versión estática de tu aplicación (sin necesidad de un servidor Node.js).  No soporta todas las características de Next.js (ej: API routes, SSR).