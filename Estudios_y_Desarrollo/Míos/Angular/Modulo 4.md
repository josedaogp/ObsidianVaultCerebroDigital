¡Recibido! Avanzamos al **Módulo 4**.

Aquí es donde separamos a los "coders" de los "arquitectos". Un error clásico es llamar a `HttpClient` directamente desde el Componente. Eso rompe la separación de responsabilidades y hace que la gestión del estado sea una pesadilla.

Vamos a implementar un **Patrón de Store Ligero basado en Signals**. No necesitamos la complejidad de NgRx (Redux) para esta escala, pero sí necesitamos sus beneficios: **Single Source of Truth**.

---

# 🧠 Módulo 4: Gestión de Estado Avanzada (Signals Store)

## 4.1. Configuración de Infraestructura HTTP

En Angular 18 Standalone, ya no importamos `HttpClientModule`. Proveemos el servicio en el arranque de la aplicación. Además, configuraremos `fetch` (más moderno que XHR) y prepararemos el terreno para interceptores.

**Archivo: `apps/dashboard/src/app/app.config.ts`**

TypeScript

```
import { ApplicationConfig, provideZoneChangeDetection } from '@angular/core';
import { provideRouter, withComponentInputBinding, withViewTransitions } from '@angular/router';
import { provideHttpClient, withFetch, withInterceptors } from '@angular/common/http'; // <--- OJO AQUÍ
import { AppRoutes } from './app.routes';
import { baseUrlInterceptor } from '@tactical-c2/data-access-api'; // Lo crearemos en breve

export const appConfig: ApplicationConfig = {
  providers: [
    // Optimización de detección de cambios (Coalescing)
    provideZoneChangeDetection({ eventCoalescing: true }), 
    provideRouter(AppRoutes.routes, withComponentInputBinding(), withViewTransitions()),
    
    // Configuración HTTP Moderna
    provideHttpClient(
      withFetch(), // Usa Fetch API nativa (mejor para SSR y rendimiento)
      withInterceptors([baseUrlInterceptor]) // Middleware de peticiones
    )
  ],
};
```

---

## 4.2. Contratos de Datos (Frontend DTOs)

Debemos replicar los DTOs del Backend en TypeScript para tener tipado estricto. Esto va en nuestra librería `data-access`.

**Archivo: `libs/data-access/tactical-api/src/lib/models/mission.model.ts`**

TypeScript

```
export type MissionStatus = 'Planned' | 'InProgress' | 'Completed' | 'Aborted';

// Debe coincidir con MissionSummaryDto del Backend
export interface MissionSummary {
  id: string;
  title: string;
  status: MissionStatus;
  assignedUnitCount: number;
}

// Debe coincidir con MissionDetailDto
export interface MissionDetail {
  id: string;
  title: string;
  description: string;
  status: MissionStatus;
  riskLevel: number;
  unitCallsigns: string[];
}

// Estado para nuestro Store
export interface MissionState {
  missions: MissionSummary[];
  selectedMission: MissionDetail | null;
  isLoading: boolean;
  error: string | null;
}
```

---

## 4.3. Infrastructure Layer: El Interceptor

Para evitar escribir `https://localhost:5001/api/v1` en cada llamada, creamos un interceptor funcional.

**Archivo: `libs/data-access/tactical-api/src/lib/interceptors/base-url.interceptor.ts`**

TypeScript

```
import { HttpInterceptorFn } from '@angular/common/http';
import { environment } from '@tactical-c2/shared/util-env'; // Asumimos un env file

// Hardcodeado para este ejemplo, en producción usar environment.ts
const API_URL = 'http://localhost:5001/api/v1';

export const baseUrlInterceptor: HttpInterceptorFn = (req, next) => {
  // Si la URL ya es absoluta (ej: assets externos), no tocar
  if (req.url.startsWith('http')) {
    return next(req);
  }

  // Clonar la petición y prefijar la URL
  const apiReq = req.clone({
    url: `${API_URL}/${req.url}`
  });

  return next(apiReq);
};
```

---

## 4.4. Data Access Layer: ApiService

Este servicio solo se preocupa de **hablar con el servidor**. No guarda estado. Devuelve Observables (RxJS) porque HTTP es asíncrono por naturaleza.

**Archivo: `libs/data-access/tactical-api/src/lib/services/mission-api.service.ts`**

TypeScript

```
import { Injectable, inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { MissionSummary, MissionDetail } from '../models/mission.model';

@Injectable({ providedIn: 'root' })
export class MissionApiService {
  private http = inject(HttpClient);

  getMissions(): Observable<MissionSummary[]> {
    return this.http.get<MissionSummary[]>('missions');
  }

  getMissionById(id: string): Observable<MissionDetail> {
    return this.http.get<MissionDetail>(`missions/${id}`);
  }
  
  createMission(payload: { title: string, description: string, riskLevel: number }): Observable<MissionDetail> {
     return this.http.post<MissionDetail>('missions', payload);
  }
}
```

---

## 4.5. State Management: The Signal Store (El Cerebro)

Aquí ocurre la magia. Convertiremos los Observables "fríos" del servicio API en Signals "calientes" para la vista. Este patrón encapsula la mutación del estado (inmutabilidad local).

**Archivo: `libs/data-access/tactical-api/src/lib/stores/mission.store.ts`**

TypeScript

