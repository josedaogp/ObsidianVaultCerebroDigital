Iniciamos el **Módulo 2**. Aquí es donde la lógica de negocio cobra vida.

En el sector defensa, un Backend no es solo un "CRUD". Es un sistema que garantiza la **integridad de datos**, la **trazabilidad** y el **rendimiento**.

El objetivo de este módulo es construir una API robusta, limpia y testeable, desacoplando la base de datos de la vista pública mediante DTOs y capas de servicio.

---

# ⚙️ Módulo 2: El Backend (.NET 8 Core)

## 2.1. Preparación del Entorno y NuGets

Primero, instalemos las herramientas necesarias en nuestro proyecto `Tactical.API`. Necesitamos EF Core para SQL Server y las herramientas de diseño.

**Terminal (en la carpeta raíz):**

Bash

```
cd apps/api
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet add package Microsoft.EntityFrameworkCore.Design
# Para validaciones fluidas en los DTOs (Estándar de industria)
dotnet add package FluentValidation.DependencyInjectionExtensions
```

---

## 2.2. Persistence Layer: DbContext & Configurations

Un error común de Junior es poner toda la configuración de la BBDD en el método `OnModelCreating`. Como Seniors, usaremos **`IEntityTypeConfiguration`** para mantener el `DbContext` limpio (Single Responsibility Principle).

**Paso 1: Configuración de Entidades** Crea `apps/api/Infrastructure/Persistence/Configurations/MissionConfiguration.cs`.

C#

```
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata.Builders;
using Tactical.API.Domain.Entities;

namespace Tactical.API.Infrastructure.Persistence.Configurations;

public class MissionConfiguration : IEntityTypeConfiguration<Mission>
{
    public void Configure(EntityTypeBuilder<Mission> builder)
    {
        builder.HasKey(m => m.Id);

        builder.Property(m => m.Title)
            .HasMaxLength(100)
            .IsRequired();

        builder.Property(m => m.Description)
            .HasMaxLength(500);
            
        // Indexado para búsquedas rápidas por estado (Performance)
        builder.HasIndex(m => m.Status);

        // Relación Uno a Muchos
        builder.HasMany(m => m.AssignedUnits)
            .WithOne(u => u.CurrentMission)
            .HasForeignKey(u => u.CurrentMissionId)
            .OnDelete(DeleteBehavior.SetNull); // Si borro misión, no borro la unidad
    }
}
```

**Paso 2: El DbContext** Crea `apps/api/Infrastructure/Persistence/TacticalDbContext.cs`.

C#

```
using Microsoft.EntityFrameworkCore;
using Tactical.API.Domain.Entities;
using System.Reflection;

namespace Tactical.API.Infrastructure.Persistence;

public class TacticalDbContext : DbContext
{
    public TacticalDbContext(DbContextOptions<TacticalDbContext> options) : base(options) { }

    public DbSet<Mission> Missions { get; set; }
    public DbSet<TacticalUnit> TacticalUnits { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);
        // Aplica automáticamente todas las clases de configuración creadas arriba
        modelBuilder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());
    }
}
```

---

## 2.3. Pattern: Generic Repository (¿Necesario?)

_Debate Senior:_ ¿EF Core no es ya un Repositorio? _Respuesta:_ Sí. Pero en entornos complejos (Defensa), a menudo necesitamos encapsular lógica de **filtrado de seguridad** (ej: Multitenancy o Row-Level Security) o facilitar el **Unit Testing** con Mocks sin depender de una BBDD en memoria. Usaremos una abstracción ligera.

Crea `apps/api/Domain/Interfaces/IGenericRepository.cs` y su implementación en `Infrastructure`.

C#

```
// Interface
using System.Linq.Expressions;

namespace Tactical.API.Domain.Interfaces;

public interface IGenericRepository<T> where T : class
{
    Task<IEnumerable<T>> GetAllAsync();
    Task<T?> GetByIdAsync(Guid id);
    Task<IEnumerable<T>> FindAsync(Expression<Func<T, bool>> predicate);
    Task AddAsync(T entity);
    void Update(T entity);
    void Remove(T entity);
}

// Implementación (Infrastructure/Repositories/GenericRepository.cs)
using Microsoft.EntityFrameworkCore;
using Tactical.API.Infrastructure.Persistence;

namespace Tactical.API.Infrastructure.Repositories;

public class GenericRepository<T> : IGenericRepository<T> where T : class
{
    protected readonly TacticalDbContext _context;
    
    public GenericRepository(TacticalDbContext context)
    {
        _context = context;
    }

    public async Task<IEnumerable<T>> GetAllAsync() => await _context.Set<T>().ToListAsync();
    
    public async Task<T?> GetByIdAsync(Guid id) => await _context.Set<T>().FindAsync(id);

    public async Task<IEnumerable<T>> FindAsync(Expression<Func<T, bool>> predicate)
    {
        return await _context.Set<T>().Where(predicate).ToListAsync();
    }

    public async Task AddAsync(T entity) => await _context.Set<T>().AddAsync(entity);

    public void Update(T entity) => _context.Set<T>().Update(entity);

    public void Remove(T entity) => _context.Set<T>().Remove(entity);
}
```

