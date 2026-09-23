Este es el **Q&A Definitivo** diseñado para simular una entrevista técnica de alto nivel para un puesto de **Tech Lead** en el sector Defensa.

Aquí no hay respuestas de libro de texto. Hay **justificaciones arquitectónicas**, defensa de decisiones técnicas y análisis de trade-offs. Estudia esto para responder no como quien _sabe programar_, sino como quien _sabe decidir_.

---

# 🎙️ Q&A Técnico: Defensa del Proyecto "Tactical Command Dashboard"

## 🏗️ Bloque 1: Arquitectura y Estrategia (High Level)

**P1: ¿Por qué has elegido un Monorepo Nx para este proyecto en lugar de repositorios separados (Polyrepo)?**

> **Respuesta de Tech Lead:** En sistemas complejos como un C2 (Command and Control), la consistencia de los contratos de datos es vital. Un monorepo Nx me permite compartir los DTOs (interfaces TypeScript) entre el Front y el Back (si usamos BFF con Node) o mantener librerías de utilidades y estilos compartidos (`ui-kit`) sin la fricción de versionar paquetes NPM privados. Además, Nx nos da **Computation Caching**: si solo toco el CSS de un botón, el CI no recompila el Backend ni corre los tests de la API.

**P2: Veo que usaste "Vertical Slice Architecture" o Features en el Backend. ¿Por qué no una Clean Architecture clásica por capas (Controller -> Service -> Repository)?**

> **Respuesta:** La Clean Architecture tradicional ("Onion") tiende a dispersar la lógica de una sola funcionalidad en 4 o 5 proyectos/carpetas diferentes. En un entorno táctico donde necesitamos iterar rápido sobre una _Feature_ (ej: añadir "Nivel de Combustible" a la telemetría), la **Vertical Slice** mantiene todo lo relacionado con esa feature junto (Controller, DTOs, Lógica, Reglas de DB). Reduce el _Cognitive Load_ y el _Context Switching_ del desarrollador.

**P3: ¿Cuál es tu estrategia para migrar este monolito modular a Microservicios si el proyecto escala a nivel OTAN?**

> **Respuesta:** Gracias al diseño modular de Nx y al desacoplamiento en el Backend, la migración es natural.
> 
> 1. **Backend:** Como organizamos por _Features_ (Slices), puedo coger la carpeta `Features/Telemetry` y extraerla a su propio contenedor Docker/API sin desenredar "spaghetti code".
>     
> 2. **Frontend:** Nx permite convertir las librerías `libs/features` en **Micro-Frontends** desplegables independientemente usando _Module Federation_, orquestados por el `apps/dashboard` (Shell).
>     

---

## 🅰️ Bloque 2: Angular Moderno (Frontend Deep Dive)

**P4: ¿Por qué Signals? ¿Significa que RxJS está muerto?**

> **Respuesta:** No, RxJS no está muerto, pero su rol ha cambiado.
> 
> - **Signals** son para la **Sincronización de Estado** (State Synchronization). Son síncronos, libres de glitches y permiten una reactividad de grano fino. Si cambia un dato, solo se actualiza el nodo de texto exacto en el DOM, no todo el árbol de componentes. Esto es crucial para el rendimiento del Dashboard.
>     
> - **RxJS** sigue siendo el rey para **Eventos Asíncronos** (Streams). Lo uso para manejar la comunicación HTTP y WebSockets (`retry`, `debounce`, `switchMap`), pero una vez llega el dato, lo convierto a Signal para la vista.
>     
> - _Frase clave:_ "RxJS para el viaje (flujo), Signals para el destino (vista)."
>     

**P5: Has usado `OnPush` y `runOutsideAngular`. ¿Por qué tanta obsesión con el rendimiento?**

