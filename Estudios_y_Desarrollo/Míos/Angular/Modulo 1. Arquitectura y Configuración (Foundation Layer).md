¡Entendido! Iniciamos la operación. Como Tech Lead, sabes que **la batalla se gana o se pierde en la fase de preparación**. Una mala estructura inicial se convierte en deuda técnica masiva a los 6 meses.

En este módulo, estableceremos los cimientos de **Grado Militar** para nuestro "Tactical Command Dashboard".

---

# 🛠️ Módulo 1: Arquitectura y Configuración (Foundation Layer)

## 1.1. Visión Arquitectónica (The "Senior" Why)

Antes de ejecutar un comando, entendamos la estrategia. En proyectos de Defensa/Enterprise, la escalabilidad y la mantenibilidad son prioritarias sobre la velocidad inicial.

- **Nx Monorepo:** No es solo una carpeta con proyectos. Nos permite compartir **Contratos de Datos (Interfaces/DTOs)** entre el Backend y el Frontend, y aplicar **Limites de Módulo (Module Boundaries)** para evitar el "Spaghetti Code".
    
- **Vertical Slice Architecture (Backend):** En lugar de organizar por capas técnicas (Controllers, Services, Repos), organizaremos por **Features** (Missions, Units). Esto reduce el acoplamiento.
    
- **Librerías sobre Apps (Frontend):** La aplicación Angular principal (`apps/dashboard`) será solo un contenedor vacío. Toda la lógica residirá en librerías (`libs/`) categorizadas por dominio. Esto facilita testear y reutilizar código.
    

---

## 1.2. Infraestructura Local (Docker & Database)

Primero, necesitamos nuestro "Data Center" local. Usaremos Docker Compose para levantar SQL Server y Redis (necesario más adelante como Backplane para SignalR si escalamos horizontalmente).

**Paso 1:** Crea una carpeta raíz para tu proyecto en tu máquina local llamada `tactical-c2-system`. **Paso 2:** Dentro, crea un archivo `docker-compose.yml`.

YAML

```
version: '3.8'

services:
  # Base de datos principal: SQL Server 2022
  # En entornos reales usaríamos Azure SQL Database
  db:
    image: mcr.microsoft.com/mssql/server:2022-latest
    container_name: tactical_db
    environment:
      - ACCEPT_EULA=Y
      - MSSQL_SA_PASSWORD=Tactical_P@ssw0rd! # En prod usar KeyVault
    ports:
      - "1433:1433"
    volumes:
      - sqlvolume:/var/opt/mssql

  # Redis: Para caché distribuida y SignalR Backplane
  redis:
    image: redis:alpine
    container_name: tactical_redis
    ports:
      - "6379:6379"

  # Seq: Para Logging centralizado (Simulando Application Insights)
  seq:
    image: datalust/seq
    container_name: tactical_logs
    environment:
      - ACCEPT_EULA=Y
    ports:
      - "5341:80"

volumes:
  sqlvolume:
```

**Ejecuta:** `docker-compose up -d` para tener la infraestructura lista.

---

## 1.3. Setup del Monorepo Nx

Vamos a crear el workspace. Usaremos `npm` (o `pnpm` si prefieres velocidad).

**Comando en Terminal:**

Bash

```
# 1. Crear el workspace vacío
npx create-nx-workspace@latest tactical-c2 --preset=apps --interactive=false --nxCloud=skip

cd tactical-c2

# 2. Instalar plugins de Angular
npm install -D @nx/angular @angular/core @angular/cli

# 3. Generar la aplicación Angular (Host)
# --standalone: Usa Standalone Components por defecto (Adiós NgModules)
# --style=scss: Estándar en industria
npx nx g @nx/angular:app apps/dashboard --style=scss --ssr=false --standalone --e2eTestRunner=cypress --bundler=esbuild
```

_Nota: El Backend (.NET) lo crearemos manualmente dentro de la estructura para tener control total sobre el archivo `.sln`, en lugar de depender de plugins de terceros que a veces ocultan complejidad._

---

## 1.4. Estructura de Carpetas DDD (Frontend)

Aquí es donde te diferencias de un Junior. No pondremos todo en `src/app`. Vamos a estructurar las librerías por tipo y dominio.

**Esquema Mental de Carpetas:**

- `libs/features`: Lógica de negocio visual (Smart Components).
    
- `libs/ui`: Componentes puros (Botones, Cards) sin lógica de negocio (Dumb Components).
    
- `libs/data-access`: Servicios, APIs, Estado (Signals).
    
- `libs/util`: Helpers puros.
    

**Ejecuta estos comandos para generar la estructura inicial:**

Bash

```
# Feature: Mapa Táctico (Donde vivirá OpenLayers)
npx nx g @nx/angular:lib libs/features/tactical-map --directory=libs/features/tactical-map --simpleName

# Feature: Gestión de Misiones
npx nx g @nx/angular:lib libs/features/mission-control --directory=libs/features/mission-control --simpleName

# UI: Componentes compartidos (Design System)
npx nx g @nx/angular:lib libs/ui/kit --directory=libs/ui/kit --simpleName

# Data Access: Comunicación con API y Store
npx nx g @nx/angular:lib libs/data-access/tactical-api --directory=libs/data-access/tactical-api --simpleName
```

