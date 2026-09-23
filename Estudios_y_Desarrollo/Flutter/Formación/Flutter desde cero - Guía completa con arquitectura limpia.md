Código del curso (no se si es exactamente el mismo): https://github.com/darwin-morocho/flutter-desde-cero-2022/tree/master
## Sección 11: El contexto en flutter
### 229. Extensiones - parte 1 
En esta clase explica cómo utilizar las extensiones, que sirven para añadir métodos y funciones a clases ya definidas como las del sdk de flutter. Por ejemplo, podemos hacer una extensión para que cuando tengamos un objeto del tipo BuildContext, devuelva directamente las dimensiones de la pantalla.

### 231. addPostFrameCallback - parte 1
Aquí explica cómo hacer para ejecutar código en el initState() de un statefullWidget y que no pete porque no se haya cargado aún el widget por primera vez.
Resumidamente, se hace con:
WdigetsBinding.instance.addPostFrameCallback((_){código a ejecutar});

### 232. addPostFrameCallback - parte 2
Explica cómo hacer que siempre que se termine de cargar un statefullWidget (o el tipo donde se hace el mixin) se realice alguna acción (en este caso, mostrar la pantalla de carga). Lo que hace es crear un mixin y la pantalla que quiera utilizarlo, tendrá el onInit ya implementado. MUY ÚTIL.

### 234. GlobalKey - dimensiones de un widget
En esta clase explica cómo utilizar los key de los widgets y además cómo utilizarlo adecuadamente gracias al mixin de la clase 232, para que no podamos acceder al valor del key antes de que el widget se haya construido. En esta clase lo ha utilizado para saber el tamaño que ocupa el widget en la pantalla. En la siguiente clase lo utiliza para saber la posición del widget en pantalla.

## Sección 12: Inherited widgets
### 237. Planteamiento del problema - parte 1
Explica por qué utilizar inherited wdigets. El problema es básicamente que tengamos una propiedad por ejemplo "counter" que se le pase a un widget hijo, que a su vez se lo tenga que pasar a un widget nieto, etc etc.

### 239. Inherited Widgets - parte 1
En esta clase ha explicado cómo utilizar los inherited widgets. Básicamente nos tenemos que crear una clase que extienda de InheritedWidget. Definimos las propiedades que queremos que los hijos escuche, las pasamos en el constructor junto la Key? key, y por último, pasamos en el constructor un required Widget child. Al super le pasamos la key y el child. Tenemos que sobreescribir el método updateShouldNotify.
Luego para acceder a estos parámetros podemos hacer un método estático dentro del inherited widget que reciba el context como parámetro y haga return context.dependOnInheritedWidgetOfExactType\<NuestroWidgetInherit\>()!;

Luego en el widget hijo que queramos, podemos utilizar el método estático para acceder a las propiedades del inherited widget.

### 240. Inherited Widgets - parte 2
Está explicando por qué no se puede utilizar lo de arriba en widgets que no son padress del inherited widget. 
También ha utilizado por primera vez los assert(), que lo que hace es no seguir el código si no se cumple la condición que lleva dentro de los paréntesis.

### 242. Inherited Widgets - parte 4
Explica la importancia de añadir una validación en updateShouldNotify del inherited Widget, para no redibujar innecesariamente widgets hijos. Solo se redibujaran en caso de que se cumpla la condición (si devuelve un true), por lo que se puede añadir por ejemplo la validación de que la variable que estamos pasando haya cambiado (por ejemplo: return oldWidget.counter != counter || oldWidget.color != color).

### 244. Custom state management - parte 1
Comienza a hacer un Provider desde cero. 