> **Respuesta:** En un dashboard militar, el mapa puede recibir 50 actualizaciones por segundo vía SignalR.
> 
> - Si usamos `Default Change Detection`, Angular revisaría todo el árbol de componentes 50 veces por segundo. La CPU se quemaría.
>     
> - Con **`OnPush`**, Angular ignora el componente a menos que cambie una entrada (Input Signal).
>     
> - Con **`runOutsideAngular`**, evito que los eventos internos de OpenLayers (miles de pixeles moviéndose) disparen el ciclo de detección de cambios de Angular. Solo "entro" en la zona (`ngZone.run`) cuando necesito actualizar un contador o una alerta visible en la UI.
>     

**P6: ¿Por qué evitas los `NgModules`?**

> **Respuesta:** Los `NgModules` añadían una capa de complejidad innecesaria y dificultaban el _Tree-shaking_ (eliminar código no usado). Con **Standalone Components**, cada componente declara explícitamente lo que necesita. Esto hace que los tests sean más fáciles (no hay que configurar módulos de prueba gigantes) y permite cargas perezosas (`Lazy Loading`) a nivel de componente individual, mejorando el _Core Web Vital_ de LCP (Largest Contentful Paint).

---

## ⚙️ Bloque 3: Backend .NET 8 & Performance

**P7: ¿Por qué usas `Records` para los DTOs en lugar de `Classes`?**

> **Respuesta:** Inmutabilidad y concurrencia. Un `record` en C# es inmutable por defecto y tiene igualdad por valor. En un entorno multihilo (como recibir miles de peticiones SignalR), saber que los datos no van a cambiar inesperadamente mientras los proceso evita condiciones de carrera. Además, la sintaxis es mucho más limpia (`public record Dto(int Id, string Name);`).

**P8: El simulador de batalla corre en el mismo proceso. ¿Qué pasa si tenemos 1 millón de unidades?**

> **Respuesta:** En la demo corre como un `BackgroundService` (IHostedService), lo cual está bien para < 5000 unidades. Para escalar a 1 millón:
> 
> 1. Extraería el simulador a una **Azure Function** o un contenedor separado (Worker Process).
>     
> 2. Usaría una cola de mensajes (como **RabbitMQ** o **Azure Service Bus**) para desacoplar la generación de datos de la ingestión.
>     
> 3. En SignalR, usaría **Redis Backplane**. Una sola instancia de servidor no puede mantener 1 millón de websockets abiertos. Redis permite tener 10 servidores web y distribuir los mensajes entre ellos transparentemente.
>     

**P9: ¿Por qué Entity Framework Core y no Dapper si buscas rendimiento?**

> **Respuesta:** EF Core 8 es extremadamente rápido, casi a la par de Dapper en consultas simples de lectura (`AsNoTracking`). Uso EF Core por la **Productividad** y seguridad de tipos que me dan las Migraciones y LINQ. _Estrategia Híbrida:_ Uso EF Core para el 95% de la aplicación (CRUD de misiones, usuarios). Si detecto un cuello de botella en la inserción masiva de telemetría, escribiría esa única consulta en SQL crudo o Dapper dentro del mismo proyecto. No hay que sacrificar mantenibilidad prematuramente.

---

## 🛡️ Bloque 4: Seguridad (Defensa & OTAN)

**P10: Guardas el JWT en LocalStorage. ¿No es eso vulnerable a XSS (Cross-Site Scripting)?**

> **Respuesta:** Tienes toda la razón. Para el curso/demo, LocalStorage es aceptable por facilidad. **En un entorno real de Defensa:**
> 
> 1. El token se guardaría en una **Cookie HttpOnly, Secure y SameSite=Strict**. Así, el JavaScript (y por tanto un atacante XSS) no puede leer el token, solo el navegador puede enviarlo.
>     
> 2. Implementaría **Short-lived Access Tokens** (ej: 5 min) y **Refresh Tokens** con rotación para minimizar la ventana de ataque si se compromete una sesión.
>     
> 3. Implementaría **CSP (Content Security Policy)** estricto para prevenir la ejecución de scripts no autorizados.
>     

**P11: ¿Cómo implementarías seguridad a nivel de fila (Row-Level Security)? Un comandante español no debe ver las unidades francesas a menos que estén en misión conjunta.**