---

## 1.5. Diseño del Dominio (.NET Solution)

Ahora, configuremos el esqueleto del Backend dentro del mismo repositorio.

**Estructura de carpetas deseada:**

```
/apps
  /api (Web API .NET 8)
  /dashboard (Angular)
/libs (Angular shared libs)
/Tactical.sln (Solución .NET global)
```

**Comandos para crear el Backend:**

Bash

```
# Desde la raíz del workspace
mkdir -p apps/api

# Crear solución vacía
dotnet new sln -n TacticalC2 -o .

# Crear la Web API (.NET 8)
# --use-controllers: Aunque usaremos Minimal APIs, los controllers siguen siendo útiles para organizar features grandes
dotnet new webapi -n Tactical.API -o apps/api --use-controllers

# Agregar proyecto a la solución
dotnet sln add apps/api/Tactical.API.csproj
```

### Definición de Entidades (Domain Modeling)

Dentro de `apps/api`, vamos a definir nuestras entidades base. Aunque luego moveremos esto a una estructura más limpia, necesitamos visualizar los datos ahora.

Crea una carpeta `apps/api/Domain/Entities` y añade los siguientes archivos. Usaremos **Records** para objetos inmutables donde sea posible, o Clases limpias para EF Core.

**Archivo:** `apps/api/Domain/Entities/TacticalUnit.cs` _Representa un activo en el campo (Tanque, Soldado, Drone)._

C#

```
namespace Tactical.API.Domain.Entities;

public enum UnitStatus { Active, Offline, Engaged, Compromised }
public enum UnitType { Infantry, Vehicle, Drone, Headquarters }

public class TacticalUnit
{
    public Guid Id { get; set; }
    public string Callsign { get; set; } = string.Empty; // Ejemplo: "Alpha-1"
    public UnitType Type { get; set; }
    public UnitStatus Status { get; set; }
    
    // Posición Geoespacial Actual
    public double Latitude { get; set; }
    public double Longitude { get; set; }
    
    public DateTime LastUpdate { get; set; }

    // Relaciones
    public Guid? CurrentMissionId { get; set; }
    public virtual Mission? CurrentMission { get; set; }
}
```

**Archivo:** `apps/api/Domain/Entities/Mission.cs` _Representa un objetivo operativo._

C#

```
namespace Tactical.API.Domain.Entities;

public enum MissionStatus { Planned, InProgress, Completed, Aborted }

public class Mission
{
    public Guid Id { get; set; }
    public string Title { get; set; } = string.Empty;
    public string Description { get; set; } = string.Empty;
    public MissionStatus Status { get; set; }
    public int RiskLevel { get; set; } // 1-5
    
    public DateTime CreatedAt { get; set; }
    
    // Relación con unidades asignadas
    public virtual ICollection<TacticalUnit> AssignedUnits { get; set; } = new List<TacticalUnit>();
}
```

---

## 1.6. Strict Linting (Module Boundaries)

Para finalizar el setup, configuremos `ESLint` para asegurar que la arquitectura se respeta. Un desarrollador Junior podría importar `features/tactical-map` dentro de `ui/kit`, creando una dependencia circular. Vamos a prohibir eso.

Edita el archivo `eslint.json` (o `.eslintrc.json`) en la raíz del workspace. Busca la sección de `@nx/enforce-module-boundaries`.

JSON

```
"rules": {
  "@nx/enforce-module-boundaries": [
    "error",
    {
      "enforceBuildableLibDependency": true,
      "allow": [],
      "depConstraints": [
        {
          "sourceTag": "type:feature",
          "onlyDependOnLibsWithTags": ["type:ui", "type:data-access", "type:util"]
        },
        {
          "sourceTag": "type:ui",
          "onlyDependOnLibsWithTags": ["type:util"]
        },
        {
          "sourceTag": "type:data-access",
          "onlyDependOnLibsWithTags": ["type:util"]
        }
      ]
    }
  ]
}
```

_Nota: Necesitarás añadir los `tags` correspondientes en el `project.json` de cada librería creada en el paso 1.4 (ej: `"tags": ["type:feature"]`). Esto lo haremos sobre la marcha, pero tenlo en cuenta._

---

### ✅ Checkpoint Módulo 1

Hemos logrado:

1. Infraestructura de datos (SQL/Redis) lista en Docker.
    
2. Monorepo Nx configurado con Angular Standalone.
    
3. Backend .NET 8 inicializado en la solución.
    
4. Librerías Front organizadas por DDD.
    
5. Modelo de Dominio (Entidades) definido.
    

**Siguiente paso:** Escribir código Backend real. Vamos a configurar EF Core, las Migraciones y los Servicios Base para exponer estas entidades.

¿Todo listo en tu entorno? **Di "Empieza el Módulo 2"** para construir la API.