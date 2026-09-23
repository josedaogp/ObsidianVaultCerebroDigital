¡Recibido! Entramos en la fase final.

En el sector Defensa/OTAN, **"funciona en mi máquina" es inaceptable**. El software controla operaciones críticas. Un fallo en la UI que oculte una unidad enemiga o un error en el Backend que pierda una misión puede tener consecuencias reales.

En este módulo, implementaremos una estrategia de **Calidad Total**:

1. **Unit Testing:** Para asegurar que la lógica aislada (Servicios/Stores) es matemáticamente correcta.
    
2. **E2E Testing:** Para asegurar que todos los engranajes (Front, Back, DB) giran juntos correctamente.
    

---

# 🛡️ Módulo 8: Calidad y Testing (Mission Assurance)

![Imagen de software testing pyramid unit integration e2e](https://encrypted-tbn0.gstatic.com/licensed-image?q=tbn:ANd9GcToTGgxxlwSPoE5scytACmus0Mv19z4_1hhqM_Uu_020MxuGnq1w17mzTZLZu-5ArH-9AVQxyVEi0LvVHgPUZJfSDawyS66Dy9vGrvUEO-6c95396o)

Getty Images

## 8.1. Frontend Unit Testing: Jest & Signals

Angular y Nx ya vienen configurados con **Jest**. Es más rápido que Karma y permite "Mocking" fácil. Vamos a testear el cerebro de nuestra aplicación: el `MissionStore`.

**Reto:** ¿Cómo testeamos Signals? **Solución:** Igual que cualquier valor, pero accediendo a ellos como funciones `()`.

**Archivo: `libs/data-access/tactical-api/src/lib/stores/mission.store.spec.ts`**

TypeScript

```
import { TestBed } from '@angular/core/testing';
import { MissionStore } from './mission.store';
import { MissionApiService } from '../services/mission-api.service';
import { of, throwError } from 'rxjs';
import { MissionSummary } from '../models/mission.model';

// 1. Mock Data (Datos de prueba)
const mockMissions: MissionSummary[] = [
  { id: '1', title: 'Test Mission', status: 'Planned', assignedUnitCount: 0 }
];

describe('MissionStore', () => {
  let store: MissionStore;
  let apiServiceMock: any;

  beforeEach(() => {
    // 2. Crear el Mock del Servicio API
    // No queremos hacer llamadas HTTP reales en un Unit Test
    apiServiceMock = {
      getMissions: jest.fn().mockReturnValue(of(mockMissions))
    };

    TestBed.configureTestingModule({
      providers: [
        MissionStore,
        { provide: MissionApiService, useValue: apiServiceMock }
      ]
    });

    store = TestBed.inject(MissionStore);
  });

  it('should initialize with default state', () => {
    expect(store.missions()).toEqual([]);
    expect(store.isLoading()).toBe(false);
    expect(store.error()).toBeNull();
  });

  it('should load missions successfully', () => {
    // Act: Ejecutar la acción
    store.loadMissions();

    // Assert: Verificar que el Signal cambió
    expect(apiServiceMock.getMissions).toHaveBeenCalled();
    expect(store.missions()).toEqual(mockMissions);
    expect(store.isLoading()).toBe(false);
    expect(store.error()).toBeNull();
  });

  it('should handle API errors gracefully', () => {
    // Arrange: Simular error
    apiServiceMock.getMissions.mockReturnValue(throwError(() => new Error('Server Down')));

    // Act
    store.loadMissions();

    // Assert
    expect(store.missions()).toEqual([]); // Debe seguir vacío o mantener anterior
    expect(store.error()).toBe('Fallo en la conexión táctica.'); // El mensaje definido en el store
    expect(store.isLoading()).toBe(false);
  });

  it('should calculate active missions correctly (Computed Signal)', () => {
    // Inyectamos datos directos para probar el computed
    // Nota: Como 'state' es privado, lo hacemos a través del efecto de carga o mockeando internamente,
    // pero aquí asumiremos que loadMissions funciona.
    store.loadMissions();
    
    // Asumiendo que mockMissions tiene 1 misión 'Planned' (Activa)
    expect(store.activeMissionsCount()).toBe(1);
  });
});
```

Para ejecutar los tests:

Bash

```
npx nx test data-access-tactical-api
```

---

## 8.2. Backend Unit Testing: xUnit & Moq

En .NET, el estándar es **xUnit**. Vamos a asegurar que el `MissionService` valida correctamente antes de guardar en BBDD.

**Terminal (Setup en `apps/api`):**

Bash

```
dotnet add package xunit
dotnet add package Moq
dotnet add package Microsoft.NET.Test.Sdk
```

**Archivo: `apps/api.tests/Services/MissionServiceTests.cs`** (Crear proyecto de test aparte idealmente, aquí simplificado)

C#

```
using Moq;
using Xunit;
using Tactical.API.Services;
using Tactical.API.Domain.Interfaces;
using Tactical.API.Domain.Entities;
using Tactical.API.DTOs;

public class MissionServiceTests
{
    private readonly Mock<IGenericRepository<Mission>> _mockRepo;
    private readonly Mock<IUnitOfWork> _mockUow;
    private readonly MissionService _service;

    public MissionServiceTests()
    {
        _mockRepo = new Mock<IGenericRepository<Mission>>();
        _mockUow = new Mock<IUnitOfWork>();
        _service = new MissionService(_mockRepo.Object, _mockUow.Object);
    }

    [Fact]
    public async Task CreateMission_ShouldThrow_WhenRiskLevelIsTooHigh()
    {
        // Arrange
        var dto = new CreateMissionDto("Suicide Mission", "Impossible", 10); // Risk 10 (Max es 5)

        // Act & Assert
        await Assert.ThrowsAsync<ArgumentException>(() => _service.CreateMissionAsync(dto));
        
        // Verificar que NUNCA se llamó a la base de datos
        _mockRepo.Verify(r => r.AddAsync(It.IsAny<Mission>()), Times.Never);
    }

    [Fact]
    public async Task CreateMission_ShouldSucceed_WhenDataIsValid()
    {
        // Arrange
        var dto = new CreateMissionDto("Routine Patrol", "Easy peasy", 1);

        // Act
        var result = await _service.CreateMissionAsync(dto);

        // Assert
        Assert.NotNull(result);
        Assert.Equal("Routine Patrol", result.Title);
        _mockRepo.Verify(r => r.AddAsync(It.IsAny<Mission>()), Times.Once);
        _mockUow.Verify(u => u.SaveChangesAsync(default), Times.Once);
    }
}
```

---

## 8.3. E2E Testing: Cypress (Simulando al Soldado)

Aquí probamos el flujo completo. **Login -> Ver Dashboard**. Nx configura Cypress automáticamente en `apps/dashboard-e2e`.

**Estrategia Senior:** No uses la API real para E2E si puedes evitarlo. Hace los tests lentos y "flaky" (que fallan a veces sí, a veces no). Usaremos `cy.intercept` para simular la red.

**Archivo: `apps/dashboard-e2e/src/e2e/app.cy.ts`**

TypeScript

```
describe('Tactical Dashboard Flow', () => {
  
  beforeEach(() => {
    // 1. Network Stubbing (Simulamos el Backend)
    
    // Interceptar Login
    cy.intercept('POST', '**/auth/login', {
      statusCode: 200,
      body: {
        token: 'fake-jwt-token',
        role: 'COMMANDER',
        expiresIn: 3600
      }
    }).as('loginRequest');

    // Interceptar Misiones
    cy.intercept('GET', '**/missions', {
      statusCode: 200,
      body: [
        { id: '101', title: 'Operación Cypress', status: 'InProgress', assignedUnitCount: 5 },
        { id: '102', title: 'Rescate Alpha', status: 'Planned', assignedUnitCount: 2 }
      ]
    }).as('getMissions');

    cy.visit('/login');
  });

  it('should allow login and display dashboard data', () => {
    // 1. Rellenar formulario de Login
    cy.get('input[type="text"]').type('admin');
    cy.get('input[type="password"]').type('admin');
    cy.get('button[type="submit"]').click();

    // 2. Verificar redirección
    cy.wait('@loginRequest');
    cy.url().should('include', '/missions');

    // 3. Verificar que cargan las tarjetas (Tactical Cards)
    cy.wait('@getMissions');
    cy.get('ui-tactical-card').should('have.length', 2);
    
    // 4. Verificar contenido específico
    cy.contains('Operación Cypress').should('be.visible');
    cy.contains('InProgress').should('have.class', 'bg-red-100'); // Verificamos clases de alerta
  });

  it('should block unauthorized access', () => {
    // Limpiar storage
    cy.clearLocalStorage();
    
    // Intentar ir directo sin login
    cy.visit('/missions');
    
    // Debe rebotar al login
    cy.url().should('include', '/login');
  });
});
```

Para ejecutar Cypress visualmente:

Bash

```
npx nx e2e dashboard-e2e --watch
```

---

## 🎓 Cierre del Curso: Tactical Command Dashboard

¡Enhorabuena, Tech Lead! Has completado el desarrollo del **Tactical Command Dashboard**.

Has construido mucho más que una simple web. Has creado una arquitectura preparada para el futuro y para la escala empresarial:

1. **Monorepo Nx:** Código modular y organizado.
    
2. **Backend .NET 8 Clean Arch:** Robusto, seguro y performante.
    
3. **Angular 18 Bleeding Edge:** Uso de Standalone Components, Signals y Control Flow.
    
4. **GIS:** Mapas tácticos de alto rendimiento fuera de la zona de Angular.
    
5. **Tiempo Real:** SignalR con simulaciones de batalla.
    
6. **Seguridad:** JWT y RBAC funcional.
    
7. **Calidad:** Tests unitarios y E2E cubriendo flujos críticos.
    

### 💼 Preparación para tu Entrevista

Cuando presentes esto en tu entrevista para el sector Defensa:

1. **No muestres solo la UI.** Muestra la estructura de carpetas `libs/`. Explica por qué separaste `feature` de `ui`.
    
2. **Habla de Signals.** Explica cómo reducen el "Change Detection Cycles" comparado con Angular viejo.
    
3. **Muestra el `BattlefieldSimulator`.** Demuestra que entiendes de concurrencia y procesos en segundo plano en .NET.
    
4. **Menciona la seguridad.** Destaca que el Frontend no "decide" la seguridad, solo la refleja, y que el Backend valida cada token.
    

**Este proyecto es tu carta de presentación.** Tienes el código, tienes la arquitectura y tienes el conocimiento.

**¡Buena suerte en la operación, Comandante!** 🚀