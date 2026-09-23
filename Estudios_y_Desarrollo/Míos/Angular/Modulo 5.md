Entendido. Iniciamos el **Módulo 5**.

En sistemas de Defensa (C2), la **Latencia** es el enemigo. No podemos pedirle al servidor "¿Dónde están los tanques?" cada 5 segundos (Polling). El servidor debe **gritarnos** la posición en cuanto cambie.

Usaremos **SignalR** en .NET y su cliente en Angular. A diferencia de implementaciones "Junior", haremos esto **Strongly Typed** (Tipado Fuerte) en el Backend y totalmente **Reactivo con Signals** en el Frontend, manejando reconexiones automáticas para soportar redes inestables.

---

# 📡 Módulo 5: Tiempo Real con SignalR (Real-Time Intelligence)

## 5.1. Backend: El Hub Táctico (Strongly Typed)

Primero, definamos el contrato. No usaremos cadenas mágicas (`SendAsync("ReceiveMessage")`). Definiremos una interfaz para que el compilador nos ayude.

**Paso 1: Definir el DTO de Telemetría** Crea `apps/api/DTOs/TelemetryDto.cs`.

C#

```
namespace Tactical.API.DTOs;

public record UnitTelemetryDto(
    Guid UnitId, 
    string Callsign, 
    double Latitude, 
    double Longitude, 
    int FuelPercentage,
    string Status, // "Active", "Moving", "Engaged"
    DateTime Timestamp
);
```

**Paso 2: Definir la Interfaz del Cliente y el Hub** Crea `apps/api/Hubs/TacticalHub.cs`.

C#

```
using Microsoft.AspNetCore.SignalR;
using Tactical.API.DTOs;

namespace Tactical.API.Hubs;

// Define qué métodos puede escuchar el Cliente (Frontend)
public interface ITacticalClient
{
    Task ReceiveTelemetry(UnitTelemetryDto data);
    Task ReceiveMissionUpdate(string message);
}

// El Hub hereda de Hub<T> para forzar el tipado
public class TacticalHub : Hub<ITacticalClient>
{
    // Método para que el cliente se una a un grupo específico (ej: una operación concreta)
    public async Task JoinOperationChannel(string operationId)
    {
        await Groups.AddToGroupAsync(Context.ConnectionId, operationId);
        await Clients.Caller.ReceiveMissionUpdate($"Conectado al canal seguro: {operationId}");
    }
}
```

---

## 5.2. Backend: Simulador de Batalla (Background Service)

Como no tenemos GPS reales conectados ahora mismo, crearemos un servicio en segundo plano (`BackgroundService`) que simule unidades moviéndose por un mapa.

Crea `apps/api/Services/BattlefieldSimulator.cs`.

C#

```
using Microsoft.AspNetCore.SignalR;
using Tactical.API.DTOs;
using Tactical.API.Hubs;

namespace Tactical.API.Services;

public class BattlefieldSimulator : BackgroundService
{
    private readonly IHubContext<TacticalHub, ITacticalClient> _hubContext;
    private readonly Random _random = new();
    
    // Coordenadas base (Ej: Cerca de Kiev o zona de entrenamiento OTAN)
    private double _baseLat = 50.4501;
    private double _baseLon = 30.5234;

    public BattlefieldSimulator(IHubContext<TacticalHub, ITacticalClient> hubContext)
    {
        _hubContext = hubContext;
    }

    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        // Simulamos 3 unidades
        var units = newList<Guid> { Guid.NewGuid(), Guid.NewGuid(), Guid.NewGuid() };
        
        // Loop infinito hasta que se apague la API
        using var timer = new PeriodicTimer(TimeSpan.FromMilliseconds(2000)); // Actualización cada 2s

        while (await timer.WaitForNextTickAsync(stoppingToken))
        {
            foreach (var unitId in units)
            {
                // Simular movimiento aleatorio (Brownian motion simplificado)
                _baseLat += (_random.NextDouble() - 0.5) * 0.001;
                _baseLon += (_random.NextDouble() - 0.5) * 0.001;

                var telemetry = new UnitTelemetryDto(
                    unitId,
                    $"Viper-{unitId.ToString().Substring(0, 4)}",
                    _baseLat,
                    _baseLon,
                    _random.Next(20, 100),
                    "Moving",
                    DateTime.UtcNow
                );

                // Enviar a TODOS los clientes conectados
                // En real, filtraríamos por grupos: _hubContext.Clients.Group("Op-1")...
                await _hubContext.Clients.All.ReceiveTelemetry(telemetry);
            }
        }
    }
}
```