### 246. Custom state management - parte 3
En el minuto 9:49 ha explicado la función didChangeDepenencies() que es una función del State de un Statefull Widget. Esa función se ejecuta justo después del initState() y justo antes del build(). La diferencia con el initState es que ya tenemos el contexto disponible, para solucionar problemas en los que necesitemos instanciar algo antes de que se cargue el widget pero dependa del contexto.
El problema es que esa función se puede llamar varias veces, como el build. Por lo tanto, si queremos inicializar algo solo al inicio del widget, creamos un booleano para controlar que sea la primera vez que se carga el widget y listo. Para ello, lo inicializamos en false y ponemos en un if que sea false para que se ejecute el código que queramos (esto ya dentro del didChangeDependencies). Cuando entre en ese if, lo seteamos a true y listo.
## Sección 15: Clean Architecture
En esta sección va a crear la aplicación de movie.db explicando conceptos de clean architecture. Utilizará el patrón repositorio y las capas de datos, dominio y presentación.
### 263. Estructura del proyecto
Aquí muestra todo el árbol de carpetas que utilizará.
### 267. Repository Pattern - parte 2
En el min 6:18 aprox explica cómo utilizar código asíncrono al inicio de un widget. (útil para cuando haya que cargar datos de la api).
En esta clase explica cómo conectar la capa de presentación con la de dominio pero utilizando la de datos, lo cual no es una arquitectura limpia. En la siguiente clase explicará cómo conectar la capa de presentación solo con la capa de dominio y utilizando los datos.
### 269. Repository Pattern - parte 4
Min 5:22 --> Explica que si queremos viajar a otra pantalla de la aplicación por ejemplo con pushReplacementName, si lo intentamos hacer después de una llamada a una función asíncrona que lleva el await, nos dará un error el vscode. Para arreglarlo, envolver lo el pushRepl... dentro de un if(mounted) , que nos indica que el widget sigue renderizado.

### 270. Comprobar el acceso a internet - parte 1
dependencia:
connectivity_plus 3.0.2
Para poder hacer código testeable, tenemos que inyectar las dependencias en la clase donde la vayamos a utilizar, no debemos inicializarlas en la propia clase. Por ejemplo, si en una clase voy a utilizar el repositorio Connectivity(), no puedo inicializarlo en la propia clase, si no que deberá venir instanciado pasado por el constructor y guardado en un parámetro de la clase.
El código de cómo comprobar si tenemos acceso a internet en flutter se muestra justo al final del vídeo en la pantalla. Pero solo es para móviles. Para página web se explica en la siguiente clase.

### 271. Comprobar el acceso a internet - parte 2
Para que sea testeable, si tenemos algo que no podemos sustituir por una prueba, hay que sacarlo a una clase aparte e inyectarlo como dependencia. (min 4:30 aprox).
Cómo saber que flutter está corriendo en web --> Importamos foundation.dart del paquete del sdk de flutter, y utilizamos la propiedad kIsWeb (es una propiedad booleana sin más). Ej: if(kIsWeb)

### 272. flutter_secure_storage
Es una dependencia que se utiliza para guardar información en la memoria del dispositivo pero de manera segura. Eso último es lo que lo diferencia de la dependencia shared_preferences. Ha dejado claro que con shared_preferences no se debe guardar información sensible con tokens de inicio de sesión etc.
Para utilizar flutter_secure_storage hay que hacer ciertas configuraciones para cada plataforma (android, ios, macos etc). Por ejemplo para android, hay que subir el minsdk al 23 y quitar el autobackup. Ver vídeo mejor si quiero utlizar esta librería.

