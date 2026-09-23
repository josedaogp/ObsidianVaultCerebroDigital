Entendido. Procedemos a desplegar la capacidad de visualización geoespacial.

En el sector defensa, un mapa no es una imagen bonita; es una **herramienta de decisión**. OpenLayers es el estándar aquí porque permite manejar miles de entidades vectoriales, reproyecciones de coordenadas militares (MGRS) y funcionar offline si es necesario.

En este módulo, integraremos OpenLayers con Angular de forma eficiente. El reto principal es el rendimiento: **OpenLayers manipula el DOM agresivamente**. Si no tenemos cuidado, disparará la detección de cambios de Angular miles de veces por segundo, congelando la interfaz.

---

# 🗺️ Módulo 6: Visualización GIS (Tactical Map)

## 6.1. Instalación y Configuración de Assets

OpenLayers es una librería pura de JS, no tiene dependencias de Angular.

**1. Instalación:**

Bash

```
npm install ol
```

**2. Importar Estilos CSS:** El mapa necesita sus estilos base para funcionar (controles de zoom, attribution, etc.). Añade esto en tu archivo `apps/dashboard/src/styles.scss`:

SCSS

```
/* Importar estilos de OpenLayers */
@import 'ol/ol.css';

/* Asegurar que el contenedor del mapa tenga altura */
.map-container {
  width: 100%;
  height: 100%;
  min-height: 500px; /* Crítico: Si es 0, el mapa no se renderiza */
  background-color: #0f172a; /* Slate-900 */
  border-radius: 0.5rem;
  overflow: hidden;
}
```

---

## 6.2. Arquitectura: El MapComponent (Zone Bypass)

Aquí aplicaremos una técnica **Senior**: `NgZone.runOutsideAngular`. OpenLayers dispara eventos de `mousemove`, `render`, y `pointerdrag` constantemente. Si dejamos que Angular escuche todo esto, la aplicación será lenta. Instanciaremos el mapa "fuera" de Angular y solo volveremos a entrar a la "Zona" cuando necesitemos actualizar un dato de la UI (como hacer click en un tanque).

Crea el componente en: `libs/features/tactical-map/src/lib/map-view/map-view.component.ts`

TypeScript

```
import { Component, ElementRef, OnInit, OnDestroy, viewChild, inject, NgZone, effect } from '@angular/core';
import { CommonModule } from '@angular/common';

// OpenLayers Imports (Modular para Tree Shaking)
import Map from 'ol/Map';
import View from 'ol/View';
import TileLayer from 'ol/layer/Tile';
import OSM from 'ol/source/OSM'; // Usaremos OpenStreetMap por simplicidad, en PROD usaríamos servidores de teselas propios
import VectorLayer from 'ol/layer/Vector';
import VectorSource from 'ol/source/Vector';
import Feature from 'ol/Feature';
import Point from 'ol/geom/Point';
import { fromLonLat } from 'ol/proj'; // Conversión de coords GPS a WebMercator del mapa
import { Style, Circle, Fill, Stroke } from 'ol/style';

// Importamos nuestro servicio de señales
import { RealtimeService, UnitTelemetry } from '@tactical-c2/data-access-api';

@Component({
  selector: 'feat-map-view',
  standalone: true,
  imports: [CommonModule],
  template: `
    <div #map class="map-container"></div>
    
    <div class="absolute top-4 right-4 bg-slate-900/80 text-white p-2 rounded text-xs pointer-events-none">
      Unidades en Radar: {{ service.unitPositions().size }}
    </div>
  `,
  styles: [`:host { display: block; height: 100%; width: 100%; position: relative; }`]
})
export class MapViewComponent implements OnInit, OnDestroy {
  // Inyecciones
  private ngZone = inject(NgZone);
  public service = inject(RealtimeService); // Hacemos público para el template

  // Acceso al elemento del DOM
  mapElement = viewChild.required<ElementRef>('map');

  // Instancias de OpenLayers
  private map!: Map;
  private vectorSource = new VectorSource(); // Aquí viven los marcadores
  
  constructor() {
    // EFFECT: Reacciona a los cambios en el Signal del servicio (Telemetría)
    effect(() => {
      const units = this.service.unitPositions(); // Dependencia reactiva
      
      // Ejecutamos la lógica del mapa fuera de la comprobación de cambios
      this.ngZone.runOutsideAngular(() => {
        this.updateMapFeatures(units);
      });
    });
  }

  ngOnInit() {
    // Inicializar mapa fuera de Angular para performance
    this.ngZone.runOutsideAngular(() => {
      this.initMap();
    });
  }

  private initMap() {
    // Capa base (Mapa mundi)
    const rasterLayer = new TileLayer({
      source: new OSM({
        // En modo oscuro invertimos colores con CSS filter en el canvas (truco pro)
        // o usamos una fuente de tiles oscura.
      })
    });

    // Capa vectorial (Nuestras unidades)
    const vectorLayer = new VectorLayer({
      source: this.vectorSource,
      style: new Style({
        image: new Circle({
          radius: 8,
          fill: new Fill({ color: '#ef4444' }), // Rojo Táctico
          stroke: new Stroke({ color: '#ffffff', width: 2 })
        })
      })
    });

    this.map = new Map({
      target: this.mapElement().nativeElement, // Enganchar al DIV
      layers: [rasterLayer, vectorLayer],
      view: new View({
        center: fromLonLat([30.5234, 50.4501]), // Centrado en Kiev (aprox)
        zoom: 10
      }),
      controls: [] // Quitamos controles por defecto para look limpio
    });
  }

  // Algoritmo de reconciliación (Diffing)
  // En lugar de borrar y repintar todo, actualizamos solo lo que cambia
  private updateMapFeatures(units: Map<string, UnitTelemetry>) {
    if (!this.map) return;

    const existingFeatures = this.vectorSource.getFeatures();
    const activeIds = new Set<string>();

    // 1. Actualizar o Crear features
    units.forEach((unit) => {
      activeIds.add(unit.unitId);
      
      const existingFeature = this.vectorSource.getFeatureById(unit.unitId);
      const newCoords = fromLonLat([unit.longitude, unit.latitude]);

      if (existingFeature) {
        // MOVER: Solo actualizamos geometría
        const geometry = existingFeature.getGeometry() as Point;
        geometry.setCoordinates(newCoords);
      } else {
        // CREAR: Nueva unidad detectada
        const feature = new Feature({
          geometry: new Point(newCoords),
          name: unit.callsign,
          id: unit.unitId // Guardamos ID como propiedad
        });
        feature.setId(unit.unitId); // ID interno de OpenLayers para búsqueda rápida
        this.vectorSource.addFeature(feature);
      }
    });

    // 2. Limpieza (Garbage Collection)
    // Si una unidad ya no está en el mapa (destruida/offline), borrar su marcador
    existingFeatures.forEach(feature => {
      const id = feature.getId()?.toString();
      if (id && !activeIds.has(id)) {
        this.vectorSource.removeFeature(feature);
      }
    });
  }

  ngOnDestroy() {
    // Limpiar recursos de WebGL/Canvas para evitar fugas de memoria
    if (this.map) {
      this.map.setTarget(undefined);
    }
  }
}
```

