## Objetivo

Diseñar una app que gracias a sensores, muestre información acerca de una sesión de cachimba. La aplicación estará hecha en flutter y se conectará mediante bluetooth a un ~~arduino con un módulo bluetooth hc-06~~  ESP32 y un sensor bme280. El arduino por ahora solo tiene la funcionalidad de enviar cada medio segundo los datos leídos por el sensor. La aplicación debe recibir esos datos para tratarlos. El protocolo Bluetooth a utilizar será ~~por ahora serial, pero en futuras versiones se puede estudiar la opción~~ BLE (Bluetooth Low Energy).

## Información posible a mostrar en una sesión de cachimba

·        Duración de la sesión

·        Número de caladas de la sesión

·        Tiempo de la última calada dada

·        Tiempo medio de calada de la sesión

·        Duración media de varias sesiones (hay que ver el periodo de tiempo)

·        Temperatura del humo

·        Presión ejercida al fumar y al purgar (puede servir para comparar las purgas de varias cachimbas y demás)

·        Información acerca del tabaco, cachimba, cazoleta, etc usada en la sesión (introducida por el usuario)**

## Pantallas

- **Pantalla de Inicio (Login)**
    
    - **Contenido:**
        - Campos de texto para ingresar correo y contraseña.
        - Botones para iniciar sesión, crear una nueva cuenta y recuperar contraseña.
    - **Diseño Propuesto:**
        - Un formulario con un `Column` para los campos de texto y botones.
        - Un `FlatButton` o `TextButton` para los enlaces a crear cuenta y recuperar contraseña.
- **Pantalla Principal**
    
    - **Bienvenida y Resumen Rápido:**
    
    - Texto de bienvenida personalizado con el nombre del usuario.
    - Resumen rápido de la última sesión (duración, número de caladas, temperatura media, etc.).
	- **Accesos Directos a Funciones Principales:**
    
	    - Botones o tarjetas para acceder rápidamente a:
        - Comenzar una nueva sesión de cachimba.
        - Ver estadísticas y gráficas detalladas.
        - Gestión de la conexión Bluetooth.
        - Configuración de la cuenta.
	- **Gráfica Resumen:**
    
	    - Una gráfica o un widget que muestre un resumen de las estadísticas de las últimas sesiones, como un gráfico de líneas o un histograma.
	- **Información Adicional o Noticias:**
    
	    - Espacio para mostrar noticias o actualizaciones sobre la aplicación, consejos sobre el uso de cachimbas, etc.

	- Diseño Propuesto:

	- Utiliza un `SingleChildScrollView` con un `Column` para organizar los diferentes widgets verticalmente.
	- Utiliza `Card` widgets para los resúmenes y accesos directos para que sean visualmente atractivos y accesibles.
	- **Menú**
    
	    - **Contenido:**
	        - Enlaces a:
	            - Ver estadísticas y gráficas.
	            - Gestión de la conexión Bluetooth.
	            - Comenzar una nueva sesión de cachimba.
	            - Ranking de usuarios.
	            - Cerrar sesión.
	    - **Diseño Propuesto:**
	        - Un `Drawer` con una lista de `ListTile` para cada opción del menú.
- **Gestión de la Conexión Bluetooth**
    
    - **Contenido:**
        - Botón para escanear dispositivos Bluetooth.
        - Lista de dispositivos encontrados.
        - Estado de la conexión actual.
    - **Diseño Propuesto:**
        - Un `ListView` para mostrar los dispositivos encontrados.
        - Botones para conectar y desconectar.
        - Indicadores de estado de la conexión usando `Icon` y `Text`.
- **Comenzar Nueva Sesión de Cachimba**
    
    - **Contenido:**
        - Botón para iniciar, pausar y parar la sesión.
        - Información en tiempo real como:
            - Duración de la sesión.
            - Número de caladas.
            - Tiempo de la última calada.
            - Tiempo medio de calada.
            - Temperatura del humo.
            - Presión ejercida.
    - **Diseño Propuesto:**
        - Un `Column` para organizar los datos en tiempo real.
        - Botones de control en la parte inferior usando `FloatingActionButton`.
- **Estadísticas y Gráficas de las Sesiones Individuales**
    
    - **Contenido:**
        - Gráficas que representan:
            - Duración media de las sesiones.
            - Caladas medias de las sesiones.
            - Temperatura media de las sesiones.
            - Otros datos relevantes.
    - **Diseño Propuesto:**
        - Un `ListView` o `SingleChildScrollView` con múltiples `Card` widgets.
        - Utilización de widgets como `LineChart`, `BarChart` para las gráficas.
- **Ranking de Usuarios Global**
    
    - **Contenido:**
        - Estadísticas agrupadas por:
            - Duración de la sesión.
            - Marca de cachimba, cazoleta, tabaco, etc.
            - Número de caladas.
            - Tiempo medio de caladas.
    - **Diseño Propuesto:**
        - Un `ListView` con `ListTile` o `Card` widgets para cada categoría del ranking.
        - Gráficas o tablas para visualizar los datos de manera clara.