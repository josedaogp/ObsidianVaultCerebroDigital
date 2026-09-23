¡Afirmativo! Continuamos con la operación.

Bienvenido al **Módulo 3**. Aquí es donde notarás la mayor diferencia con el Angular que conocías. Si vienes de versiones anteriores a la 14, prepárate: **Angular ha cambiado de piel.**

Ya no pensamos en "Módulos" (`NgModule`). Ahora pensamos en un grafo de componentes independientes. Esto no es solo azúcar sintáctico; permite un **Tree-shaking** (eliminación de código muerto) mucho más agresivo y una carga más rápida, vital para aplicaciones tácticas que corren en hardware limitado sobre el terreno.

---

# 🎨 Módulo 3: Fundamentos Angular Moderno (The View Layer)

## 3.1. El Nuevo Paradigma: Signal Inputs & Control Flow

Antes de escribir, actualicemos tu "Mental Model".

1. **Standalone Components:** El componente se autodefine. Sus dependencias (`imports`) están en el propio decorador `@Component`. Adiós `SharedModule`.
    
2. **Signal Inputs:** Olvida `@Input() data: any`. Ahora usamos `data = input<any>()`. Esto convierte las entradas en Signals reactivos automáticamente.
    
3. **Control Flow:** `*ngIf` y `*ngFor` son historia. Usamos la sintaxis nativa `@if` y `@for`, que el compilador optimiza mejor y no requiere importar `CommonModule`.
    

---

## 3.2. UI Kit: Creando Componentes "Dumb" (Presentational)

Vamos a construir nuestra librería UI (`libs/ui/kit`). Estos componentes deben ser puros: reciben datos, muestran datos y emiten eventos. **Cero lógica de negocio.**

### Componente 1: Tactical Status Badge

Un indicador visual del estado de una misión o unidad.

**Generar componente:**

Bash

```
# --export: Para que sea visible fuera de la librería
npx nx g @nx/angular:component components/status-badge --project=ui-kit --export
```

**Código: `libs/ui/kit/src/lib/components/status-badge/status-badge.component.ts`**

TypeScript

```
import { Component, computed, input } from '@angular/core';
import { CommonModule } from '@angular/common';

// Definimos tipos literales para restringir valores (TypeScript estricto)
export type StatusType = 'active' | 'offline' | 'engaged' | 'compromised' | 'planned';

@Component({
  selector: 'ui-status-badge',
  standalone: true,
  imports: [CommonModule],
  template: `
    <span
      class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium border"
      [class]="badgeClasses()"
    >
      @if (status() === 'active' || status() === 'engaged') {
        <span class="flex h-2 w-2 relative mr-1.5">
          <span class="animate-ping absolute inline-flex h-full w-full rounded-full opacity-75 bg-current"></span>
          <span class="relative inline-flex rounded-full h-2 w-2 bg-current"></span>
        </span>
      }
      
      {{ label() | uppercase }}
    </span>
  `,
  styles: [`
    :host { display: inline-block; }
  `]
})
export class StatusBadgeComponent {
  // 1. Signal Inputs: Reactivos por defecto.
  // Si el padre cambia el input, este signal se actualiza.
  status = input.required<StatusType>(); 
  label = input.required<string>();

  // 2. Computed Signal: Derivamos las clases CSS basándonos en el estado.
  // Se recalcula SOLO si 'status' cambia.
  badgeClasses = computed(() => {
    switch (this.status()) {
      case 'active': return 'bg-green-100 text-green-800 border-green-200';
      case 'engaged': return 'bg-red-100 text-red-800 border-red-200 animate-pulse'; // Parpadeo alerta
      case 'offline': return 'bg-gray-100 text-gray-800 border-gray-200';
      case 'compromised': return 'bg-yellow-100 text-yellow-800 border-yellow-200';
      default: return 'bg-blue-100 text-blue-800 border-blue-200';
    }
  });
}
```

### Componente 2: Tactical Card (Contenedor Reutilizable)

Usaremos **Content Projection** (`ng-content`) para crear contenedores consistentes.

**Generar componente:**

Bash

```
npx nx g @nx/angular:component components/tactical-card --project=ui-kit --export
```

**Código: `libs/ui/kit/src/lib/components/tactical-card/tactical-card.component.ts`**

TypeScript

```
import { Component, input } from '@angular/core';

@Component({
  selector: 'ui-tactical-card',
  standalone: true,
  template: `
    <div class="bg-white dark:bg-slate-800 shadow-lg rounded-lg overflow-hidden border-l-4"
         [class.border-blue-500]="type() === 'info'"
         [class.border-red-500]="type() === 'alert'">
      
      <div class="px-4 py-3 border-b border-slate-200 dark:border-slate-700 flex justify-between items-center bg-slate-50 dark:bg-slate-900">
        <h3 class="text-sm font-bold uppercase tracking-wider text-slate-700 dark:text-slate-200">
          {{ title() }}
        </h3>
        <ng-content select="[slot=actions]"></ng-content>
      </div>

      <div class="p-4">
        <ng-content></ng-content>
      </div>
    </div>
  `
})
export class TacticalCardComponent {
  title = input.required<string>();
  type = input<'info' | 'alert'>('info'); // Valor por defecto 'info'
}
```

---

## 3.3. Feature Layer: El "Smart" Component (Mission Control)

