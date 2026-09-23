#  Angular Cheatsheet: El Framework para Aplicaciones Web Escalables 🚀

## 🌟 **Conceptos Fundamentales**

*   **Angular:** Un framework de JavaScript (TypeScript) de código abierto, mantenido por Google, para construir aplicaciones web *single-page applications* (SPAs) y aplicaciones móviles (con Ionic o NativeScript).
*   **Componentes (Components):**  Bloques de construcción fundamentales de una aplicación Angular.  Un componente controla una parte de la pantalla (una vista).  Están formados por:
    *   **Plantilla (Template):**  HTML que define la vista.
    *   **Clase (Class):**  Código TypeScript que define la lógica del componente (propiedades y métodos).
    *   **Metadatos (Metadata):**  Información adicional sobre el componente (selector, templateUrl, styleUrls, etc.).  Se definen con un *decorador* (`@Component`).
*   **Módulos (Modules):**  Contenedores de componentes, directivas, pipes y servicios relacionados.  Una aplicación Angular tiene al menos un módulo raíz (`AppModule`).  Los módulos ayudan a organizar la aplicación y a dividirla en funcionalidades.
*   **Plantillas (Templates):**  HTML con sintaxis especial de Angular para data binding, directivas, etc.
*   **Data Binding:**  Mecanismo para sincronizar datos entre la clase del componente y la plantilla.
    *   **Interpolación (`{{ }}`):**  Muestra valores de propiedades del componente en la plantilla.
    *   **Property Binding (`[]`):**  Vincula una propiedad de un elemento HTML a una propiedad del componente.
    *   **Event Binding (`()`):**  Vincula un evento de un elemento HTML a un método del componente.
    *   **Two-way Binding (`[(ngModel)]`):**  Vincula una propiedad del componente a un input de formulario (sincronización bidireccional).
*   **Directivas (Directives):**  Instrucciones en el DOM que extienden la funcionalidad de HTML.
    *   **Componentes:**  Directivas con una plantilla.
    *   **Directivas estructurales (`*ngIf`, `*ngFor`, `*ngSwitch`):**  Modifican la estructura del DOM (añaden o eliminan elementos).
    *   **Directivas de atributo:**  Cambian la apariencia o el comportamiento de un elemento.
*   **Servicios (Services):**  Clases que proporcionan funcionalidades reutilizables en toda la aplicación (ej: acceso a datos, logging, etc.).  Se inyectan en los componentes a través de *inyección de dependencias*.
*   **Inyección de Dependencias (Dependency Injection - DI):**  Patrón de diseño que permite a los componentes obtener las dependencias (servicios, etc.) que necesitan sin tener que crearlas ellos mismos.  Angular tiene un sistema de DI integrado.
*   **Routing:**  Permite la navegación entre diferentes vistas (componentes) de la aplicación.
*   **Observables (RxJS):**  Biblioteca para programación reactiva con *observables*.  Los observables son *streams* de datos asíncronos.  Angular usa RxJS extensivamente (ej: para las peticiones HTTP).
*   **Formularios (Forms):**  Angular proporciona dos enfoques para crear formularios:
    *   **Template-driven forms:**  La lógica del formulario se define principalmente en la plantilla.
    *   **Reactive forms:**  La lógica del formulario se define principalmente en la clase del componente (más flexible y potente).
* **Pipes**: Transforman datos en la plantilla (formateo de fechas, números, etc.).

## 💻 **Instalación y Configuración**

1.  **Instalar Angular CLI (globalmente):**

    ```bash
    npm install -g @angular/cli
    ```

2.  **Crear un nuevo proyecto:**

    ```bash
    ng new mi-app
    ```

3.  **Ejecutar la aplicación en modo de desarrollo:**

    ```bash
    cd mi-app
    ng serve
    ```
    La aplicación estará disponible en `http://localhost:4200`.

## 🗂️ **Estructura de un Proyecto (Básica)**