> **Respuesta:**
> 
> 1. **En BBDD:** SQL Server soporta RLS nativo, pero es complejo de mantener.
>     
> 2. **En Aplicación (Global Query Filter de EF Core):** Definiría un `TenantId` o `CountryId` en las entidades. En el `DbContext`, inyectaría el servicio de usuario actual (`ICurrentUserService`) y aplicaría un filtro global:
>     

> C#
> 
> ```
> builder.Entity<TacticalUnit>().HasQueryFilter(u => u.CountryId == _currentUser.CountryId);
> ```
> 
> Así, incluso si el programador se olvida de poner el `Where`, el sistema nunca devuelve datos de otro país.

---

## 🗺️ Bloque 5: GIS (Geoespacial)

**P12: ¿Leaflet o OpenLayers? ¿Por qué elegiste OpenLayers?**

> **Respuesta:** Leaflet es fantástico para mapas sencillos (ej: Airbnb, Uber). Pero para Defensa:
> 
> 1. **Proyecciones:** Los militares usan **MGRS** (Military Grid Reference System). OpenLayers maneja reproyecciones complejas al vuelo mucho mejor.
>     
> 2. **Vector Rendering:** OpenLayers usa WebGL para renderizar capas vectoriales. Si necesito pintar 10,000 entidades moviéndose, Leaflet (basado en DOM/SVG) se arrastraría. OpenLayers aguanta la carga.
>     
> 3. **Offline:** OpenLayers tiene mejor soporte para fuentes de teselas cacheadas localmente (XYZ/PBF) para operaciones desconectadas.
>     

**P13: ¿Cómo manejas la "Clustering" (Agrupación) si haces zoom out y hay demasiados tanques juntos?**

> **Respuesta:** OpenLayers tiene una fuente `ol/source/Cluster`. En lugar de pasarle los datos crudos a la capa vectorial, se los paso al Cluster Source. Este agrupa puntos cercanos según la distancia en píxeles y el nivel de zoom. Visualmente, mostraría un círculo con un número ("15") en lugar de 15 iconos solapados. Al hacer clic, el mapa haría zoom (Spiderify) para dispersarlos.

---

### 💡 Consejo Final para tu Entrevista

Si te preguntan algo que no sabes, aplica el principio **"No lo sé, pero sé cómo mitigarlo"**.

_Ejemplo:_ "¿Cómo configurarías Kubernetes para esto?" _Respuesta:_ "Aunque mi foco ha sido la arquitectura de aplicación, sé que para este stack definiría `HealthChecks` en .NET para los `LivenessProbes` de K8s, y configuraría el `HorizontalPodAutoscaler` basándome en el uso de CPU o métricas custom de SignalR (conexiones activas), no solo memoria."



---
Entendido. Vamos a dejarnos de formalismos excesivos. Si pides un **"Q&A a prueba de balas"** (o como tú dices, _idiota_, en el sentido de que no haya lugar a error), es porque sabes que el diablo está en los detalles.

Aquí tienes la **Lista de Problemas, Dudas y Errores** con los que te vas a topar al 100% cuando intentes montar esto, explicados para que no te quedes bloqueado ni 5 minutos.

---

# 🆘 Q&A de Supervivencia: Todo lo que puede salir mal

Este no es un Q&A teórico. Es una lista de **errores de compilación y ejecución** que vas a tener.

### 🔹 BLOQUE 1: SETUP E INFRAESTRUCTURA (Nx & Docker)

**P: He ejecutado `docker-compose up` pero la base de datos no conecta.**

> **R:**
> 
> 1. **¿Tienes SQL Server instalado en local en tu PC?** Si es así, el puerto `1433` ya está ocupado. O apagas tu SQL local o cambias el puerto en el `docker-compose.yml` (ej: `"1434:1433"`) y actualizas la connection string (`Server=localhost,1434`).
>     
> 2. **Contraseña Débil:** SQL Server en Docker exige contraseñas complejas (Mayúscula + Minúscula + Número + Símbolo). Si pusiste "1234", el contenedor se inicia y se apaga inmediatamente (mira los logs de Docker).
>     