### 273. Comprobando una sesión activa en el dispositivo
En esta clase ha enseñado cómo utilizar el flutter_secure_storage para guardar una key de sesión.
### 277. Modulo de inicio de sesión - parte 1
(min 5:00 aprox) --> Explica cómo hacer que el teclado desaparezca cuando pulsamos fuera de los textFormField, para que se oculte el teclado sin tener que darle al botón atrás.
Para ello símplemente, en el widget padre de todos (MyApp por ejemplo), lo envuelve en un GestureDetector y en el onTap del GestureDetector, pone: FocusManager.instance.primaryFocus?.unfocus();
### 279. Modulo de inicio de sesión - parte 3
Muestra el flujo entero de validación del formulario de inicio de sesión, poner un spinner mientras cargan los datos y demás.
### 281. Authentication API - postman
Aquí explica como usar postman como un pro, utilizando variables y tal y guardando las llamadas a la api.
### 282. Authentication API - parte 1
Por qué no guardar las API keys en el código del cliente (en flutter vaya) --> https://codewithandrea.com/articles/flutter-api-keys-dart-define-env-files/
### 287. La clase Http - parte 1
En estas clases explica cómo hacer una clase genérica en flutter para cualqueir petición que queramos hacer a cualquier api. Muy interesante.
### 292. La clase Http - parte 6
Ha explicado cómo añadir logs a la aplicación.
### 293. La clase Http - parte 7
Lo único interesante es que ha separado en ficheros algunas funciones y demás, pero solo para que sea más legible. Para no tener que importar de nuevo todo, ha usado part of ('http.dart'); en el fichero nuevo y part (failure.dart) en http.dart, para decir que es como si perteneciera al mismo fichero.
### 296. context.read
Cuando vayamos a usar un Provider, en vez de tener que hacer:
final connectivityRepository = Provider.of\<ConnectivityRepository>(context, listen = false);
Podemos hacer:
final connectivityRepository = context.read\<ConnectivityRepository>(); 
ó
final ConnectivityRepository connectivityRepository = context.read();
Ojo, el context.read inicializa el listen a false.
### 298. Lazy initialization
Ha explicado el lazy initialization del Provider. Por defecto es true, así que solo se creará la instancia del provider cuando se necesite en la aplicación. Si necesitamos la instancia antes por cualquier cosa, hay un parámetro al inicializar el provider llamado lazy, que habrá que poner a false.
### 299. ChangeNotifier - parte 1
Crea el controlador de la vista Sign_in , para gestionar los datos de la vista a parte. Utiliza un ChangeNotifier para actualizar la vista. Utiliza notifiListeners para ello.
### 300. ChangeNotifier - parte 2
En vez de provider utiliza changenotifierprovider, para escuchar los cambios. Envuelve toda la vista con ese widget, y en el create devuelve un SignInController (que extendía de ChangeNotifier). Al actualizar el usuario y el password, lo hace a través del controlador.
### 301. ChangeNotifier - parte 3
Explica cómo funciona el parámetro listen del ChangeNotifierProvider. Explica cómo solo se vuelve a pintar el formulario y no toda la pantalla. 
Si no se pone el notifyListeners, no se actualizará la vista aunque esté el listen a true.
También explica por qué no se puede poner el listen a true en una función o método que esté fuera del widget, ya que no se puede repintar algo que esté fuera del árbol de widget.
 
### 302. ChangeNotifier - parte 4
En las partes de ChangeNotifier, explica cómo podemos utilizar el provider para notificar los cambios. Con eso ya incluso no tenemos por qué utilizar un StateFull wdiget, y podemos utilizar un stateLess utilizando el change Notifier.
Refactoriza el código y saca el código del botón de login a un widget a parte. (Lo pone en la carpeta widgets de la vista).
En esta clase en concreto, enseña también cómo solucionar el problema de que no podemos utilizar el parámetro mounted para saber si el widget está todavía renderizado. Para ello símplemente en el controlador del ChangeNotifier, crea un booleano inicializado a true, y sobreescribe el método dispose(). Ahí lo pone a false y luego expone la propiedad con un getter.
### 303. Estado inmutables - parte 1
Empieza a explicar cómo gestionar el estado en una pantalla. Para ello todas las propiedades del ChangeNotifier las mueve a una nueva clase SignInState. Las propiedades serán final (inmutables). Cuando se quiera cambiar alguna de esas propiedades (cambiar el estado dicho de otra forma), no podremos hacerlo directamente porque son final. Para solucionarlo, tendríamos que declarar otra instancia nueva de SignInState. Para hcer eso, ha hecho una función que ha llamado copyWith, donde si cambia algún parámetro, lo reemplaza y devuelve una nueva instancia.
Entonces en la vista, habrá que utilizar la instancia del estado controlado en el controlador de la vista y luego acceder a la propiedad que queramos. 
Es importante que para gestionar clases que representan estados inmutables, podamos comparar si dos estados son iguales o no. Para nosotros, dos estados serán iguales si sus propiedades son iguales. Eso lo hace en la parte 2.
### 303. Estado inmutables - parte 2
Para sobreescribir el operador de igualdad:

@override
bool operator \==(Objetct other){
	if (other is SignInState){
		return this.username \== other.username && this.password \== other.password ...-resto de parametros de la clase--- ;
	}else return false;
}
Además también hay que sobreescribir el hash de las propiedades.

Pero todo esto lo hace ya un package: equatable