```
mi-app/
├── src/                 # Código fuente de la aplicación
│   ├── app/             # Módulo principal de la aplicación
│   │   ├── app.component.html    # Plantilla del componente principal
│   │   ├── app.component.scss    # Estilos del componente principal
│   │   ├── app.component.spec.ts # Pruebas del componente principal
│   │   ├── app.component.ts      # Clase del componente principal
│   │   └── app.module.ts         # Módulo principal
│   ├── assets/          # Archivos estáticos (imágenes, etc.)
│   ├── environments/    # Archivos de entorno (desarrollo, producción, etc.)
│   ├── index.html       # Página HTML principal
│   ├── main.ts          # Punto de entrada principal
│   ├── styles.scss      # Estilos globales
│   └── ...
├── node_modules/
├── angular.json         # Configuración de Angular CLI
├── package.json
├── tsconfig.json        # Configuración de TypeScript
└── ...
```

## ⚛️ **Componentes (`@Component`)**

```typescript
// mi-componente.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-mi-componente', // Selector CSS (cómo se usará el componente en otras plantillas)
  templateUrl: './mi-componente.component.html', // Ruta a la plantilla
  styleUrls: ['./mi-componente.component.scss']  // Rutas a los archivos de estilos (opcional)
  // También se puede usar template y styles:
  // template: `<h1>Hola, mundo!</h1>`,
  // styles: [`h1 { color: blue; }`]
})
export class MiComponente {
  // Propiedades
  titulo: string = 'Mi Componente';
  contador: number = 0;

  // Métodos
  incrementarContador() {
    this.contador++;
  }
}
```

```html
<!-- mi-componente.component.html -->
<h1>{{ titulo }}</h1>  <!-- Interpolación -->

<p>Contador: {{ contador }}</p>

<button (click)="incrementarContador()">Incrementar</button> <!-- Event binding -->

<input type="text" [value]="titulo">  <!-- Property binding -->

<input type="text" [(ngModel)]="titulo"> <!-- Two-way binding (necesita FormsModule) -->

```

*   **`@Input()`:**  Decorador para definir propiedades que se pueden pasar desde un componente padre.
*   **`@Output()`:**  Decorador para definir eventos personalizados que el componente puede emitir.  Se usan con `EventEmitter`.
* **Ciclo de vida de un componente (Lifecycle Hooks):**
    *   `ngOnChanges()`:  Se llama cuando cambia el valor de una propiedad `@Input()`.
    *   `ngOnInit()`:  Se llama después de que Angular ha inicializado el componente (buen lugar para inicializar datos).
    *   `ngDoCheck()`: Se llama durante cada ciclo de detección de cambios.
    *   `ngAfterContentInit()`, `ngAfterContentChecked()`, `ngAfterViewInit()`, `ngAfterViewChecked()`:  Hooks relacionados con el contenido y la vista del componente.
    *   `ngOnDestroy()`:  Se llama justo antes de que Angular destruya el componente (buen lugar para limpiar recursos).

## 📦 **Módulos (`@NgModule`)**

```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms'; // Necesario para two-way binding (ngModel)
import { HttpClientModule } from '@angular/common/http'; // Para peticiones HTTP

import { AppComponent } from './app.component';
import { MiComponente } from './mi-componente/mi-componente.component';
// ... otros imports

@NgModule({
  declarations: [ // Componentes, directivas y pipes que pertenecen a este módulo
    AppComponent,
    MiComponente,
    // ...
  ],
  imports: [      // Otros módulos que este módulo necesita
    BrowserModule,
    FormsModule,
    HttpClientModule,
    // ...
  ],
  providers: [    // Servicios (providers) que estarán disponibles en este módulo
    // ...
  ],
  bootstrap: [AppComponent]  // Componente raíz (solo en el módulo principal)
})
export class AppModule { }
```