Ahora vamos a `libs/features/mission-control`. Aquí crearemos una vista que consuma estos componentes. Este componente será "Inteligente" (en el futuro conectará con servicios), pero por ahora usaremos **datos mockeados** para establecer la estructura visual.

**Generar componente:**

Bash

```
npx nx g @nx/angular:component mission-list --project=mission-control --export
```

**Código: `libs/features/mission-control/src/lib/mission-list/mission-list.component.ts`**

Presta atención al uso de **`@for`**, **`@empty`** y **`@defer`**.

TypeScript

```
import { Component, signal } from '@angular/core';
import { CommonModule } from '@angular/common';
import { StatusBadgeComponent, StatusType } from '@tactical-c2/ui-kit'; // Importación desde librería
import { TacticalCardComponent } from '@tactical-c2/ui-kit';

// Mock Interface local (luego vendrá del DTO)
interface MissionMock {
  id: string;
  title: string;
  status: StatusType;
  description: string;
}

@Component({
  selector: 'feat-mission-list',
  standalone: true,
  // IMPORTANTE: Importamos los componentes Standalone que usamos
  imports: [CommonModule, StatusBadgeComponent, TacticalCardComponent],
  template: `
    <div class="p-6 bg-slate-100 min-h-screen">
      <header class="mb-6">
        <h1 class="text-2xl font-bold text-slate-800">Operaciones Activas</h1>
        <p class="text-slate-500">Panel de control de misiones en tiempo real</p>
      </header>

      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        
        @for (mission of missions(); track mission.id) {
          
          @defer (on viewport) {
            <ui-tactical-card [title]="mission.title" [type]="mission.status === 'engaged' ? 'alert' : 'info'">
              
              <div slot="actions">
                <span class="text-xs text-gray-400">#{{ mission.id }}</span>
              </div>

              <p class="text-sm text-slate-600 mb-4">{{ mission.description }}</p>
              
              <div class="flex justify-between items-center mt-4">
                <ui-status-badge [status]="mission.status" [label]="mission.status" />
                <button class="text-blue-600 text-sm hover:underline">Ver Detalles &rarr;</button>
              </div>

            </ui-tactical-card>
          } @placeholder {
            <div class="h-40 bg-gray-200 rounded-lg animate-pulse"></div>
          }

        } @empty {
          <div class="col-span-3 text-center p-10 text-gray-500">
            No hay misiones activas en este momento.
          </div>
        }

      </div>
    </div>
  `
})
export class MissionListComponent {
  // Usamos Signals para el estado local
  missions = signal<MissionMock[]>([
    { id: 'OP-101', title: 'Operación Vanguardia', status: 'active', description: 'Reconocimiento de perímetro norte.' },
    { id: 'OP-102', title: 'Escudo Delta', status: 'engaged', description: 'Intercepción de hostiles en sector 4.' },
    { id: 'OP-103', title: 'Suministro Aéreo', status: 'planned', description: 'Logística para base avanzada.' },
    { id: 'OP-104', title: 'Silencio Radio', status: 'offline', description: 'Unidad en zona de sombra.' },
  ]);
}
```

---

## 3.4. Routing: Configuración de Lazy Loading

En un monorepo Nx, la aplicación principal (`apps/dashboard`) no debe importar componentes directamente. Debe cargar las librerías "Feature" perezosamente (Lazy Loading).

**Archivo: `apps/dashboard/src/app/app.routes.ts`**

TypeScript

```
import { Route } from '@angular/router';

export class AppRoutes {
  static routes: Route[] = [
    {
      path: '',
      redirectTo: 'missions',
      pathMatch: 'full',
    },
    {
      path: 'missions',
      // Lazy Loading de la librería completa
      loadComponent: () => 
        import('@tactical-c2/mission-control').then(m => m.MissionListComponent)
    },
    // Más adelante añadiremos 'map'
    {
      path: '**',
      redirectTo: 'missions'
    }
  ];
}
```

Asegúrate de que `libs/features/mission-control/src/index.ts` exporte el componente:

TypeScript

```
export * from './lib/mission-list/mission-list.component';
```

**Archivo: `apps/dashboard/src/app/app.config.ts`** (Setup Moderno)

TypeScript

```
import { ApplicationConfig } from '@angular/core';
import { provideRouter, withComponentInputBinding, withViewTransitions } from '@angular/router';
import { AppRoutes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [
    // withViewTransitions: ¡Magia! Animaciones automáticas entre rutas (tipo App nativa)
    provideRouter(AppRoutes.routes, withComponentInputBinding(), withViewTransitions()),
  ],
};
```

---

### ✅ Checkpoint Módulo 3

Hemos modernizado el Frontend:

1. **Sin NgModules:** Todo es `standalone`.
    
2. **Librería UI:** Creamos componentes `Dumb` reutilizables (`ui-kit`).
    
3. **Signals Inputs:** Comunicación padres-hijos reactiva y tipada.
    
4. **Control Flow:** `@if`, `@for`, `@defer` implementados.
    
5. **Architecture:** Separación estricta Feature / UI.
    

Ahora tenemos una UI bonita pero "tonta". Los datos están hardcodeados.

**Siguiente paso:** Necesitamos conectar esto con el Backend .NET que creamos en el Módulo 2. Para eso, necesitamos un **Store** (Gestión de Estado) que use `HttpClient` y `Signals` para manejar la verdad de la aplicación.

¿Listo para la gestión de estado avanzada? **Di "Empieza el Módulo 4"**.