Para utilizarlo, tenemos que hacer que la clase que represente el estado (o cualquier otra que queramos poder comparar) extienda de Equatable, y tendremos que sobreescribir el método props. Tenemos que devolver una lista con las propiedades que queramos que compare para decidir si una clase es igual a otra o no. Si alguna de las propiedades no son tipos primitivos, también tendremos que extender esa otra propiedad de Equatable y definir a la vez el props.

Por lo visto, hay una mejor forma de hacerlo que con el equatable xd. Lo explicará en próximos vídeos.
### 303. Estado inmutables - parte 2
En esta clase hace la clase StateNotifier que se encargará de actualizar el estado. Con esto concentra la lógica del notifyListener  y la actualización del estado en esta clase, además de gestionar el estado anterior.

### 307. Postman - get user account
Cuando tenemos llamadas a la api que dependen del resultado de otras anteriores, tenemos que ir copiando y pegando respuestas entre llamadas. Para eso, se puede crear una variable que, programada con javascript en postman, se auto configure. Eso es lo que explica en esta clase.
### 308. Get User Account API - parte 1
Aquí (y en la siguiente clase ) explica cómo hacer una nueva llamada a otra api de TMDB. Me podría servir de ejemplo para mi proyecto.
### 314. Code generation - parte 2
En Code generation va a explicar como autogenerar código en dart. Esto se usa por ejemplo, cuando tengo un modelo User y tiene varias propiedades. Tendríamos que ir escribiendo cada propiedad una por una y obteniendola del json y haciendo validaciones y demás. Con el código autogenerado, se hace 'solo'.
Para ello utiliza el package [[build_runner]] y el [[json_serializable]].
Esto puede ser MUY ÚTIL.
### 315. Code generation - parte 3
Por defecto el generador de código asigna el mismo nombre de la variable del modelo a la propiedad del json. Para tener nombres distintos, justo antes de la propiedad, poner:
@JsonKey(name: 'nombrequequiera').
Explica también cómo hacer que el comando que genrea los ficheros ( flutter pub run build_runner build ) 
Es importante meter en el .gitignore todos los archivos de código autogenerado. En este caso tendríamos que añadir
\*.g.dart
### 316. vscode - command runner
Explica cómo hacer un atajo a comandos que ejecutamos en la consola.
### 318. Code generation - parte 4
Aquí explica cómo hacer con código generado, cuando una propiedad que viene en un json es hija de otra propiedad del mismo json.
Aquí modifica también el build.yaml que definía dónde tenía que ir build_runner a buscar los archivos a generar.