**Paso 3: Registrar en `Program.cs`** Añade esto antes de `builder.Build()`:

C#

```
// 1. Registrar SignalR
builder.Services.AddSignalR();

// 2. Registrar el Simulador (Hosted Service)
builder.Services.AddHostedService<BattlefieldSimulator>();

// ... (build app) ...

// 3. Mapear el Endpoint
app.MapHub<TacticalHub>("/hubs/tactical");
```

---

## 5.3. Frontend: SignalR Client Service

Ahora pasamos a Angular. Necesitamos instalar la librería oficial.

**Terminal:**

Bash

```
npm install @microsoft/signalr
```

Vamos a crear un servicio en `libs/data-access` que gestione la conexión. Este servicio expondrá un **Signal** con la última telemetría recibida.

**Archivo: `libs/data-access/tactical-api/src/lib/services/realtime.service.ts`**

TypeScript

```
import { Injectable, signal } from '@angular/core';
import * as signalR from '@microsoft/signalr';
import { UnitTelemetry } from '../models/telemetry.model'; // (Definir interfaz abajo)

// Definición del Modelo en Frontend (debe coincidir con el DTO)
export interface UnitTelemetry {
  unitId: string;
  callsign: string;
  latitude: number;
  longitude: number;
  fuelPercentage: number;
  status: string;
  timestamp: string;
}

@Injectable({ providedIn: 'root' })
export class RealtimeService {
  private hubConnection: signalR.HubConnection;

  // STATE: Map para acceso O(1) por ID de unidad.
  // Usamos un Signal que contiene un Map inmutable.
  unitPositions = signal<Map<string, UnitTelemetry>>(new Map());

  // Status de conexión para la UI
  connectionStatus = signal<'connected' | 'disconnected' | 'reconnecting'>('disconnected');

  constructor() {
    this.hubConnection = new signalR.HubConnectionBuilder()
      .withUrl('http://localhost:5001/hubs/tactical') // URL del Hub
      .withAutomaticReconnect() // Resiliencia: Reintenta si cae la red
      .build();

    this.setupListeners();
    this.startConnection();
  }

  private setupListeners() {
    // Escuchar el método 'ReceiveTelemetry' definido en el Backend
    this.hubConnection.on('ReceiveTelemetry', (data: UnitTelemetry) => {
      console.log(`📡 Telemetría recibida: ${data.callsign}`);
      
      // Actualización Inmutable del Signal Map
      this.unitPositions.update(currentMap => {
        const newMap = new Map(currentMap);
        newMap.set(data.unitId, data);
        return newMap;
      });
    });

    this.hubConnection.onreconnecting(() => this.connectionStatus.set('reconnecting'));
    this.hubConnection.onreconnected(() => this.connectionStatus.set('connected'));
    this.hubConnection.onclose(() => this.connectionStatus.set('disconnected'));
  }

  private startConnection() {
    this.hubConnection
      .start()
      .then(() => {
        console.log('✅ Conectado al Tactical Hub');
        this.connectionStatus.set('connected');
      })
      .catch(err => console.error('❌ Error conectando SignalR:', err));
  }
}
```

---

## 5.4. Visualización de Datos Crudos (Debugging UI)

Antes de meter el mapa complejo (OpenLayers), verifiquemos que los datos fluyen mostrando una tabla en tiempo real. Esto es una buena práctica: **Fail Fast**.