**P: Intento ejecutar comandos de `nx` y me dice `command not found`.**

> **R:** No has instalado Nx globalmente.
> 
> - **Opción A (Instalar):** `npm install -g nx`
>     
> - **Opción B (Usar npx):** Pon `npx` delante de todo. `npx nx g component...`
>     

**P: Nx me crea los componentes en carpetas que no quiero.**

> **R:** Nx es estricto. Si no especificas `--project`, se pierde.
> 
> - **Solución:** Usa siempre el flag `--project=nombre-de-libreria` o entra en la carpeta `libs/tu-lib` y ejecútalo desde ahí.
>     

---

### 🔹 BLOQUE 2: ANGULAR 18+ (El Frontend Moderno)

**P: Copié tu código del `AppConfig` pero me da error en `provideHttpClient`.**

> **R:** Estás importando desde el lugar incorrecto o te falta el import.
> 
> - Asegúrate de importar: `import { provideHttpClient, withFetch } from '@angular/common/http';`
>     
> - **OJO:** ¡Ya no se usa `HttpClientModule` en los `imports` de los componentes ni en el bootstrap! Se provee la función.
>     

**P: Me sale error en el `@if` o `@for`. El IDE me lo marca en rojo.**

> **R:**
> 
> 1. **Extensión Antigua:** Tu extensión "Angular Language Service" en VS Code es vieja. Actualízala.
>     
> 2. **Versión de Angular:** Revisa `package.json`. Si dice `"@angular/core": "16.x"`, no funcionará. Necesitas la 17 o 18.
>     

**P: ¿Por qué usas `inject(Servicio)` en lugar del `constructor(private servicio: Servicio)`?**

> **R:** Es lo mismo, pero `inject()` es más moderno y funcional.
> 
> - **Ventaja:** Puedes usarlo fuera de clases (en interceptores funcionales o guards).
>     
> - **Regla de Oro:** `inject()` solo funciona durante la fase de **creación** (inicialización de campos o constructor). No lo llames dentro de un `ngOnInit` o un método normal, fallará.
>     

**P: He creado un Signal `misDatos = signal([])` y al hacer `misDatos.push(item)` no se actualiza la vista.**

> **R:** ¡ERROR CLÁSICO! **Los Signals no detectan mutaciones internas**.
> 
> - **Mal:** `misDatos().push(item)` (Angular no se entera).
>     
> - **Bien:** `misDatos.update(lista => [...lista, item])`. Tienes que reemplazar la referencia o usar `.update()`.
>     

---

### 🔹 BLOQUE 3: BACKEND .NET 8 (API)

**P: Me da un error de CORS: `Access to XMLHttpRequest at ... from origin 'http://localhost:4200' has been blocked`.**

> **R:** El navegador protege al usuario. El Frontend (puerto 4200) y Backend (puerto 5001) son "orígenes distintos".
> 
> - **Solución:** Revisa en `Program.cs` que `.UseCors("AllowAngular")` está puesto **ENTRE** `app.UseRouting()` y `app.UseAuthorization()`. El orden de los Middlewares en .NET es sagrado. Si lo pones antes o después de tiempo, no funciona.
>     

**P: La base de datos me dice que no existe la tabla `Missions`.**

