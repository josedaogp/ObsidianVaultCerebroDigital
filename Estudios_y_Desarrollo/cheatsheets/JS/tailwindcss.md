#  Tailwind CSS Cheatsheet: Utilidad Primero, Diseño Rápido 🚀

## 🌟 **Conceptos Fundamentales**

*   **Tailwind CSS:** Un framework CSS de *utilidad primero* (utility-first).  Proporciona clases predefinidas de bajo nivel (clases de utilidad) que puedes combinar para construir cualquier diseño directamente en tu HTML.
*   **Utilidad Primero (Utility-First):**  En lugar de escribir CSS personalizado para cada componente, usas clases de utilidad predefinidas que realizan una función específica (ej: `text-center`, `bg-blue-500`, `p-4`).
*   **Diseño Responsivo (Responsive Design):** Tailwind tiene modificadores de prefijo para diferentes tamaños de pantalla (breakpoints): `sm:`, `md:`, `lg:`, `xl:`, `2xl:`.
*   **Pseudo-Clases:** Tailwind tiene modificadores de prefijo para pseudo-clases (ej: `hover:`, `focus:`, `active:`, `disabled:`).
*   **Personalización:** Tailwind es altamente configurable.  Puedes personalizar los colores, tamaños, fuentes, breakpoints, etc., en el archivo `tailwind.config.js`.
* **Directivas**: `@tailwind`, `@apply`, `@layer`, `@config`
* **Funciones**: `theme()`, `screen()`

## 💻 **Instalación y Configuración**

1.  **Instalar Tailwind CSS y sus dependencias:**

    ```bash
    npm install -D tailwindcss postcss autoprefixer
    ```

2.  **Crear el archivo de configuración de Tailwind:**

    ```bash
    npx tailwindcss init -p # -p crea también el postcss.config.js
    ```
    Esto generará un archivo `tailwind.config.js`.

3.  **Configurar `tailwind.config.js`:**

    ```javascript
    // tailwind.config.js
    module.exports = {
      content: [  // Archivos donde Tailwind buscará clases
        "./src/**/*.{html,js,ts,jsx,tsx}", // Ajusta según la estructura de tu proyecto
      ],
      theme: {
        extend: {
            //Aquí puedes personalizar
        },
      },
      plugins: [],
    }
    ```

4.  **Crear un archivo CSS principal (ej: `styles.css` o `input.css`):**

    ```css
    /* styles.css */
    @tailwind base;
    @tailwind components;
    @tailwind utilities;
    ```
    *   `@tailwind base`:  Estilos base (reset).
    *   `@tailwind components`:  Estilos para tus componentes (opcional, si usas la directiva `@apply`).
    *   `@tailwind utilities`:  Clases de utilidad de Tailwind.

5.  **Importar el archivo CSS en tu proyecto:**

    *   **En un proyecto JavaScript (React, Vue, etc.):**  Importa el archivo CSS en tu archivo principal (ej: `index.js`, `main.js`, `App.js`).
    *   **En HTML plano:**  Incluye el archivo CSS compilado en el `<head>` de tu HTML.

6. **Configurar PostCSS**

```javascript
    // postcss.config.js
    module.exports = {
        plugins: {
            tailwindcss: {},
            autoprefixer: {},
        }
    }
```

7. **Compilar CSS (durante el desarrollo):**
```bash
npx tailwindcss -i ./src/input.css -o ./dist/output.css --watch
# -i input
# -o output
# --watch: observa los cambios y recompila.
```

## 🎨 **Clases de Utilidad Comunes**

### 🔤 **Tipografía**

