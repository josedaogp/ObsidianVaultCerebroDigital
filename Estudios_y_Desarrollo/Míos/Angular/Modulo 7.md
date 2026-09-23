Recibido. Pasamos a la fase de **Aseguramiento**.

En un entorno OTAN/Defensa, la seguridad no es una "feature" más, es el requisito cero. Implementaremos una arquitectura **Zero Trust** a nivel de aplicación.

No usaremos cookies de sesión. Usaremos **Stateless JWT (JSON Web Tokens)**. El servidor no recordará quiénes somos; nosotros le recordaremos quiénes somos en cada petición mediante una "credencial" firmada criptográficamente.

Además, implementaremos **RBAC (Role-Based Access Control)**.

- **COMMANDER:** Puede crear/editar misiones.
    
- **VIEWER:** Solo puede ver el mapa y estado.
    

---

# 🔐 Módulo 7: Seguridad y Control de Acceso (RBAC)

## 7.1. Backend: Configuración de JWT (.NET 8)

Primero, blindemos la API.

**Paso 1: Configuración en `appsettings.json`** Añade una clave secreta (en prod usaríamos Azure KeyVault, aquí simulamos).

JSON

```
"JwtSettings": {
  "Key": "Tactical_Super_Secret_Key_For_Demo_Only_123!",
  "Issuer": "TacticalHQ",
  "Audience": "TacticalFieldUnits",
  "DurationInMinutes": 60
}
```

**Paso 2: DTOs de Autenticación** En `apps/api/DTOs/AuthDtos.cs`:

C#

```
namespace Tactical.API.DTOs;

public record LoginRequest(string Username, string Password);
public record LoginResponse(string Token, string Role, int ExpiresIn);
```

**Paso 3: Servicio de Identidad (AuthService)** Para este curso, simularemos la validación de usuario (Hardcoded) para no complicar la BBDD con tablas de `IdentityUser` ahora mismo, pero la estructura será real.

Crea `apps/api/Services/AuthService.cs`:

C#

```
using System.IdentityModel.Tokens.Jwt;
using System.Security.Claims;
using System.Text;
using Microsoft.IdentityModel.Tokens;
using Tactical.API.DTOs;

namespace Tactical.API.Services;

public class AuthService
{
    private readonly IConfiguration _config;

    public AuthService(IConfiguration config)
    {
        _config = config;
    }

    public LoginResponse? Login(LoginRequest request)
    {
        // SIMULACIÓN DE BBDD DE USUARIOS
        // En producción: await _userManager.FindByNameAsync(...)
        string role = "VIEWER";
        
        if (request.Username == "admin" && request.Password == "admin") role = "COMMANDER";
        else if (request.Username == "user" && request.Password == "user") role = "VIEWER";
        else return null; // Credenciales inválidas

        return GenerateToken(request.Username, role);
    }

    private LoginResponse GenerateToken(string username, string role)
    {
        var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_config["JwtSettings:Key"]!));
        var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

        var claims = new[]
        {
            new Claim(JwtRegisteredClaimNames.Sub, username),
            new Claim(ClaimTypes.Role, role), // CRÍTICO: Aquí va el rol para el RBAC
            new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString())
        };

        var duration = int.Parse(_config["JwtSettings:DurationInMinutes"]!);
        var token = new JwtSecurityToken(
            issuer: _config["JwtSettings:Issuer"],
            audience: _config["JwtSettings:Audience"],
            claims: claims,
            expires: DateTime.UtcNow.AddMinutes(duration),
            signingCredentials: creds
        );

        return new LoginResponse(
            new JwtSecurityTokenHandler().WriteToken(token),
            role,
            duration * 60
        );
    }
}
```

**Paso 4: AuthController** Crea `apps/api/Controllers/AuthController.cs`:

C#

```
using Microsoft.AspNetCore.Mvc;
using Tactical.API.DTOs;
using Tactical.API.Services;

namespace Tactical.API.Controllers;

[ApiController]
[Route("api/v1/[controller]")]
public class AuthController : ControllerBase
{
    private readonly AuthService _authService;

    public AuthController(AuthService authService) => _authService = authService;

    [HttpPost("login")]
    public IActionResult Login([FromBody] LoginRequest request)
    {
        var result = _authService.Login(request);
        if (result == null) return Unauthorized("Credenciales inválidas.");
        return Ok(result);
    }
}
```