---

## 6.3. Integración Visual: Dashboard Layout

Ahora reemplazaremos nuestro "Live Feed" de texto por el mapa real. Vamos a crear un layout dividido en el `AppComponent` o una ruta específica.

**Archivo:** Modifica `apps/dashboard/src/app/app.component.ts` para estructurar el layout.

TypeScript

```
import { Component } from '@angular/core';
import { RouterModule } from '@angular/router';
import { MapViewComponent } from '@tactical-c2/tactical-map'; // Importar mapa

@Component({
  standalone: true,
  imports: [RouterModule, MapViewComponent],
  selector: 'app-root',
  template: `
    <div class="flex h-screen w-screen bg-slate-900 text-white overflow-hidden">
      
      <aside class="w-1/3 min-w-[400px] bg-slate-800 border-r border-slate-700 flex flex-col z-10 shadow-2xl">
        <div class="p-4 bg-slate-900 border-b border-slate-700">
          <h1 class="text-xl font-bold tracking-widest text-blue-400">TACTICAL<span class="text-white">C2</span></h1>
        </div>
        
        <div class="flex-1 overflow-y-auto relative">
          <router-outlet></router-outlet>
        </div>
      </aside>

      <main class="flex-1 relative">
        <feat-map-view />
      </main>

    </div>
  `
})
export class AppComponent {}
```

_Nota: Asegúrate de que `MapViewComponent` esté exportado en el `index.ts` de su librería `libs/features/tactical-map/src/index.ts`._

---

## 6.4. Refinamiento Táctico: Estilos Avanzados (Opcional)

Un punto rojo es básico. Si quieres diferenciar unidades (amigos/enemigos) según su estado, modificamos la función de estilo en `updateMapFeatures`.

TypeScript

```
// Dentro de MapViewComponent

private getStyleForStatus(status: string): Style {
  const color = status === 'Engaged' ? '#ef4444' : '#22c55e'; // Rojo vs Verde
  
  return new Style({
    image: new Circle({
      radius: 6,
      fill: new Fill({ color: color }),
      stroke: new Stroke({ color: '#fff', width: 2 })
    }),
    // Añadir texto con el Callsign
    // text: new Text({ ... }) 
  });
}

// Y al crear la feature:
feature.setStyle(this.getStyleForStatus(unit.status));
```

---

### ✅ Checkpoint Módulo 6

Hemos logrado una integración GIS profesional:

1. **Performance:** Uso de `NgZone.runOutsideAngular` para mantener la aplicación fluida a 60fps incluso con actualizaciones constantes.
    
2. **Reactividad:** El mapa reacciona a `Signals`. No hay código imperativo sucio llamando a `updateMap()` desde otros componentes.
    
3. **Eficiencia:** Solo movemos píxeles (`setCoordinates`), no recreamos objetos DOM.
    
4. **Visualización:** Mapa a pantalla completa integrado con el sidebar de navegación.
    

Al correr la aplicación ahora, verás un mapa de OpenStreetMap y puntos rojos moviéndose solos (gracias al simulador .NET).

**El problema actual:** Cualquiera puede ver esto. Un sistema militar necesita **Seguridad**. Necesitamos Login, Tokens y Roles.

**Siguiente paso:** Implementar Autenticación JWT y Guards en Angular.

¿Listo para asegurar el perímetro? **Di "Empieza el Módulo 7"**.