*   **`font-sans`, `font-serif`, `font-mono`:**  Familias de fuentes.
*   **`text-{tamaño}`:**  Tamaños de fuente (ej: `text-sm`, `text-base`, `text-lg`, `text-xl`, `text-2xl`, `text-9xl`).
*   **`font-{peso}`:**  Pesos de fuente (ej: `font-thin`, `font-light`, `font-normal`, `font-medium`, `font-semibold`, `font-bold`, `font-extrabold`).
*   **`text-{color}`:**  Colores de texto (ej: `text-red-500`, `text-gray-800`, `text-blue-700`, `text-white`).  Los números representan la intensidad del color (50, 100, 200, ..., 900, 950).
*   **`text-left`, `text-center`, `text-right`, `text-justify`:**  Alineación del texto.
*   **`leading-{valor}`:**  Interlineado (ej: `leading-none`, `leading-tight`, `leading-normal`, `leading-loose`).
*   **`tracking-{valor}`:**  Espaciado entre letras (letter-spacing) (ej: `tracking-tighter`, `tracking-tight`, `tracking-normal`, `tracking-wide`).
*   **`italic`, `not-italic`:**  Cursiva.
*   **`underline`, `overline`, `line-through`, `no-underline`:**  Decoración del texto.
* **`uppercase`, `lowercase`, `capitalize`, `normal-case`**: Transformación de mayúsculas/minúsculas.
* **`whitespace-{valor}`**: Control del espacio en blanco (`whitespace-normal`, `whitespace-nowrap`, `whitespace-pre`, `whitespace-pre-line`, `whitespace-pre-wrap`).
* **`truncate`**: Trunca el texto con puntos suspensivos (...).
* **`list-none`, `list-disc`, `list-decimal`**: Estilos de lista

### 🌈 **Colores**

*   **`bg-{color}`:**  Color de fondo (ej: `bg-blue-500`, `bg-gray-200`, `bg-red-700`).
*   **`text-{color}`:**  Color de texto.
*   **`border-{color}`:**  Color del borde.
* **`ring-{color}`**: Color del anillo (para focus).
* **`placeholder-{color}`**: Color del placeholder de un input.
* **`accent-{color}`**: Color de acento
*   **Opacidad:**  `bg-opacity-{valor}`, `text-opacity-{valor}`, `border-opacity-{valor}` (ej: `bg-opacity-50`, `text-opacity-75`).  El valor va de 0 a 100.

### 📏 **Espaciado (Padding y Margin)**

*   **`p-{tamaño}`:**  Padding (relleno) en todos los lados (ej: `p-0`, `p-1`, `p-2`, `p-4`, `p-8`, `p-px` (1px)).  Los tamaños suelen estar basados en una escala (0, 1, 2, 3, 4, 5, 6, 8, 10, 12, 16, 20, 24, ... , 96). Puedes usar valores arbitrarios `p-[13px]`.
*   **`pt-{tamaño}`, `pr-{tamaño}`, `pb-{tamaño}`, `pl-{tamaño}`:**  Padding top, right, bottom, left.
*   **`px-{tamaño}`, `py-{tamaño}`:**  Padding horizontal (left y right), vertical (top y bottom).
*   **`m-{tamaño}`:**  Margin (margen) en todos los lados.
*   **`mt-{tamaño}`, `mr-{tamaño}`, `mb-{tamaño}`, `ml-{tamaño}`:**  Margin top, right, bottom, left.
*   **`mx-{tamaño}`, `my-{tamaño}`:**  Margin horizontal, vertical.
*   **Espacio negativo:**  `-m-{tamaño}`, `-mt-{tamaño}`, etc. (ej: `-mt-4`).
*   **`space-x-{tamaño}`, `space-y-{tamaño}`:**  Espacio *entre* elementos hijos (útil en `flex` y `grid`).

### 📐 **Tamaño (Width y Height)**

*   **`w-{tamaño}`:**  Ancho (ej: `w-0`, `w-1`, `w-1/2`, `w-full`, `w-screen`, `w-auto`, `w-[123px]`).
*   **`h-{tamaño}`:**  Alto (ej: `h-0`, `h-1`, `h-1/2`, `h-full`, `h-screen`, `h-auto`, `h-[45%]`).
*   **`min-w-{tamaño}`, `max-w-{tamaño}`:**  Ancho mínimo, ancho máximo.
*   **`min-h-{tamaño}`, `max-h-{tamaño}`:**  Alto mínimo, alto máximo.