**Paso 5: Middleware en `Program.cs`** Esto es vital. Si no lo pones, el atributo `[Authorize]` no funcionará.

C#

```
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.IdentityModel.Tokens;
using System.Text;
using Tactical.API.Services;

// ... (Después de crear el builder)

// Registrar servicio
builder.Services.AddScoped<AuthService>();

// Configurar JWT
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidateAudience = true,
            ValidateLifetime = true,
            ValidateIssuerSigningKey = true,
            ValidIssuer = builder.Configuration["JwtSettings:Issuer"],
            ValidAudience = builder.Configuration["JwtSettings:Audience"],
            IssuerSigningKey = new SymmetricSecurityKey(
                Encoding.UTF8.GetBytes(builder.Configuration["JwtSettings:Key"]!))
        };
    });

// ...

var app = builder.Build();

// ...
app.UseHttpsRedirection();

app.UseAuthentication(); // <--- ORDEN IMPORTANTE: Antes de Authorization
app.UseAuthorization();

app.MapControllers();
```

**Paso 6: Proteger Endpoints** Ve a `MissionsController.cs` y añade seguridad.

C#

```
using Microsoft.AspNetCore.Authorization;

[ApiController]
[Route("api/v1/[controller]")]
[Authorize] // <--- Bloquea todo el controlador por defecto
public class MissionsController : ControllerBase
{
    // ... constructor ...

    [HttpPost]
    [Authorize(Roles = "COMMANDER")] // <--- Solo comandantes pueden crear
    public async Task<IActionResult> Create(...) { ... }

    [HttpGet]
    // [Authorize] se hereda, cualquier usuario autenticado puede leer
    public async Task<IActionResult> GetAll() { ... }
}
```

---

## 7.2. Frontend: Auth Store & Interceptor

Ahora, Angular debe aprender a guardar el token y enviarlo.

**Paso 1: AuthStore (Gestión de Sesión)** En `libs/data-access/tactical-api/src/lib/stores/auth.store.ts`.

TypeScript

```
import { Injectable, signal, computed, inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Router } from '@angular/router';
import { tap } from 'rxjs';

interface AuthState {
  token: string | null;
  role: 'COMMANDER' | 'VIEWER' | null;
  username: string | null;
}

@Injectable({ providedIn: 'root' })
export class AuthStore {
  private http = inject(HttpClient);
  private router = inject(Router);

  // Estado inicial: Intentar leer de localStorage para persistencia al refrescar
  private initialState: AuthState = {
    token: localStorage.getItem('tactical_token'),
    role: localStorage.getItem('tactical_role') as any,
    username: localStorage.getItem('tactical_user')
  };

  private state = signal<AuthState>(this.initialState);

  // Selectores
  readonly isAuthenticated = computed(() => !!this.state().token);
  readonly userRole = computed(() => this.state().role);
  readonly username = computed(() => this.state().username);

  login(credentials: { username: string, password: string }) {
    return this.http.post<any>('auth/login', credentials).pipe(
      tap(res => {
        // Guardar en estado
        this.state.set({
          token: res.token,
          role: res.role,
          username: credentials.username
        });
        
        // Persistencia simple (En prod usaríamos cookies HttpOnly o almacenamiento más seguro)
        localStorage.setItem('tactical_token', res.token);
        localStorage.setItem('tactical_role', res.role);
        localStorage.setItem('tactical_user', credentials.username);

        this.router.navigate(['/dashboard']);
      })
    );
  }

  logout() {
    this.state.set({ token: null, role: null, username: null });
    localStorage.clear();
    this.router.navigate(['/login']);
  }
}
```

**Paso 2: Functional Interceptor (Auth Token)** Este interceptor "secuestra" cada petición y le pega el token. Crea `libs/data-access/tactical-api/src/lib/interceptors/auth.interceptor.ts`.