Necesitamos un **UnitOfWork** para transacciones atómicas (guardar cambios de golpe). Crea `apps/api/Domain/Interfaces/IUnitOfWork.cs`.

C#

```
public interface IUnitOfWork
{
    Task<int> SaveChangesAsync(CancellationToken cancellationToken = default);
}
```

_(Implementa esto en el DbContext o una clase wrapper. Por brevedad asumiremos que inyectamos DbContext como IUnitOfWork)._

---

## 2.4. Data Contracts: DTOs (Records)

Jamás expongas tus Entidades de EF Core directamente en la API. Eso crea acoplamiento y fugas de seguridad (Over-posting). Usaremos **C# Records** por su inmutabilidad y sintaxis concisa.

Crea `apps/api/DTOs/MissionDtos.cs`.

C#

```
using Tactical.API.Domain.Entities;

namespace Tactical.API.DTOs;

// DTO para listar (Data mínima)
public record MissionSummaryDto(Guid Id, string Title, string Status, int AssignedUnitCount);

// DTO para detalle (Data completa)
public record MissionDetailDto(Guid Id, string Title, string Description, string Status, int RiskLevel, List<string> UnitCallsigns);

// DTO para crear (Input)
public record CreateMissionDto(string Title, string Description, int RiskLevel);

// DTO para actualizar estado
public record UpdateMissionStatusDto(MissionStatus NewStatus);
```

---

## 2.5. Service Layer: Lógica de Negocio

Aquí reside la inteligencia. El controlador solo recibe HTTP, el servicio ejecuta la lógica.

Crea `apps/api/Services/MissionService.cs`.

C#

```
using Tactical.API.Domain.Entities;
using Tactical.API.Domain.Interfaces;
using Tactical.API.DTOs;

namespace Tactical.API.Services;

public interface IMissionService 
{
    Task<MissionDetailDto> CreateMissionAsync(CreateMissionDto dto);
    Task<IEnumerable<MissionSummaryDto>> GetAllMissionsAsync();
}

public class MissionService : IMissionService
{
    private readonly IGenericRepository<Mission> _repository;
    private readonly IUnitOfWork _unitOfWork;

    // Constructor Injection
    public MissionService(IGenericRepository<Mission> repository, IUnitOfWork unitOfWork)
    {
        _repository = repository;
        _unitOfWork = unitOfWork;
    }

    public async Task<MissionDetailDto> CreateMissionAsync(CreateMissionDto dto)
    {
        // 1. Lógica de Negocio / Validación
        if (dto.RiskLevel > 5) throw new ArgumentException("Risk level cannot exceed 5");

        // 2. Mapping (Manual es más rápido que AutoMapper para casos simples)
        var mission = new Mission
        {
            Id = Guid.NewGuid(),
            Title = dto.Title,
            Description = dto.Description,
            RiskLevel = dto.RiskLevel,
            Status = MissionStatus.Planned,
            CreatedAt = DateTime.UtcNow
        };

        // 3. Persistencia
        await _repository.AddAsync(mission);
        await _unitOfWork.SaveChangesAsync();

        // 4. Return DTO
        return new MissionDetailDto(mission.Id, mission.Title, mission.Description, mission.Status.ToString(), mission.RiskLevel, new List<string>());
    }

    public async Task<IEnumerable<MissionSummaryDto>> GetAllMissionsAsync()
    {
        var missions = await _repository.GetAllAsync();
        
        // Proyección
        return missions.Select(m => new MissionSummaryDto(
            m.Id, 
            m.Title, 
            m.Status.ToString(), 
            m.AssignedUnits?.Count ?? 0
        ));
    }
}
```

---

## 2.6. API Layer: Controllers (The Clean Way)

Usaremos Controllers estándar para mantener una estructura organizada y compatible con Swagger/OpenAPI de forma nativa y clara.

Crea `apps/api/Controllers/MissionsController.cs`.

C#

```
using Microsoft.AspNetCore.Mvc;
using Tactical.API.DTOs;
using Tactical.API.Services;

namespace Tactical.API.Controllers;

[ApiController]
[Route("api/v1/[controller]")]
public class MissionsController : ControllerBase
{
    private readonly IMissionService _missionService;

    public MissionsController(IMissionService missionService)
    {
        _missionService = missionService;
    }

    [HttpPost]
    [ProducesResponseType(typeof(MissionDetailDto), StatusCodes.Status201Created)]
    public async Task<IActionResult> Create([FromBody] CreateMissionDto dto)
    {
        var result = await _missionService.CreateMissionAsync(dto);
        return CreatedAtAction(nameof(GetAll), new { id = result.Id }, result);
    }

    [HttpGet]
    [ProducesResponseType(typeof(IEnumerable<MissionSummaryDto>), StatusCodes.Status200OK)]
    public async Task<IActionResult> GetAll()
    {
        var result = await _missionService.GetAllMissionsAsync();
        return Ok(result);
    }
}
```