### 📦 **Flexbox**

*   **`flex`:**  Convierte un elemento en un contenedor flexbox.
*   **`flex-row`, `flex-col`:**  Dirección de los elementos (fila, columna).  `flex-row-reverse`, `flex-col-reverse`.
*   **`flex-wrap`, `flex-nowrap`, `flex-wrap-reverse`**: Control del wrapping.
*   **`items-start`, `items-center`, `items-end`, `items-baseline`, `items-stretch`:**  Alineación de los elementos en el *cross axis* (eje transversal).
*   **`justify-start`, `justify-center`, `justify-end`, `justify-between`, `justify-around`, `justify-evenly`:**  Alineación de los elementos en el *main axis* (eje principal).
*   **`content-start`, `content-center`, `content-end`, `content-between`, `content-around`, `content-evenly`**: Alineación de las *líneas* de elementos (cuando hay wrapping).
*   **`self-start`, `self-center`, `self-end`, `self-baseline`, `self-stretch`:**  Alineación de un elemento *individual* dentro del contenedor flexbox.
*   **`flex-1`, `flex-auto`, `flex-initial`, `flex-none`:**  Control del crecimiento y encogimiento de los elementos (flex-grow, flex-shrink, flex-basis).
*   **`grow`, `grow-0`:**  `flex-grow: 1` y `flex-grow: 0`.
*   **`shrink`, `shrink-0`:**  `flex-shrink: 1` y `flex-shrink: 0`.
*   **`basis-{tamaño}`**: `flex-basis`.
*   **`order-{valor}`:**  Control del orden de los elementos.

### 🕸️ **Grid**

*   **`grid`:**  Convierte un elemento en un contenedor grid.
*   **`grid-cols-{número}`:**  Define el número de columnas (ej: `grid-cols-1`, `grid-cols-2`, `grid-cols-3`, `grid-cols-none`).
*   **`grid-rows-{número}`:**  Define el número de filas (ej: `grid-rows-1`, `grid-rows-2`, `grid-rows-3`, `grid-rows-none`).
*   **`grid-flow-row`, `grid-flow-col`, `grid-flow-dense`:**  Control del flujo de los elementos.
*   **`gap-{tamaño}`:**  Espacio entre las celdas (gutter).  `gap-x-{tamaño}`, `gap-y-{tamaño}`.
*   **`row-start-{número}`, `row-end-{número}`, `row-span-{número}`:**  Control de la posición y el tamaño de los elementos en las filas.
*   **`col-start-{número}`, `col-end-{número}`, `col-span-{número}`:**  Control de la posición y el tamaño de los elementos en las columnas.
*   **`auto-cols-{tamaño}`, `auto-rows-{tamaño}`**: Tamaño de las columnas/filas implícitas.
*  `justify-items-{valor}`, `justify-self-{valor}`, `items-{valor}` (start, end, center, stretch):  Alineación horizontal.
*  `content-{valor}`, `self-{valor}` (start, end, center, stretch): Alineación vertical.

### 🖼️ **Imágenes y Otros Medios**

*   **`object-cover`, `object-contain`, `object-fill`, `object-none`, `object-scale-down`:**  Control de cómo se ajusta una imagen o video (`object-fit`).
*   **`object-center`, `object-top`, `object-bottom`, `object-left`, `object-right`:**  Posición del objeto dentro de su contenedor (`object-position`).
* **`rounded-{tamaño}`**: Bordes redondeados (ej: `rounded`, `rounded-md`, `rounded-lg`, `rounded-full`).
* **`border`, `border-{tamaño}`:** Añade un borde.
    * `border-t-{tamaño}`, `border-r-{tamaño}`, `border-b-{tamaño}`, `border-l-{tamaño}`
* **`border-{estilo}`:** `border-solid`, `border-dashed`, `border-dotted`, `border-double`, `border-none`.

### 🖱️ **Interactividad**

