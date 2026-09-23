# 🚀 Cheatsheet de jQuery

Este cheatsheet cubre los aspectos esenciales de jQuery, la popular biblioteca de JavaScript que simplifica la manipulación del DOM, el manejo de eventos, las animaciones y AJAX.

## 🔍 Introducción

*   **jQuery:** Biblioteca de JavaScript que facilita la interacción con el DOM, el manejo de eventos, las animaciones y AJAX.
*   **Selector:** Expresión (similar a CSS) que selecciona uno o más elementos del DOM.
*   **`$` (o `jQuery`):** Función principal de jQuery.  Se usa para seleccionar elementos, crear elementos, y acceder a la funcionalidad de jQuery.

## 📦 Incluir jQuery

1.  **CDN (Content Delivery Network):**

    ```html
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.7.1/jquery.min.js"></script>
    ```
    *   Reemplaza `3.7.1` con la versión deseada.

2.  **Descarga local:**

    *   Descarga jQuery desde [https://jquery.com/download/](https://jquery.com/download/)
    *   Incluye el archivo en tu proyecto:

    ```html
    <script src="ruta/a/jquery.min.js"></script>
    ```

## 🎯 Selectores

jQuery usa selectores (similares a CSS) para encontrar elementos en el DOM.

1.  **Selectores básicos:**

    ```javascript
    $("*")  // Todos los elementos  # *️⃣
    $("#miId")  // Elemento con id="miId"  # 🆔
    $(".miClase")  // Elementos con class="miClase"  # .️⃣
    $("p")  // Todos los elementos <p>  # 🅿️
    $("div, p")  // Todos los elementos <div> y <p>
    $("ul li")  // Elementos <li> dentro de <ul>  # ⬇️
    $("ul > li") // Elementos <li> que son hijos directos de <ul>
    $("p:first") //El primer párrafo
    $("p:last") //El último párrafo
    $("p:even") // Los párrafos pares
    $("p:odd") // Los párrafos impares
    ```

2.  **Selectores de atributos:**

    ```javascript
    $("[href]")  // Elementos con atributo href  # 🔗
    $("a[target='_blank']")  // Enlaces con target="_blank"  # 🎯
    $("input[type='text']")  // Campos de texto  # 📝
    $("img[alt]")
    $("[title='ejemplo']") //Con un valor específico
    $("[title!='ejemplo']") //Que no tengan ese valor
    $("[title^='ejemplo']") //Que empiecen por ese valor
    $("[title$='ejemplo']") //Que terminen por ese valor
    $("[title*='ejemplo']") //Que contengan ese valor
    $("[title~='ejemplo']") // Que contengan la palabra, delimitada por espacios
    ```

3.  **Selectores de jerarquía:**

    ```javascript
    $("div p")  // Todos los <p> dentro de <div> (descendientes)
    $("div > p")  // <p> que son hijos directos de <div>
    $("div + p")  // <p> inmediatamente después de un <div> (hermano adyacente)
    $("div ~ p")  // Todos los <p> hermanos después de un <div> (hermanos generales)
    ```

4.  **Filtros:**

    ```javascript
    $("li:first")  // Primer <li>  # 🥇
    $("li:last")  // Último <li>  # 🏁
    $("li:even")  // Elementos <li> pares  # 짝
    $("li:odd")  // Elementos <li> impares  # 홀
    $("li:eq(2)")  // <li> con índice 2 (tercer elemento)  # 3️⃣
    $("li:gt(2)")  // <li> con índice mayor que 2
    $("li:lt(2)")  // <li> con índice menor que 2
    $("p:empty") // Párrafos vacíos
    $("div:has(p)") //Divs que tengan un párrafo dentro
    $(":header")// Todos los headers (h1, h2, etc.)
    $(":animated") // Los elementos que se estén animando
    $(":focus") // El elemento con foco
    $(":root") // El elemento raíz (normalmente html)
    $(":contains('texto')")// Los elementos que contengan ese texto
    ```

5. **Filtros de formulario**
    ```javascript
    $(":input") //Todos los campos de formulario
    $(":text") //Los de tipo texto
    $(":password") //Los de tipo contraseña
    $(":radio") // Los de tipo radio
    $(":checkbox") //Los de tipo checkbox
    $(":submit") //Los de tipo submit
    $(":reset") //Los de tipo reset
    $(":button") //Los de tipo button
    $(":image") //Los de tipo image
    $(":file") //Los de tipo file
    $(":enabled")// Los habilitados
    $(":disabled") //Los deshabilitados
    $(":checked")// Los seleccionados
    $(":selected")// Los elementos option seleccionados
    ```

6.  **Métodos de filtrado:**

    ```javascript
    $("li").first()  // Primer <li>
    $("li").last()  // Último <li>
    $("li").eq(2)  // <li> con índice 2
    $("li").filter(".importante")  // <li> con clase "importante"
    $("li").not(".importante")  // <li> sin clase "importante"
    $("div").has("p")  // <div> que contienen <p>
    $("li").slice(1, 3) // Elementos del 1 al 3 (sin incluir el 3)
    $("p").find("span") // Encuentra los <span> dentro de los <p>
    $("div").children() // Hijos directos
    $("div").children("p") //Hijos directos que sean <p>
    $("p").parent() // El padre
    $("p").parents() //Todos los padres
    $("p").parents("div") // Todos los padres que sean <div>
    $("p").closest("div") // El ancestro más cercano que sea un <div>
    $("p").next() // El hermano siguiente
    $("p").nextAll()// Todos los hermanos siguientes
    $("p").nextUntil("h2")// Los hermanos siguientes hasta el h2 (sin incluirlo)
    $("p").prev()// El hermano anterior
    $("p").prevAll() //Todos los hermanos anteriores
    $("p").prevUntil("h2")//Los hermanos anteriores hasta el h2 (sin incluirlo)
    $("p").siblings() //Todos los hermanos.
    $("div").contents()// Todos los nodos hijos (incluye texto y comentarios).
    ```

## 💻 Manipulación del DOM

1.  **Obtener y establecer contenido:**

    ```javascript
    $("p").html()  // Obtener el HTML interno del primer <p>  # 📄
    $("p").html("<strong>Nuevo contenido</strong>")  // Establecer el HTML interno  # ✍️
    $("p").text()  // Obtener el texto interno del primer <p>  # 📝
    $("p").text("Nuevo texto")  // Establecer el texto interno
    $("input").val()  // Obtener el valor de un campo de formulario  # 📥
    $("input").val("Nuevo valor")  // Establecer el valor
    ```

2.  **Atributos:**

    ```javascript
    $("img").attr("src")  // Obtener el atributo src  # 🖼️
    $("img").attr("src", "nueva_imagen.jpg")  // Establecer el atributo src
    $("img").attr({ src: "imagen.jpg", alt: "Descripción" })  // Establecer múltiples atributos
    $("a").removeAttr("target")  // Eliminar un atributo  # 🗑️
    $("a").prop("checked") // Obtener el valor de una propiedad
    $("a").prop("checked", true) //Establecer el valor de una propiedad.
    ```

3.  **Clases CSS:**

    ```javascript
    $("div").addClass("destacado")  // Agregar una clase  # ✨
    $("div").removeClass("destacado")  // Eliminar una clase  # 💨
    $("div").toggleClass("destacado")  # Alternar una clase (agregar/quitar)  # 🔄
    $("div").hasClass("destacado")
    ```

4.  **Estilos CSS:**

    ```javascript
    $("p").css("color")  // Obtener el valor de una propiedad CSS  # 🎨
    $("p").css("color", "red")  // Establecer una propiedad CSS
    $("p").css({ color: "red", fontSize: "16px" })  // Establecer múltiples propiedades CSS
    ```

5.  **Dimensiones:**

    ```javascript
    $("div").width()  // Obtener el ancho  # 📏
    $("div").width(200)  // Establecer el ancho
    $("div").height()  // Obtener la altura
    $("div").height(100)  // Establecer la altura
    // Otros: innerWidth, innerHeight, outerWidth, outerHeight (incluyen padding y/o border)
    ```

6.  **Posición:**

    ```javascript
    $("div").offset()  // Obtener la posición (top, left) relativa al documento  # 📍
    $("div").position()  // Obtener la posición (top, left) relativa al elemento padre posicionado
    $("div").offset({top: 10, left: 20}) //Establecer la posición
    ```

7.  **Crear, agregar y eliminar elementos:**

    ```javascript
    $("<p>Nuevo párrafo</p>")  // Crear un nuevo elemento <p>  # ✨
    $("<div>").append("<p>Nuevo párrafo</p>")  // Agregar al final del <div>  # ➕
    $("<div>").prepend("<p>Nuevo párrafo</p>")  // Agregar al principio del <div>
    $("<p>Nuevo párrafo</p>").appendTo("div")  // Agregar al final del <div> (otra forma)
    $("<p>Nuevo párrafo</p>").prependTo("div")  // Agregar al principio del <div> (otra forma)
    $("p").before("<h2>Título</h2>") //Insertar antes
    $("p").after("<h2>Título</h2>") //Insertar después
    $("p").remove()  // Eliminar el elemento  # 🗑️
    $("div").empty()  // Vaciar el contenido del elemento (eliminar hijos)  # 🗑️
    $("p").replaceWith("<h2>Subtítulo</h2>") //Reemplaza el elemento.
    $("<div>").clone() //Clona un elemento.
    ```
8.  **Recorrer elementos (iteración):**

    ```javascript
     $("li").each(function(index, element) {
        console.log(index, $(element).text());
     });
    ```

## ⚡️ Eventos

1.  **Asociar eventos:**

    ```javascript
    $("button").click(function() {  // Evento click  # 🖱️
      alert("¡Haz hecho clic!");
    });

    $("input").focus(function() {  // Evento focus  # 👁️
      $(this).css("background-color", "yellow");
    });

    $("a").hover(function() {  // Evento mouseover/mouseout (hover)  # 🐭
      $(this).addClass("hover");
    }, function() {
      $(this).removeClass("hover");
    });

    // Otros eventos comunes:  blur, change, keydown, keyup, keypress, load, resize, scroll, submit, mouseover, mouseout, mousedown, mouseup, mousemove
    ```

2.  **`on` y `off` (métodos más flexibles):**

    ```javascript
    $("p").on("click", function() {  // Asociar evento  # ➕
      console.log("Clic en párrafo");
    });

    $("p").off("click");  // Desasociar evento  # ➖
    // on se puede usar para delegación de eventos (asociar a un elemento padre para manejar eventos en elementos hijos, incluso si se agregan dinámicamente)

    $("ul").on("click", "li", function() {
      console.log("Clic en un <li> dentro de un <ul>");
    });
    ```
3.  **Objeto `event`:**

    ```javascript
    $("a").click(function(event) {
      event.preventDefault();  // Evitar el comportamiento predeterminado (ej. navegar a un enlace)  # 🚫
      event.stopPropagation();  // Detener la propagación del evento a elementos padres  # 🛑
      console.log(event.type); // Tipo de evento (ej. "click")
      console.log(event.target); // El elemento que originó el evento
      console.log(event.pageX, event.pageY); // Coordenadas del evento.
    });
    ```
4.  **Eventos personalizados:**

    ```javascript
    $("div").trigger("miEvento"); //Disparar un evento

    $("div").on("miEvento", function() {  //Escuchar un evento
       console.log("Evento personalizado disparado")
    });
    ```

## ✨ Efectos (Animaciones)

1.  **Mostrar y ocultar:**

    ```javascript
    $("p").show()  // Mostrar  # 👀
    $("p").hide()  // Ocultar  # 🙈
    $("p").toggle()  // Mostrar/ocultar  # 🔄
    $("p").show(1000)  // Mostrar con duración (en milisegundos)  # ⏱️
    $("p").hide("slow")  // Ocultar lentamente ("slow", "fast", o milisegundos)
    $("p").fadeIn()
    $("p").fadeOut()
    $("p").fadeToggle()
    $("p").fadeTo(1000, 0.5) //Transparencia 0.5 en 1 segundo.
    ```

2.  **Deslizar:**

    ```javascript
    $("div").slideDown()  // Deslizar hacia abajo  # 🔽
    $("div").slideUp()  // Deslizar hacia arriba  # 🔼
    $("div").slideToggle()  // Deslizar arriba/abajo  # ↕️
    ```

3.  **Animaciones personalizadas (`animate`):**

    ```javascript
    $("div").animate({
      width: "300px",
      height: "200px",
      opacity: 0.5
    }, 1000, "linear", function() { // Duración, easing, callback
      console.log("Animación completada");
    });
    ```
    *   `animate` permite animar propiedades CSS numéricas.
    *   `easing`:  Función de easing (por defecto: "swing", otras: "linear").

4.  **Detener animaciones:**
    ```javascript
      $("div").stop()
      $("div").finish() // Completa todas las animaciones.
    ```

## 🌐 AJAX (Asynchronous JavaScript and XML)

jQuery simplifica las peticiones AJAX.

1.  **`$.ajax` (método general):**

    ```javascript
    $.ajax({
      url: "datos.json",  // URL del recurso  # 🌐
      method: "GET",  // Método HTTP (GET, POST, PUT, DELETE, etc.)  # ⚙️
      data: {  // Datos a enviar (para POST, PUT)  # 📦
        parametro1: "valor1",
        parametro2: "valor2"
      },
      dataType: "json",  // Tipo de datos esperado (json, xml, html, text)  # 🧮
      success: function(data) {  // Función a ejecutar si la petición es exitosa  # ✅
        console.log("Datos recibidos:", data);
      },
      error: function(jqXHR, textStatus, errorThrown) {  // Función a ejecutar si hay un error  # ❌
        console.error("Error AJAX:", textStatus, errorThrown);
      }
    });
    ```

2.  **Métodos abreviados:**

    ```javascript
    $.get("datos.json", function(data) {  // Petición GET  # ⬇️
      console.log(data);
    });

    $.post("guardar.php", { nombre: "Juan", edad: 30 }, function(response) {  // Petición POST  # ⬆️
      console.log(response);
    });

    $.getJSON("datos.json", function(data) { // Petición GET con JSON
        console.log(data);
    })

    $("#resultado").load("contenido.html");  // Cargar contenido HTML en un elemento  # 🔄
    ```

3.  **Eventos AJAX globales:**
    *   `ajaxStart`: Se ejecuta cuando comienza una petición AJAX (si no hay otras activas).
    *   `ajaxSend`: Se ejecuta antes de enviar una petición AJAX.
    *   `ajaxSuccess`: Se ejecuta si una petición AJAX tiene éxito.
    *   `ajaxError`: Se ejecuta si una petición AJAX falla.
    *   `ajaxComplete`: Se ejecuta cuando una petición AJAX se completa (éxito o error).
    *   `ajaxStop`: Se ejecuta cuando terminan todas las peticiones AJAX.

    ```javascript
    $(document).ajaxStart(function() {
        console.log("Comienza la carga");
    });
    ```

## ✨ Utilidades

*   `$.trim()`: Elimina espacios en blanco al principio y al final de una cadena.
*   `$.isArray()`: Verifica si un objeto es un array.
*   `$.isFunction()`: Verifica si un objeto es una función.
*   `$.isNumeric()`: Verifica si un valor es numérico.
*   `$.type()`: Obtiene el tipo de dato de un valor.
*   `$.extend()`: Combina objetos.
*   `$.each()`: Itera sobre arrays y objetos.
*   `$.grep()`: Filtra un array.
*   `$.map()`: Transforma un array.
*  `$.parseJSON()`:  Convierte una cadena JSON en un objeto JavaScript.
*  `$.now()`: Devuelve la hora actual en milisegundos
*  `$.contains()`: Comprueba si un elemento es descendiente de otro.

## 💡 jQuery y `noConflict`

Si usas jQuery con otras bibliotecas que también usan `$`, puedes usar `jQuery.noConflict()` para evitar conflictos.

```javascript
let jq = jQuery.noConflict();
jq("p").hide(); // Usar jq en lugar de $
```

Este cheatsheet cubre los aspectos más importantes de jQuery, desde los selectores y la manipulación del DOM hasta los eventos, efectos y AJAX. ¡Guárdalo y consúltalo a menudo!