*   **`declarations`:**  Componentes, directivas y pipes que pertenecen a este módulo.
*   **`imports`:**  Otros módulos que este módulo necesita.
*   **`providers`:**  Servicios (providers) que estarán disponibles en este módulo (y en sus componentes hijos).
*   **`bootstrap`:**  Componente raíz (solo en el módulo principal - `AppModule`).
*   **`exports`:**  Componentes, directivas y pipes que este módulo exporta para que puedan ser usados por otros módulos.
* **Feature Modules**: Módulos que encapsulan una funcionalidad específica.
* **Shared Modules**: Módulos que contienen componentes, directivas y pipes que se usan en varios módulos.

## 🔄 **Data Binding**

*   **Interpolación (`{{ }}`):**

    ```html
    <p>Hola, {{ nombre }}!</p>
    ```

*   **Property Binding (`[]`):**

    ```html
    <img [src]="imagenUrl" [alt]="descripcion">
    <button [disabled]="estaDeshabilitado">Haz clic</button>
    ```

*   **Event Binding (`()`):**

    ```html
    <button (click)="manejarClic($event)">Haz clic</button>
    ```

*   **Two-way Binding (`[(ngModel)]`):**  (Requiere `FormsModule`)

    ```html
    <input type="text" [(ngModel)]="nombre">
    <p>Hola, {{ nombre }}!</p>
    ```

## 🚀 **Directivas**

*   **`*ngIf`:**  Añade o elimina un elemento del DOM según una condición.

    ```html
    <p *ngIf="mostrarMensaje">Este mensaje se mostrará si mostrarMensaje es true.</p>
    ```

*   **`*ngFor`:**  Repite un elemento para cada elemento de una lista.

    ```html
    <ul>
      <li *ngFor="let item of items; let i = index">
        {{ i + 1 }} - {{ item.nombre }}
      </li>
    </ul>
    ```

*   **`*ngSwitch`:**  Muestra un elemento u otro según el valor de una expresión.

    ```html
    <div [ngSwitch]="color">
      <p *ngSwitchCase="'red'" style="color: red;">Rojo</p>
      <p *ngSwitchCase="'green'" style="color: green;">Verde</p>
      <p *ngSwitchCase="'blue'" style="color: blue;">Azul</p>
      <p *ngSwitchDefault>Color desconocido</p>
    </div>
    ```
*  **`ngClass`**: Añade o elimina clases CSS condicionalmente.

    ```html
    <div [ngClass]="{ 'clase-a': condicionA, 'clase-b': condicionB }"></div>
    ```

*   **`ngStyle`:**  Aplica estilos CSS condicionalmente.

    ```html
      <div [ngStyle]="{ 'color': color, 'font-size': fontSize + 'px' }"></div>
    ```
* **Crear directivas personalizadas**:
    * **`@Directive` decorator:**
        ```typescript
            import { Directive, ElementRef, HostListener } from '@angular/core';

            @Directive({
              selector: '[appResaltar]' // Selector CSS (como un componente, pero sin plantilla)
            })
            export class ResaltarDirective {
              constructor(private el: ElementRef) { }

              @HostListener('mouseenter') onMouseEnter() {
                this.resaltar('yellow');
              }

              @HostListener('mouseleave') onMouseLeave() {
                this.resaltar('');
              }

              private resaltar(color: string) {
                this.el.nativeElement.style.backgroundColor = color;
              }
            }
        ```

## 💉 **Servicios e Inyección de Dependencias**

```typescript
// mi-servicio.service.ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'  // Disponible en toda la aplicación (inyectable a nivel raíz)
  // También se puede proporcionar en un módulo específico:
  // providedIn: MiModulo
})
export class MiServicio {
  datos: string[] = [];

  obtenerDatos(): string[] {
    return this.datos;
  }

  agregarDato(dato: string) {
    this.datos.push(dato);
  }
}
```