*   **`cursor-{tipo}`:**  Tipo de cursor (ej: `cursor-pointer`, `cursor-wait`, `cursor-text`, `cursor-move`).
*   **`pointer-events-none`, `pointer-events-auto`:**  Control de los eventos del puntero.
*   **`select-none`, `select-text`, `select-all`, `select-auto`**: Control de la selección de texto.
*   **`resize`, `resize-none`, `resize-x`, `resize-y`**: Control del redimensionamiento de elementos (ej: `textarea`).
* **`outline`, `outline-none`, `outline-{color}`**: Estilos de outline.
* **`ring`, `ring-{ancho}`, `ring-offset-{ancho}`**: Añade un anillo (para focus).
* **`appearance-none`**: Quita la apariencia por defecto de elementos de formulario (select, checkbox, radio).

### ✨ **Efectos**

*   **`opacity-{valor}`:**  Opacidad (de 0 a 100).
*   **`shadow-{tamaño}`:**  Sombra (ej: `shadow-sm`, `shadow`, `shadow-md`, `shadow-lg`, `shadow-xl`, `shadow-inner`, `shadow-none`).
*   **`transition-{propiedades}`:**  Transiciones CSS (ej: `transition-all`, `transition-colors`, `transition-opacity`, `transition-shadow`, `transition-transform`).
    *   `duration-{tiempo}`:  Duración de la transición (ej: `duration-150`, `duration-300`, `duration-500`, `duration-1000` - en milisegundos).
    *   `ease-{tipo}`:  Función de easing (ej: `ease-linear`, `ease-in`, `ease-out`, `ease-in-out`).
    * `delay-{tiempo}`: Retraso de la transición.
* **`transform`:** Habilita las transformaciones (rotación, escalado, traslación, sesgado).
    *   `scale-{valor}`, `scale-x-{valor}`, `scale-y-{valor}`:  Escalado.
    *   `rotate-{valor}`:  Rotación (ej: `rotate-0`, `rotate-1`, `rotate-2`, `rotate-3`, `rotate-6`, `rotate-12`, `rotate-45`, `rotate-90`, `rotate-180`, valores negativos).
    *   `translate-x-{valor}`, `translate-y-{valor}`:  Traslación.
    *   `skew-x-{valor}`, `skew-y-{valor}`:  Sesgado.
    * `transform-gpu`: Fuerza el uso de la GPU para las transformaciones.
    * `transform-none`: Quita las transformaciones.
* **`animate-{animación}`:** Animaciones predefinidas (ej: `animate-spin`, `animate-ping`, `animate-pulse`, `animate-bounce`).

### 📱 **Diseño Responsivo (Responsive Design)**

*   **Prefijos:**  `sm:`, `md:`, `lg:`, `xl:`, `2xl:`.  Se aplican a partir de un ancho de pantalla mínimo.

    ```html
    <div class="text-center md:text-left">  <!-- Centrado en pantallas pequeñas, alineado a la izquierda en pantallas medianas y mayores -->
      <p class="text-sm md:text-base lg:text-lg">Texto responsivo</p>
    </div>
    ```

*   **Breakpoints por defecto:**
    *   `sm`: 640px
    *   `md`: 768px
    *   `lg`: 1024px
    *   `xl`: 1280px
    *   `2xl`: 1536px

### 🖱️ **Pseudo-Clases**

*   **`hover:`:**  Estilos al pasar el ratón por encima.
*   **`focus:`:**  Estilos cuando el elemento tiene el foco.
*   **`active:`:**  Estilos cuando el elemento está activo (ej: botón presionado).
*   **`disabled:`:**  Estilos para elementos deshabilitados.
*   **`first-child:`, `last-child:`, `nth-child(n):`, `odd:`, `even:`:**  Selección de hijos.
*  `focus-within:`: Se aplica al padre cuando un hijo tiene el foco.
* `group-hover:`: Se aplica a un elemento cuando se pasa el ratón por encima de un elemento padre con la clase `group`.
*  `placeholder:`: Estilos para el placeholder de un input.
* Y otras: `visited:`, `checked:`, `required:`, `invalid:`, `valid:`, `optional:`, etc.