Crea un nuevo componente en `libs/features/tactical-map` (o úsalo temporalmente en mission-control).

**Archivo: `libs/features/tactical-map/src/lib/live-feed/live-feed.component.ts`**

TypeScript

```
import { Component, inject, computed } from '@angular/core';
import { CommonModule } from '@angular/common';
import { RealtimeService } from '@tactical-c2/data-access-api';

@Component({
  selector: 'feat-live-feed',
  standalone: true,
  imports: [CommonModule],
  template: `
    <div class="bg-black text-green-400 p-4 rounded font-mono text-xs shadow-inner border border-green-900">
      <div class="flex justify-between items-center mb-2 border-b border-green-800 pb-1">
        <h3 class="uppercase font-bold">Stream de Telemetría Encriptado</h3>
        
        <span [ngClass]="{
          'text-green-500': service.connectionStatus() === 'connected',
          'text-red-500': service.connectionStatus() === 'disconnected',
          'text-yellow-500': service.connectionStatus() === 'reconnecting'
        }">
          ● {{ service.connectionStatus() | uppercase }}
        </span>
      </div>

      <div class="space-y-1 h-32 overflow-y-auto">
        @for (unit of unitList(); track unit.unitId) {
          <div class="grid grid-cols-4 gap-2 animate-pulse">
            <span>[{{ unit.callsign }}]</span>
            <span>LAT: {{ unit.latitude | number:'1.4-4' }}</span>
            <span>LON: {{ unit.longitude | number:'1.4-4' }}</span>
            <span [class.text-red-500]="unit.fuelPercentage < 30">
              FUEL: {{ unit.fuelPercentage }}%
            </span>
          </div>
        } @empty {
          <div class="text-gray-600 italic">Esperando señal de satélite...</div>
        }
      </div>
    </div>
  `
})
export class LiveFeedComponent {
  public service = inject(RealtimeService);

  // Transformamos el Map a Array para iterar en el template fácilmente
  unitList = computed(() => Array.from(this.service.unitPositions().values()));
}
```

Ahora, inserta `<feat-live-feed />` en tu `MissionListComponent` o en el `AppComponent` principal para verlo funcionar.

---

## 5.5. Resiliencia Táctica (Retry Policy)

Un sistema militar no puede fallar si SignalR no conecta a la primera. En el constructor de `RealtimeService`, el método `.withAutomaticReconnect()` ya maneja reintentos con backoff (0s, 2s, 10s, 30s).

Sin embargo, si queremos control manual, podemos configurar la política:

TypeScript

```
// En RealtimeService
.withAutomaticReconnect({
    nextRetryDelayInMilliseconds: retryContext => {
        if (retryContext.elapsedMilliseconds < 60000) {
            // Reintenta rápido el primer minuto
            return Math.random() * 2000; 
        } else {
            // Si falla mucho, espera más
            return 10000;
        }
    }
})
```

---

### ✅ Checkpoint Módulo 5

1. **Backend Simulator:** Tenemos un "fantasma" moviéndose en el servidor (.NET BackgroundService).
    
2. **WebSockets:** Canal abierto y tipado.
    
3. **Frontend Reactivo:** `RealtimeService` escucha y actualiza un `Signal<Map>`.
    
4. **Visualización:** Vemos los números cambiar en la pantalla sin refrescar el navegador.
    

**Prueba:** Ejecuta la API (`dotnet run`) y el Frontend (`nx serve`). Deberías ver la consola negra estilo "Hacker" actualizando coordenadas cada 2 segundos.

¡Esto es la base de un C2 System! Pero ver números es aburrido. Un General quiere ver mapas.

**Siguiente paso:** Integrar **OpenLayers**. Vamos a pintar esos puntos en un mapa real, manejar proyecciones y optimizar el renderizado para que no se congele si hay 1000 unidades.

¿Listo para la cartografía avanzada? **Di "Empieza el Módulo 6"**.