```typescript
// mi-componente.component.ts
import { Component, OnInit } from '@angular/core';
import { MiServicio } from '../mi-servicio.service'; // Importa el servicio

@Component({ ... })
export class MiComponente implements OnInit {
  datos: string[] = [];

  constructor(private miServicio: MiServicio) { } // Inyecta el servicio

  ngOnInit() {
    this.datos = this.miServicio.obtenerDatos();
  }

  agregarDato(dato: string) {
    this.miServicio.agregarDato(dato);
  }
}
```

*   **`@Injectable()`:**  Decorador que indica que una clase puede ser inyectada.
    *   `providedIn`:  Especifica dónde estará disponible el servicio ('root', un módulo específico, etc.).
*   **Inyección en el constructor:**  Los servicios se inyectan a través del constructor del componente.  Angular se encarga de crear las instancias de los servicios y pasarlas al constructor.

## 🧭 **Routing (`@angular/router`)**

1.  **Configurar las rutas:**

    ```typescript
    // app-routing.module.ts (generalmente se crea automáticamente)
    import { NgModule } from '@angular/core';
    import { RouterModule, Routes } from '@angular/router';

    import { HomeComponent } from './home/home.component';
    import { AboutComponent } from './about/about.component';
    import { NotFoundComponent } from './not-found/not-found.component';

    const routes: Routes = [
      { path: '', component: HomeComponent }, // Ruta principal
      { path: 'about', component: AboutComponent },
      { path: 'usuarios/:id', component: UserDetailComponent }, // Ruta con parámetro
      { path: '**', component: NotFoundComponent } // Ruta comodín (para 404)
    ];

    @NgModule({
      imports: [RouterModule.forRoot(routes)],  // forRoot() para el módulo raíz
      exports: [RouterModule]
    })
    export class AppRoutingModule { }
    ```

2.  **Importar `AppRoutingModule` en `AppModule`:**

    ```typescript
    // app.module.ts
    import { AppRoutingModule } from './app-routing.module';

    @NgModule({
      imports: [
        // ...
        AppRoutingModule
      ],
      // ...
    })
    export class AppModule { }

    ```

3.  **Usar `<router-outlet>`:**  Directiva que indica dónde se mostrarán los componentes de las rutas.

    ```html
    <!-- app.component.html -->
    <nav>
      <a routerLink="/">Inicio</a>
      <a routerLink="/about">Acerca de</a>
    </nav>

    <router-outlet></router-outlet>  <!-- Aquí se cargarán los componentes -->
    ```

4.  **Navegación:**
    *   **`routerLink` directive:**  Para crear enlaces.

        ```html
        <a routerLink="/usuarios/123">Ver Usuario 123</a>
        <a [routerLink]="['/usuarios', userId]">Ver Usuario</a>
        ```

    *   **`Router` service:**  Para navegación programática.

        ```typescript
        import { Router } from '@angular/router';

        constructor(private router: Router) { }

        navegarAAbout() {
          this.router.navigate(['/about']);
        }

        navegarAUsuario(id: number) {
          this.router.navigate(['/usuarios', id]);
        }
        ```
    *  **`ActivatedRoute`**: Para acceder a información sobre la ruta actual (parámetros, query parameters, etc.).

## 📡 **HTTP (`@angular/common/http`)**

1.  **Importar `HttpClientModule` en `AppModule` (o en el módulo donde lo vayas a usar).**
2.  **Inyectar `HttpClient` en tu servicio.**

    ```typescript
    // mi-servicio.service.ts
    import { Injectable } from '@angular/core';
    import { HttpClient } from '@angular/common/http';
    import { Observable } from 'rxjs';

    @Injectable({
      providedIn: 'root'
    })
    export class MiServicio {
      private apiUrl = 'https://api.example.com';

      constructor(private http: HttpClient) { }

      obtenerDatos(): Observable<any> {
        return this.http.get(`${this.apiUrl}/datos`);
      }

      crearDato(dato: any): Observable<any> {
        return this.http.post(`${this.apiUrl}/datos`, dato);
      }

      // Otros métodos: put, delete, patch, etc.
    }
    ```