### 319. Freezed - parte 1
Explica cómo utilizar [[freezed]] para autogenerar código para la clase state del signin.
### 320. Freezed - parte 2
Explica cómo optimizar el uso de [[build_runner]].
### 323. Union Types
Un Union Type es cuando queremos pasar o un tipo u otro a una función. Por ejemplo si no sabemos si un Failure será un Unknow, o un NotFound o un Unauthorized.
En esta clase explica cómo hacerlo con clases y demás.
Para ello crea una clase abstracta principal (SignInFailure) y luego distintas clases específicas (Unknow, NotFound, Unauthorized, etc) que extienden de la principal.
Esto tiene el problema de que si queremos comparar o saber de qué tipo es algo retornado, tendremos que hacer ifs anidados diciendo... if propiedadX is NotFound entonces ... if propiedadX is Unauthorized entonces... . Para poder hacer todo eso con programación funcional, se puede utilizar [[freezed]].
### 324. Freezed - parte 5
Explica cómo pasar de la implementación anterior con clases abstractas a freezed, y cómo utilizar la programación funcional en vez de los ifs anidados con el when.
### 325. Freezed - parte 6
Explica cómo utilizar las clases con el factoryConstructor de freezed.
### 326. Freezed - parte 7
Explica cómo poner la clase either con el freezed, y cómo funciona el paso de argumentos en los factoryConstructors de freezed. (Esto lo explica casi al final).
### 330. Trending API - parte 3
Ha dicho una extensión de VSCode que te monta automaticamente el freezed: Flutter freezed Helpers.
Una vez instalada, con el snipet frf te crea el freezed.
### 331. FutureBuilder - parte 1
Explica el funcionamiento del widget FutureBuilder.
### 332. FutureBuilder - parte 2
Explica cómo:
Cómo manejar si ha habido un error en el future de FutureBuilder.
Cómo manejar que el future pueda retornar un dato null.
Cómo usar el FutureBuilder para hacer la llamada a la api.
Ha tenido un problema con los logs que tiene. Era porque no era results. También intentaba asignar un List\<dynamic\> a otro tipo de lista. Puede ser útil. (sobre min 10)
Finalmente se dio cuenta de que a veces la api retornaba un nombre de json y otras veces otras. Esto lo solucionará en el siguiente vídeo.
Al final de esta clase ya muestra por pantalla la info de la api por pantalla.
### 333. json serializable - readValue
readValue es una función que se puede utilizar con la anotación JsonKey de jsonSerializable . Esto se usa cuando no sabemos a priori qué nombre va a traer el json  de la api que queremos leer en esa propiedad.
Quita los logs también, comentando \_printLogs de http.dart
### 334. FutureBuilder - parte 3
Explica:
Cómo evitar que cada vez que repinte la vista, se llame a la api por el FutureBuilder. Para ello, primero lo pasa a StatefulWidget, y en el initState es donde lee el repositorio y luego guarda en una variable de tipo Future el future. Ese será el que le pase luego al FutureBuilder:
![[Pasted image 20240520105833.png]]
De todos modos, por lo que veo este no será el modo en que lo haga finalmente. (No se en qué clase lo cambia aún).
Termina mostrando una lista con las imágenes de las películas, utilizando un ListView.separated. La información en todo momento estará en la propia vista con el snapshot.data del FutureBuilder.
### 335. Trending UI - parte 1
Está dando diseño a la lista de películas. Ha hecho:
- Poner paddings en la lista ListView.separated . 
- Redondear las imágenes con ClipRRect
- Mostrar encima de la imagen la votación con un Stack y un Positioned.fill
- Ajustar el ancho de las imágenes a la pantalla, utilizando el widget AspectRatio. Para obtener el aspectRatio, utiliza como child un LayoutBuilder, y luego obtiene el máximo ancho del AspectRatio con el segundo parámetro del builder de LayoutBuilder .maxHeight. Lo multiplica por 0.75 para calcular una altura variable para las imágenes.
- Utilizar widget Chip para envolver la puntuación de las películas.
- Refactoriza el código. Saca todo el ClipRRect que contiene la imagen a un widget a parte que le ha llamado TrendingTile.
### 336. Trending UI - parte 2
Ha hecho:
- DropdownButton para seleccionar la temporalidad de las películas. (Tendrá que volver a llamar a la api).
- Se ha encontrado con el problema de que tendría que volver a llamar a la api, por lo que inicializa el repositorio con un getter y lo quita del initState. En el onChanged del DrodownButton es donde vuelve a hacer la llamada a la api con el repositorio.
- Separar al máximo (uno en cada extremo) los elementos de un Row.
- Usar el key del FutureBuilder para que vuelva a mostrar el circularprogressindicator cuando vuelve a llamar a la api.
### 337. Trending UI - parte 3
Ha hecho:
- Cambiar color al DropdownButton
- Quitar la línea que subraya el título del DropdownButton con underline: const SizedBox()
- Quitar el padding por defecto del dropdownbutton
- Separación entre el widget y el final de la pantalla, símplemente con un SizedBox
- Refactoriza y saca el DropdownButton a otro widget.
### 338. Enums con json serializable
Cuando sabemos de antemano los tipos de string que nos puede devolver la api, mejor trabajar como enums.
Aquí explica cómo pasar el string que devuelve la api a un enum con JsonSerializable. Se hace básicamente metiendo la anotación @JsonValue en el propio enum en cada valor del enum.
### 339. Performers - parte 1 (actores y actrices de TMDB)
Explica:
- Si en el modelo de datos que utiliza JsonSerializable tengo una propiedad que es una lista de alguna otra clase, como por ejemplo List\<Media\> , tengo que asegurarme de que Media soporte JsonSerializable (que extienda y demás). Lo mismo si tengo una propiedad de tipo Media directamente.
- Aplica una función fromJson distinta a la generada por defecto en la anotación JsonKey
- Ha pasado la lógica de crear una lista de Medias a la clase Media, con los filtros correspondientes.
### 340. Performers - parte 2
- Hace la llamada a la api de performers como lo hizo con trending ui.
- Ha tenido el error:
type List\<dynamic\> is not a subtype of type 'List\<Map\<String, dynamic>>' in type cast . Para solucionarlo, cuando pasa la Lista, no le especifica de qué tipo es.
- También ha tenido un error porque le ha llegado un objeto del json con una propiedad vacía. Para ello, lo ha incluido en el getPerformers en el where , filtrando así el no mostrar los actores que tengan null en esa propiedad.
### 341. Performers - parte 3
- Cómo evitar que salga un espacio en blanco hasta que carga la imagen de la película. --> Extended_image (de pub.dev) . Esto creo que se podría hacer con del otro curso de flutter. Este widget hace que se guarden en caché.
- Pinta una lista horizontal con los actores . Para ello en el FutureBuilder, utiliza el snapshot.data.when.
### 342. Performers - parte 4
- Cómo hacer que el scroll horizontal deje en medio de la vista el elemento. --> PageView , PageView.builder
- Explica que cuando la lista a renderizar es muy grande, es mejor utilizar el factoryConstructor .builder.
- Meter texto en un container con gradiente de color. --> gradient: LinearGradient en el BoxDecoration del Container
- Separa las movies de los performers en distintas carpetas
- Refactoriza y saca el PerformerTile en un fichero llamado performer_tile.dart
### 343. Performers - parte 5
- Explica que el PageView necesita de alguna forma que la altura del widget que lo contiene esté definido, como un Expanded
- Cómo mostrar un poco del siguiente Page. Para ello, se usa la propiedad controller del PageView.
- Importante llamar al .dispose del pageController.
- Cómo mostrar el número de la Page que estamos viendo y el total de Pages que hay. --> AnimatedBuilder
### 344. Performers - parte 6
- Como mostrar el número de la Page con una lista de círculos. Para ello lo pone en un stack
- Mostrar las películas de cada actor
- .take programación funcional para coger solo x elementos de una lista
### 345. Widget para mostrar en caso de solicitudes fallidas
Para que se pueda recargar la llamada a la api cuando falle.
- Explica lo que es el tipo de dato VoidCallback. (Creo que es una función sin parámetros y que no devuelve nada)
- Crea el RequestFailed, del archivo request_failed.dart
- Crea una función \_updateFuture para refactorizar el código que llama a la api
- Este RequestFailed lo llama cuando snapshot.data devuelva un error.
### 346. flutter_get - parte 1
EN ESTAS PARTES EXPLICA CÓMO TRABAJAR CON ASSETS EN FLUTTER
Es un repositorio de código generado para manejar las rutas de imágenes y demás, para no tener que poner la ruta por ejemplo: 'assets/images/xxx/xx...'
- unDraw --> Sitio para descargar assets
- Crea la carpeta assets/images para utilizar los assets
- Referencia la imagen en pubspec.yaml
- Muestra la imagen de error en request_failed.dart con el código autogenerado; Assets.images.error404.path
- Cada vez que se incluya un nuevo archivo o imagen o lo que sea tenemos que ejecutar el autogenerado del código.
- .image() te devuelve directamente la imagen
### 347. flutter_get - parte 2
Explica cómo trabajar con imagenes svg .
- Utiliza el paquete flutter_svg de pub.dev
- Explica por qué utiliza flutter pub run flutter_gen:flutter_gen_command (no he echado mucha cuenta)
### 348. El widget RefreshIndicator
Es el widget usado para recargar cuando deslizamos hacia abajo.
onRefresh es la propiedad del RefreshIndicator que se ejecutará cuando tiremos hacia abajo.
Explica también cómo utilizarlo con el SingleChildScrollView.
También explica que no se puede utilizar el Expanded con el SingleChildScrollView si no definimos alguna altura, como por ejemplo un SizedBox.
Calcula la altura con LayoutBuilder para calcular la altura de la pantalla que está devolviendo el SafeArea, pero con esto no se puede deslizar para abajo con el **RefreshIndicator porque necesita que haya algún scroll. Para ello, utiliza physics: AlwaysScrollableScrollPhysics.**
### 349. Refrescar contenido de la vista home - parte 1
- Primero explica cómo se refresca la pantalla con el onRefresh del RefreshIndicator. (Hasta min 2:45)
- Empieza a **crear el estado de la pantalla home**.
	- Para ello crea primero HomeController que extiende de StateNotifier
	- Crea el archivo home_state.dart. Dentro, class HomeState que utilizará freezed. Lo crea con el snippet frf. Quita el autogenerado del json serializable porque no lo va a utilizar
	- Crea las propiedades que necesita: bool loading, List\<Media> .
	- Genera el código
	- Se va a HomeController y le dice a StateNotifier que va a trabajar con el HomeState entre \<>
	- Envuelve el widget padre de HomeView con ChangeNotifierProvider\<HomeController>
	- Crea el constructor por defecto que **contendrá el state**
	- En el create: \(\_)=> HomeController
	- En el constructor de HomeController le pasa el loading a true
	- Crea en el HomeController un método que devuelve un Future\<void> init() async () donde **llamaremos a la API en cuanto se renderice la vista**. (Sigue en el siguiente video)