TypeScript

```
import { HttpInterceptorFn } from '@angular/common/http';
import { inject } from '@angular/core';
import { AuthStore } from '../stores/auth.store';

export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const authStore = inject(AuthStore);
  const token = authStore.isAuthenticated() ? localStorage.getItem('tactical_token') : null;

  if (token) {
    const cloned = req.clone({
      setHeaders: {
        Authorization: `Bearer ${token}`
      }
    });
    return next(cloned);
  }

  return next(req);
};
```

**Paso 3: Registrar Interceptor** Vuelve a `apps/dashboard/src/app/app.config.ts` y añade el interceptor a la cadena.

TypeScript

```
import { authInterceptor } from '@tactical-c2/data-access-api'; // Importar

export const appConfig: ApplicationConfig = {
  providers: [
    // ...
    provideHttpClient(
      withFetch(),
      withInterceptors([baseUrlInterceptor, authInterceptor]) // <--- Añadir aquí
    )
  ]
};
```

---

## 7.3. Frontend: Guards Modernos (Functional Route Guards)

En Angular moderno, no usamos Clases para Guards (`CanActivate`), usamos funciones. Son más simples y potentes.

**Archivo: `libs/data-access/tactical-api/src/lib/guards/auth.guard.ts`**

TypeScript

```
import { inject } from '@angular/core';
import { CanActivateFn, Router } from '@angular/router';
import { AuthStore } from '../stores/auth.store';

// Guard 1: Verifica si está logueado
export const isAuthenticatedGuard: CanActivateFn = (route, state) => {
  const authStore = inject(AuthStore);
  const router = inject(Router);

  if (authStore.isAuthenticated()) {
    return true;
  }

  return router.createUrlTree(['/login']);
};

// Guard 2: Verifica si tiene Rol específico
export const hasRoleGuard = (requiredRole: string): CanActivateFn => {
  return (route, state) => {
    const authStore = inject(AuthStore);
    
    if (authStore.userRole() === requiredRole) {
      return true;
    }

    alert('ACCESO DENEGADO: Rango insuficiente para esta operación.');
    return false;
  };
};
```

---

## 7.4. Integración en Rutas

Finalmente, cerramos las puertas.

**Archivo: `apps/dashboard/src/app/app.routes.ts`**

TypeScript

```
import { Route } from '@angular/router';
import { isAuthenticatedGuard } from '@tactical-c2/data-access-api';
import { LoginComponent } from './login/login.component'; // (Crear este componente simple)

export class AppRoutes {
  static routes: Route[] = [
    { path: 'login', component: LoginComponent },
    {
      path: '',
      // Protegemos TODAS las rutas hijas
      canActivate: [isAuthenticatedGuard], 
      children: [
        { path: '', redirectTo: 'missions', pathMatch: 'full' },
        { 
           path: 'missions', 
           loadComponent: () => import('@tactical-c2/mission-control').then(m => m.MissionListComponent) 
        },
        // Ejemplo de protección por Rol (Solo si creáramos ruta de edición)
        // { 
        //   path: 'missions/create', 
        //   canActivate: [hasRoleGuard('COMMANDER')],
        //   loadComponent: ...
        // }
      ]
    }
  ];
}
```

---

### ✅ Checkpoint Módulo 7

Hemos convertido un prototipo en un sistema seguro:

1. **JWT Backend:** Emisión y validación de tokens firmados.
    
2. **RBAC:** Diferenciación entre `COMMANDER` (admin/admin) y `VIEWER` (user/user).
    
3. **Interceptor:** Inyección automática de cabeceras `Authorization`.
    
4. **Guards:** Protección de rutas a nivel de cliente.
    

Si intentas entrar a la app ahora, te debería redirigir a `/login` (o fallar si no creaste el componente visual de login, pero la lógica de bloqueo ya funciona).

**Siguiente paso:** Para entregar esto como un proyecto profesional, necesitamos garantizar que no se rompa. Vamos a escribir **Tests Automáticos**. Un Tech Lead no acepta PRs sin tests.

¿Listo para asegurar la calidad? **Di "Empieza el Módulo 8"**.