> **R:** Creaste el código, pero no aplicaste la migración.
> 
> 1. `dotnet ef migrations add Initial` (Crea el script C#).
>     
> 2. `dotnet ef database update` (Ejecuta el SQL en la BBDD).
>     

> - Si falla el comando `dotnet ef`, instala la herramienta: `dotnet tool install --global dotnet-ef`.
>     

**P: Swagger me funciona, pero Angular no recibe datos.**

> **R:** ¿Estás usando `https` en .NET y `http` en Angular?
> 
> - Los certificados SSL de desarrollo en local (`localhost`) suelen dar problemas de "Certificado no confiable".
>     
> - **Solución Rápida (Dev):** Usa `http://localhost:5000` en el `Program.cs` (`app.Run("http://localhost:5000")`) y apunta Angular ahí. Ahórrate el lío de certificados SSL locales si estás aprendiendo.
>     

---

### 🔹 BLOQUE 4: SIGNALR (Tiempo Real)

**P: SignalR conecta (veo el log), pero no recibo ningún mensaje.**

> **R:** Hay dos causas probables:
> 
> 1. **Typo en el String:** En el Backend pusiste `await Clients.All.SendAsync("ReceiveTelemetry", data)` y en el Frontend `hub.on("receiveTelemetry")`. **Es Case Sensitive** (distingue mayúsculas). Copia y pega el string exacto.
>     
> 2. **El Back no envía nada:** El `BattlefieldSimulator` (BackgroundService) podría haber fallado silenciosamente. Pon un `try-catch` dentro del bucle `while` del simulador y loguea el error.
>     

**P: La conexión se cae y no vuelve.**

> **R:** SignalR intenta reconectar, pero si el Backend se reinicia, a veces el ID de conexión cambia.
> 
> - Asegúrate de tener `.withAutomaticReconnect()` en el cliente Angular.
>     
> - Asegúrate de que CORS permite credenciales: `.AllowCredentials()`.
>     

---

### 🔹 BLOQUE 5: OPENLAYERS (Mapas)

**P: El mapa no aparece. La pantalla está en blanco. Cero errores en consola.**

> **R:** **EL ERROR #1 DE MAPAS.**
> 
> - OpenLayers inserta un `<canvas>`. Si el contenedor padre (`div`) tiene `height: 0` (que es el defecto de un div vacío), el mapa tiene altura 0.
>     
> - **Solución:** En tu CSS global o del componente:
>     
>     CSS
>     
>     ```
>     .map-container { height: 100vh; width: 100%; display: block; }
>     ```
>     
>     Asegúrate de que el padre del componente mapa también tiene altura.
>     

**P: El mapa se ve "roto", como fichas desordenadas.**

> **R:** No importaste el CSS de OpenLayers.
> 
> - Ve a `styles.scss` y añade: `@import 'ol/ol.css';`. Sin esto, las teselas se montan unas encima de otras.
>     

**P: Al cambiar de ruta y volver al mapa, el mapa desaparece o da error.**

> **R:** No estás limpiando la instancia.
> 
> - En `ngOnDestroy`, asegúrate de hacer `this.map.setTarget(null)`. Si no, OpenLayers intenta mantener una referencia a un elemento DOM que Angular ha destruido.
>     

---

### 🔹 BLOQUE 6: SEGURIDAD (JWT)

**P: Hago login, me da token, pero la siguiente petición da 401 Unauthorized.**

> **R:**
> 
> 1. **Interceptor:** ¿Registraste el interceptor en `app.config.ts`? Pon un `console.log('Interceptando...')` dentro del interceptor para ver si salta.
>     
> 2. **Formato:** El header debe ser exactamente `Authorization: Bearer eyJhbGc...`. Si falta el espacio o la palabra "Bearer", .NET lo rechaza.
>     

**P: Entro en bucle infinito de redirecciones.**

> **R:** Tienes un Guard que protege `/login` o el Guard comprueba el token, falla, redirige a `/login`, el componente Login intenta cargar algo, falla...
> 
> - Asegúrate de que la ruta `/login` **NO** tiene el `isAuthenticatedGuard`.
>     

---

### 🔹 PREGUNTA FINAL DEL "IDIOTA" (La más importante)

**P: Todo esto es muy complejo. ¿Por qué no usamos `jQuery` y una API simple en PHP? Funciona igual.**

> **R:** Sí, para una tienda de barrio funciona. Pero el objetivo del curso es prepararte para **Defensa / Banca / Enterprise**. Si en una entrevista para la OTAN dices "¿Por qué no usamos jQuery?", la entrevista acaba ahí.
> 
> La complejidad que estamos metiendo (Signals, Nx, Docker, Tipado Fuerte) es el **seguro de vida** para cuando el proyecto tenga 500.000 líneas de código y 50 desarrolladores trabajando a la vez. **Estás aprendiendo a escalar, no solo a**