3.  **Suscribirse a los observables:**

    ```typescript
    // mi-componente.component.ts
    import { Component, OnInit } from '@angular/core';
    import { MiServicio } from '../mi-servicio.service';

    @Component({ ... })
    export class MiComponente implements OnInit {
      datos: any[] = [];

      constructor(private miServicio: MiServicio) { }

      ngOnInit() {
        this.miServicio.obtenerDatos().subscribe(
          (data) => {
            this.datos = data;
          },
          (error) => {
            console.error("Error al obtener datos:", error);
          }
        );
      }
    }
    ```

*   **`HttpClient` methods:**  `get()`, `post()`, `put()`, `delete()`, `patch()`, `head()`, `options()`.  Devuelven *observables*.
*   **Interceptors:**  Interceptan las peticiones HTTP y las respuestas (para logging, autenticación, manejo de errores, etc.).

## 📝 **Formularios**

*   **Template-driven forms (Formularios basados en plantillas):**

    *   Usa `ngModel` para el two-way binding.
    *   Usa directivas como `ngForm`, `ngSubmit`.
    *   La validación se define principalmente en la plantilla.

    ```html
    <!-- app.component.html -->
    <form #miFormulario="ngForm" (ngSubmit)="onSubmit(miFormulario)">
        <label for="nombre">Nombre:</label>
        <input type="text" id="nombre" name="nombre" [(ngModel)]="modelo.nombre" required>
        <div *ngIf="miFormulario.submitted && miFormulario.controls['nombre']?.invalid">
          El nombre es obligatorio.
        </div>

      <label for="email">Email:</label>
      <input type="email" id="email" name="email" [(ngModel)]="modelo.email" required email>
        <div *ngIf="miFormulario.submitted && miFormulario.controls['email']?.invalid">
            <div *ngIf="miFormulario.controls['email']?.errors?.['required']">
                El email es obligatorio
            </div>
            <div *ngIf="miFormulario.controls['email']?.errors?.['email']">
                Email inválido
            </div>
        </div>

        <button type="submit">Enviar</button>
    </form>
    ```

    ```typescript
    // app.component.ts
    import { Component } from '@angular/core';
    import { NgForm } from '@angular/forms';

    @Component({...})
    export class AppComponent {
      modelo: any = {};

      onSubmit(form: NgForm) {
        if (form.valid) {
          console.log("Formulario válido.  Datos:", this.modelo);
          // Enviar datos al servidor...
        }
      }
    }

    ```

*   **Reactive forms (Formularios reactivos):**  (Recomendado para formularios complejos)

    *   Importar `ReactiveFormsModule` en tu módulo.
    *   Usa `FormGroup`, `FormControl`, `FormArray`, `FormBuilder`.
    *   La validación se define en la clase del componente.

    ```typescript
    // app.component.ts
    import { Component, OnInit } from '@angular/core';
    import { FormBuilder, FormGroup, Validators, FormControl, FormArray } from '@angular/forms';

    @Component({...})
    export class AppComponent implements OnInit {
      miFormulario: FormGroup;

      constructor(private fb: FormBuilder) { } // Inyecta FormBuilder

      ngOnInit() {
        this.miFormulario = this.fb.group({
          nombre: ['', [Validators.required, Validators.minLength(3)]], // FormControl con validadores
          email: ['', [Validators.required, Validators.email]],
          hobbies: this.fb.array([]), // FormArray (para campos repetidos)
        });

        //También se puede usar new FormControl, new FormGroup y new FormArray.
      }

      onSubmit() {
        if (this.miFormulario.valid) {
          console.log("Formulario válido.  Datos:", this.miFormulario.value);
          // Enviar datos al servidor...
        }
      }

      get nombre() { return this.miFormulario.get('nombre'); } // Getter para acceder fácilmente al FormControl
      get email() { return this.miFormulario.get('email'); }
      get hobbies() { return this.miFormulario.get('hobbies') as FormArray; }

      agregarHobby() {
          this.hobbies.push(new FormControl('')); //Añade dinámicamente FormControls
      }
    }

    ```

    ```html
    <!-- app.component.html -->
    <form [formGroup]="miFormulario" (ngSubmit)="onSubmit()">
      <label for="nombre">Nombre:</label>
      <input type="text" id="nombre" formControlName="nombre">
      <div *ngIf="nombre?.invalid && (nombre?.dirty || nombre?.touched)">
        <div *ngIf="nombre?.errors?.['required']">El nombre es obligatorio.</div>
        <div *ngIf="nombre?.errors?.['minlength']">El nombre debe tener al menos 3 caracteres.</div>
      </div>

      <label for="email">Email:</label>
      <input type="email" id="email" formControlName="email">

        <div formArrayName="hobbies">
            <div *ngFor="let hobby of hobbies.controls; let i=index">
                <input type="text" [formControlName]="i">
            </div>
        </div>
        <button type="button" (click)="agregarHobby()">Añadir hobby</button>

      <button type="submit">Enviar</button>
    </form>
    ```