### 350. Refrescar contenido de la visto home - parte 2
- 
	-  Le mete a la HomeController una propiedad de tipo repositorio para llamar a la API. La mete en el constructor como required (inyección de dependencias)
	- Por tanto en el home_view.dart, cuando la llamaba, tiene que pasarle el trendingRepository. **Como trendingRepository ya estaba inyectado en el main, se puede hacer con un context.Read()**.
	- En el init de HomeController, hace la llamada a la API a través del repositorio, pero necesitamos pasarle un parámetro a esa llamada, por tanto...
	- Mete esa propiedad en el HomeState (la propiedad TimeWindow), en el constructor como las otras porque va con Freezed. Le pasa un valor por defecto con la anotación @Default(defaultValue). Como le pasa un valor por defecto, al crear la instancia de HomeState en el HomeView en el constructor del HomeController no hay que pasarle este parámetro
	- Ahora, en el HomeController, en la llamada a la api, le dice que debe utilizar el valor del state. Para ello, state.timeWindow (tenemos el state en el constructor).
	- Ya gestiona el resultado de la llamada a la api asignando el resultado a un await result = ...
	- Le hace el result.when
	- En el left hace state = state.copyWith y le pasa el loading a false y un null a moviesAndSeries
	- En el right hace también un copyWith con el loading a false pero le pasa a moviesAndSeries el resultado del right
	- Ahora tiene que hacer que cuando se muestre la HomeView, se llame al init del HomeController.
		- Para ello, en el create del ChangeNotifierProvider, primero no devuelve directamente el HomeController si no que lo guarda en una variable final controller.
		- Luego llama a controller.init 
		- Devuelve la variable controller.
	- Convierte el TrendingList (widget de dentro del HomeView) a un StateLessWidget porque **el estado lo controlará el ChangeNotifierProvider**
		- Crea el constructor por defecto con el key y yastá
		- Quita el código que inicializaba el repositorio
		- Quita el initState que llamaba hacía la llamada a la api
		- Quita el dispose
		- Quita la función que actualizaba el estado cuando se llamaba a la api
		- Escuchar los cambios del HomeController --> Justo después del build hace **context.watch** que es lo mismo context.read pero para escuchar los cambios y recuperar el HomeController.
		- Donde tenía timeWindow, llama a controller.state.timeWindow.
		- Elimina el futureBuilder porque ya no tiene que hacer la llamada a la api (ya está hecha)
		- Si controller.state.loading muestra un CircularProgressIndicator
		- Si controller.state.moviesAndSeries (lo que recupera de la api) es nulo, es porque ha fallado y muestra el RequestFailed
		- Y si no, ya muestra el ListView.separated (o el widget que vaya a utilizar los datos del estado)
		- Sin embargo, en el próximo vídeo explicará como hacer esto con programación funcional.