---

## 2.7. Modern Error Handling (IExceptionHandler)

En .NET 8, tenemos una forma nueva y elegante de manejar errores globalmente sin llenar los controladores de `try-catch`.

Crea `apps/api/Infrastructure/GlobalExceptionHandler.cs`.

C#

```
using Microsoft.AspNetCore.Diagnostics;
using Microsoft.AspNetCore.Mvc;

namespace Tactical.API.Infrastructure;

public class GlobalExceptionHandler : IExceptionHandler
{
    private readonly ILogger<GlobalExceptionHandler> _logger;

    public GlobalExceptionHandler(ILogger<GlobalExceptionHandler> logger)
    {
        _logger = logger;
    }

    public async ValueTask<bool> TryHandleAsync(
        HttpContext httpContext,
        Exception exception,
        CancellationToken cancellationToken)
    {
        _logger.LogError(exception, "Exception occurred: {Message}", exception.Message);

        var problemDetails = new ProblemDetails
        {
            Status = StatusCodes.Status500InternalServerError,
            Title = "Server Error",
            Detail = "An internal error occurred in the Tactical C2 System."
        };

        // Personalización por tipo de excepción
        if (exception is ArgumentException)
        {
            problemDetails.Status = StatusCodes.Status400BadRequest;
            problemDetails.Title = "Validation Error";
            problemDetails.Detail = exception.Message;
        }

        httpContext.Response.StatusCode = problemDetails.Status.Value;
        await httpContext.Response.WriteAsJsonAsync(problemDetails, cancellationToken);

        return true;
    }
}
```

---

## 2.8. Wiring It All Up (Program.cs)

Finalmente, inyectamos las dependencias. Edita `apps/api/Program.cs`.

C#

```
using Microsoft.EntityFrameworkCore;
using Tactical.API.Domain.Interfaces;
using Tactical.API.Infrastructure;
using Tactical.API.Infrastructure.Persistence;
using Tactical.API.Infrastructure.Repositories;
using Tactical.API.Services;

var builder = WebApplication.CreateBuilder(args);

// 1. DB Context
builder.Services.AddDbContext<TacticalDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

// 2. DI Registration
builder.Services.AddScoped(typeof(IGenericRepository<>), typeof(GenericRepository<>));
builder.Services.AddScoped<IUnitOfWork>(provider => provider.GetRequiredService<TacticalDbContext>() as IUnitOfWork ?? throw new InvalidOperationException("DbContext does not implement IUnitOfWork")); 
// Nota: Asegúrate de que TacticalDbContext implemente IUnitOfWork (añádele la interfaz en el paso 2.2)

builder.Services.AddScoped<IMissionService, MissionService>();

// 3. Exception Handler (.NET 8)
builder.Services.AddExceptionHandler<GlobalExceptionHandler>();
builder.Services.AddProblemDetails();

builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

// CORS (Para permitir llamadas desde Angular Localhost)
builder.Services.AddCors(options => {
    options.AddPolicy("AllowAngular",
        policy => policy.WithOrigins("http://localhost:4200")
                        .AllowAnyMethod()
                        .AllowAnyHeader()
                        .AllowCredentials()); // Importante para SignalR después
});

var app = builder.Build();

// Middleware Pipeline
app.UseExceptionHandler(); // Usa el GlobalExceptionHandler

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseCors("AllowAngular");
app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();

app.Run();
```

### Configuración Final: Connection String

En `apps/api/appsettings.json`:

JSON

```
"ConnectionStrings": {
  "DefaultConnection": "Server=localhost,1433;Database=TacticalC2Db;User Id=sa;Password=Tactical_P@ssw0rd!;TrustServerCertificate=True;"
}
```

### Ejecutar Migraciones

Aplica los cambios a la BBDD Dockerizada:

Bash

```
dotnet ef migrations add InitialCreate --startup-project apps/api
dotnet ef database update --startup-project apps/api
```

---

### ✅ Checkpoint Módulo 2

Hemos construido un Backend que ya supera a la mayoría de pruebas técnicas estándar:

1. **Arquitectura Limpia:** Separación clara de DTOs, Entidades y Lógica.
    
2. **Seguridad:** No exponemos la estructura de la BBDD.
    
3. **Manejo de Errores:** Centralizado con `IExceptionHandler`.
    
4. **Escalabilidad:** Uso de `Records` y `Async/Await`.
    

**Siguiente paso:** El Frontend. Vamos a abandonar las viejas costumbres de Angular y entrar de lleno en **Angular 18 Standalone & Signals**.

¿Listo para el frontend moderno? **Di "Empieza el Módulo 3"**.