### ⚙️ **Personalización (`tailwind.config.js`)**

```javascript
// tailwind.config.js
module.exports = {
  // ...
  theme: {
    extend: { // Añade o modifica valores
      colors: {
        'mi-color': '#f0f0f0', // Añade un nuevo color
        'azul-claro': '#90cdf4', //Otro color
      },
      fontSize: {
        'xs': '.75rem',  // 12px
        'sm': '.875rem', // 14px
        // ...
        '7xl': '5rem', // Puedes añadir nuevos tamaños
      },
      spacing: {
        '128': '32rem', // Añade un nuevo tamaño de espaciado
      },
      borderRadius: { //Modifica valores por defecto.
          none: '0',
          sm: '0.125rem',
          DEFAULT: '0.25rem',
          md: '0.375rem',
          lg: '0.5rem',
          full: '9999px',
      }
    },
  },
  // ...
};

```
*  Puedes personalizar *todos* los valores: colores, tamaños, fuentes, breakpoints, sombras, etc.
*  `theme`: Define los valores por defecto.
*  `extend`: Añade nuevos valores o modifica los existentes sin sobrescribir completamente los valores por defecto.

### 🧩 **Directivas y Funciones**

*   **`@tailwind`:**  Inyecta los estilos de Tailwind (base, components, utilities).
*   **`@apply`:**  Aplica clases de utilidad a tus propias clases CSS (dentro de archivos CSS).  *Útil para crear componentes*.

    ```css
    /* styles.css */
    .btn {
      @apply py-2 px-4 font-semibold rounded-lg shadow-md; /*Aplica clases de utilidad*/
    }

    .btn-blue {
      @apply bg-blue-500 text-white;
      @apply hover:bg-blue-700; /*Puedes usar modificadores*/
    }
    ```
* **`@layer`**: Organiza los estilos en "capas" (base, components, utilities).  Ayuda a controlar el orden de los estilos y evitar problemas de especificidad.

```css
    @layer base {
        h1 {
            @apply text-2xl;
        }
        h2 {
            @apply text-xl;
        }
    }
```
*   **`@variants`:**  Genera variantes de clases de utilidad (responsive, hover, focus, etc.).  (Obsoleto en Tailwind 3)
*   **`theme()`:**  Accede a los valores definidos en `tailwind.config.js`.

    ```css
    .btn {
      background-color: theme('colors.blue.500');
      padding: theme('spacing.2') theme('spacing.4');
    }
    ```

*   **`screen()`:**  Genera media queries para los breakpoints definidos.

    ```css
    .my-class {
      @apply text-center;

      @media screen('md') { /* Aplica estos estilos a partir del breakpoint md*/
        @apply text-left;
      }
    }
    ```
* **`@config`**:  Accede a la configuración completa de Tailwind.

### 🔌 **Plugins**

*   Tailwind CSS tiene un sistema de plugins para extender su funcionalidad.
*   **Plugins oficiales:**
    *   `@tailwindcss/forms`:  Estilos básicos para elementos de formulario.
    *   `@tailwindcss/typography`:  Estilos para contenido de texto enriquecido (ej: artículos de blog).
    *   `@tailwindcss/aspect-ratio`:  Control de la relación de aspecto de elementos.
    * `@tailwindcss/line-clamp`: Trunca el texto después de un número de líneas.
*   **Instalar plugins:**  `npm install -D <nombre-del-plugin>`
*   **Configurar plugins:**  Añade el plugin al array `plugins` en `tailwind.config.js`.

    ```javascript
    // tailwind.config.js
    module.exports = {
      // ...
      plugins: [
        require('@tailwindcss/forms'),
        require('@tailwindcss/typography'),
        // ...
      ],
    };
    ```

### 💡 **JIT (Just-in-Time) Mode**

*   El modo JIT (Just-in-Time) genera *solo* las clases de utilidad que usas en tu proyecto, lo que resulta en archivos CSS mucho más pequeños y un mejor rendimiento en desarrollo.
*   Está habilitado por defecto en Tailwind CSS v3.