### 351. Estados inmutables con programación funcional - parte 1
Va a mejorar el código donde utiliza el HomeController (TrendingList)
- Primero guarda el state en una variable en el build (final state = controller.state;)
- Quita el operador ternario que evaluaba las propiedades del estado.
- Hace la siguiente lógica: En vez de tener un único HomeState con las tres propiedades que tiene, evalúo qué estados puedo tener: Que el Estado sea el de cargando, un estado fallido porque moviesAndSeries sea null, o que esté cargado correctamente. Entonces a través de union types y sealed classes va a crear tres tipos de clase State: HomeStateLoading, HomeStateFailed y HomeStateLoaded.
- Para ello, crea un factory constructor con cada una de ellas. (min 4:40).
- En el HomeState.loading le pasa required TimeWindow y el List\<media> moviesAndSeries SIN QUE PUEDA SER NULO, pues si estamos en el estado de loaded, es porque la llamada ha salido bien.
- Genera el código
- En la función init() del HomeController, en el left y en el right ya no utiliza el copyWith, si no que hace state=HomeState.xxx y el estado al que se quiera ir.
- Min 7:20 --> **Explica cómo tener una propiedad en común para todos los estados, como timeWindow. De esa forma se puede acceder a ella a través de state.timeWindow**. Básicamente, se le pasa la misma propieda con el mismo nombre a todos los factory constructors de HomeState. Si se quieren más propiedades comunes, se ponen con el mismo nombre y ya. De este modo, **las propiedades que necesite usar para hacer la llamada de la api serán comunes (normalmente, o esa es mi lógica) y las que devuelva la api, serán específicas del tipo de estado que lo necesite.**
- En TrendingList, ya puede utilizar programación funcional. Hace state.when y define una función para cada tipo de estado.
- En el create del ChangeNotifierProvider, ya no devolveremos una instancia de HomeState asecas, si no que devovleremos una instancia de HomeState.loading() pasándole el timeWindow (que ya podremos utilizarlo en el resto de States), ya que es el estado "inicial" de nuestra vista.
### 352. Estados inmutables con programación funcional - parte 2
Va a añadir al estado los actores.
Mete la propiedad en el estado, genera el código, añade en el init de HomeController la llamada a la API para traerse los performers (actores), y **en el mismo right** del result de las movies, mete las validaciones de los actores. --> Esto lo corrige en la siguiente clase.
Luego hace el resto de cosas igual que para las moviesAndSeries pero en el widget trendigPerformers. 
Ojo a que este sí tiene que ser StateFullWidget porque tiene que actualizar el número de página con el PageController (este controlador es propio de flutter, no lo hemos creado nosotros) así que retoma la instancia en la propia clase del State del StatefullWidget
- Además, mete en el onRefresh del HomeView el cómo recargar los datos de la api. Para ello símplemente llama al init del HomeController
Se ha dado cuenta de que necesita separar la lógica de las películas de la de los actores, porque con que falle una llamada a la api, no se mostrarán ninguna de las dos por pantalla. En el siguiente vídeo lo soluciona.
### 353. Estados inmutables con programación funcional - parte 3
En esta clase separará la lógica de las películas de los actores.
- Utiliza el snippet frc para crear solo la clase freezed sin los part ni las anotaciones de arriba, para cuando tenemos dos clases freezed en el mismo archivo y ya se había definido arriba.
- Para separar la lógica, crea otro estado llamado class PerformerState.
- Le crea los mismos estados, pero solo le pasa el List\<Performer>
- Le cambia el nombre de HomeState a MoviesAndSeriesState.
- Crea la HomeState con el factory normal y con dos propiedades: Una de tipo MoviesAndSeriesState y otra PerformersState
- A esas propiedades le da un valor por defecto: MoviesAndSeriesState.loading y lo mismo para performers. Para ello ha necesitado poner const a los factory constructor de las otras dos clases.
- Divide la función init en dos funciones, una para cada State: loadPerformers, que llama a la api y le hace el when. Y loadMoviesAndSeries que hace lo mismo pero para las pelis y series.
- En el init, llama a las dos funciones anteriores.
### 353. Estados inmutables con programación funcional - parte 4
Ahora va a devolver la funcionalidad al botón retry que recargaba solo una api, y el botón que cambiaba el timeWindow con el desplegable.
- En el HomeController, define una función onTimeWindowChanged que recibe el TimeWindow. Ahí comprueba que el timeWindow pasado sea distinto al state.moviesAndSeries.timeWindow y entonces hace una copia del estado pasándole el nuevo timeWindow --> **Esto se puede hacer así de sencillo porque timeWindow se comparte entre los tres tipos de MoviesAndSesriesState. En futuros vídeos explicará cómo hacerlo para un parámetro no compartido como sería la lista de moviesAndSeries*.*
- Pero todo eso, en vez de hacer el copyWith, se puede asignar directamente el MoviesAndSeriesState.loading()