```
import { Injectable, inject, signal, computed } from '@angular/core';
import { MissionApiService } from '../services/mission-api.service';
import { MissionState, MissionSummary } from '../models/mission.model';
import { catchError, finalize, tap } from 'rxjs/operators';
import { of } from 'rxjs';

// Estado inicial
const initialState: MissionState = {
  missions: [],
  selectedMission: null,
  isLoading: false,
  error: null
};

@Injectable({ providedIn: 'root' })
export class MissionStore {
  private api = inject(MissionApiService);

  // 1. STATE (Privado): WritableSignal que contiene todo el árbol de estado
  private state = signal<MissionState>(initialState);

  // 2. SELECTORS (Públicos): Computed Signals (Read-only slices)
  // Solo se recalculan si la parte específica del estado cambia.
  readonly missions = computed(() => this.state().missions);
  readonly isLoading = computed(() => this.state().isLoading);
  readonly error = computed(() => this.state().error);
  
  // Selector derivado complejo (Ej: filtrar misiones activas)
  readonly activeMissionsCount = computed(() => 
    this.state().missions.filter(m => m.status !== 'Completed').length
  );

  // 3. ACTIONS (Métodos que mutan el estado)
  
  loadMissions(): void {
    // Patch state: Loading start
    this.state.update(s => ({ ...s, isLoading: true, error: null }));

    this.api.getMissions().pipe(
      tap((missions) => {
        // Success: Actualizamos estado con datos
        this.state.update(s => ({ ...s, missions }));
      }),
      catchError((err) => {
        // Error handling
        console.error('Error loading missions', err);
        this.state.update(s => ({ ...s, error: 'Fallo en la conexión táctica.' }));
        return of([]);
      }),
      finalize(() => {
        // Loading end
        this.state.update(s => ({ ...s, isLoading: false }));
      })
    ).subscribe(); 
    // Nota Senior: Nos suscribimos aquí porque el Store es quien "posee" el flujo de datos.
    // Los componentes solo leen signals.
  }

  addMissionOptimistic(tempMission: MissionSummary) {
    // Ejemplo de UI Optimista: Agregar antes de que el server confirme
    this.state.update(s => ({ ...s, missions: [...s.missions, tempMission] }));
  }
}
```

---

## 4.6. Refactorizando la Feature (Conectando los puntos)

Volvemos a nuestro `MissionListComponent`. Vamos a eliminar los datos falsos y conectarlo al Store. Observa lo limpio que queda el componente.

**Archivo: `libs/features/mission-control/src/lib/mission-list/mission-list.component.ts`**

TypeScript

```
import { Component, OnInit, inject } from '@angular/core';
import { CommonModule } from '@angular/common';
import { StatusBadgeComponent, TacticalCardComponent } from '@tactical-c2/ui-kit';
import { MissionStore } from '@tactical-c2/data-access-api'; // Importar Store

@Component({
  selector: 'feat-mission-list',
  standalone: true,
  imports: [CommonModule, StatusBadgeComponent, TacticalCardComponent],
  template: `
    <div class="p-6 bg-slate-100 dark:bg-slate-900 min-h-screen">
      <header class="mb-6 flex justify-between items-center">
        <div>
          <h1 class="text-2xl font-bold text-slate-800 dark:text-white">Centro de Mando</h1>
          <p class="text-slate-500">Operaciones en curso: {{ store.activeMissionsCount() }}</p>
        </div>
        
        <button (click)="refresh()" class="px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700">
          Refrescar Datos
        </button>
      </header>

      @if (store.isLoading()) {
        <div class="w-full h-1 bg-blue-200 overflow-hidden">
          <div class="animate-progress w-full h-full bg-blue-600 origin-left-right"></div>
        </div>
      }

      @if (store.error()) {
        <div class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded mb-4">
          <strong>Alerta:</strong> {{ store.error() }}
        </div>
      }

      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        
        @for (mission of store.missions(); track mission.id) {
          
          <ui-tactical-card [title]="mission.title" [type]="mission.status === 'InProgress' ? 'alert' : 'info'">
            <div slot="actions">
              <span class="text-xs text-gray-400">#{{ mission.assignedUnitCount }} Unidades</span>
            </div>

            <div class="flex justify-between items-center mt-4">
              <ui-status-badge [status]="mission.status | lowercase" [label]="mission.status" />
              <button class="text-blue-600 text-sm hover:underline">Detalles</button>
            </div>
          </ui-tactical-card>

        } @empty {
          @if (!store.isLoading()) {
             <div class="col-span-3 text-center p-10 text-gray-500">
               Sin datos de misión.
             </div>
          }
        }

      </div>
    </div>
  `
})
export class MissionListComponent implements OnInit {
  // Inyectamos el Store (público para usarlo en template)
  public store = inject(MissionStore);

  ngOnInit() {
    // Disparamos la carga inicial
    this.store.loadMissions();
  }

  refresh() {
    this.store.loadMissions();
  }
}
```

_Nota: Para probar esto, asegúrate de que tu API .NET esté corriendo (`dotnet run` en `apps/api`) y tu Frontend también (`npx nx serve dashboard`). Habilita CORS en el backend si tienes problemas (ya lo hicimos en el módulo 2)._

---

### ✅ Checkpoint Módulo 4

Hemos logrado una arquitectura profesional:

1. **Separación de Intereses:** API Service (Http) vs Store (Estado) vs Componente (Vista).
    
2. **Reactividad:** El componente se actualiza automáticamente cuando el Store cambia.
    
3. **Manejo de Errores:** Centralizado en el Store y mostrado en la UI.
    
4. **Clean Code:** El componente es extremadamente simple, sin lógica compleja ni suscripciones manuales a Observables.
    

**Siguiente paso:** Ahora que tenemos datos estáticos (REST), vamos a darle vida real. En defensa, saber dónde están tus unidades hace 1 minuto no sirve. Necesitamos saber dónde están **AHORA**.

Es hora de implementar WebSockets.

¿Listo para el tiempo real? **Di "Empieza el Módulo 5"**.