## 🧪 **Testing**

*   **`TestBed`:**  Utilidad principal para configurar el entorno de pruebas de Angular.
*   **`ComponentFixture`:**  Wrapper para un componente, que proporciona acceso a la instancia del componente, al DOM, etc.
*   **`jasmine`:**  Framework de testing BDD (Behavior-Driven Development) usado por defecto en Angular.
*   **`karma`:**  Test runner usado por defecto en Angular.
*   **`ng test`:**  Comando de Angular CLI para ejecutar las pruebas.
*  **`async` / `fakeAsync` / `tick`**: Para pruebas asíncronas.
* **Spies**: Para simular el comportamiento de dependencias.

## 📏 **Pipes**

*   Transforman datos en la plantilla.

    ```html
    <p>Fecha: {{ fecha | date:'dd/MM/yyyy' }}</p>
    <p>Precio: {{ precio | currency:'EUR' }}</p>
    <p>{{ mensaje | uppercase }}</p>
    ```

*   **Pipes incorporados:**  `date`, `currency`, `uppercase`, `lowercase`, `number`, `percent`, `json`, `slice`, `async`, etc.
*   **Crear pipes personalizados:**
    *   `@Pipe` decorator.
    *   `transform()` method.

        ```typescript
        import { Pipe, PipeTransform } from '@angular/core';

        @Pipe({
          name: 'miPipePersonalizado'
        })
        export class MiPipePersonalizado implements PipeTransform {
          transform(valor: string, arg1: string, arg2: number): string {
            // ... lógica de transformación
            return valor.toUpperCase() + arg1 + arg2;
          }
        }
        ```

## ➕ **Temas Avanzados**

*   **RxJS (Observables):**  Programación reactiva con observables.  Muy importante en Angular.
*   **Change Detection:**  Mecanismo de Angular para detectar cambios en los datos y actualizar la UI.
    *   `ChangeDetectorRef`:  Para controlar manualmente la detección de cambios.
    *   `OnPush`:  Estrategia de detección de cambios que mejora el rendimiento (detecta cambios solo cuando cambian las referencias de las propiedades `@Input()`).
*   **Content Projection (`<ng-content>`):**  Proyecta contenido desde un componente padre a un componente hijo.
*   **ViewChild y ContentChild:**  Acceden a elementos del DOM o a componentes hijos.
*   **Lazy Loading:**  Carga módulos de forma diferida (solo cuando se necesitan), mejorando el tiempo de carga inicial.
*   **Internacionalización (i18n):**  Adapta la aplicación a diferentes idiomas y regiones.
* **Angular Universal:**  Server-side rendering (SSR) para Angular.
*   **Web Workers:**  Ejecuta código JavaScript en segundo plano (en un hilo separado).
*   **Service Workers:**  Permiten crear aplicaciones web progresivas (PWAs) que funcionan offline.
* **Animaciones**:  `@angular/animations`.
* **Esquemas (Schematics)**: Angular CLI tiene generadores de código.