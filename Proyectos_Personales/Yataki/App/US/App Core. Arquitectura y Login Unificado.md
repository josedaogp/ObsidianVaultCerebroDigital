# 📱 App Core: Arquitectura, Login y Autenticación

Este documento define la base de la **única aplicación** que servirá para todos los roles, así como los flujos de autenticación críticos.

## Aclaración Importante: Flujo de Registro

A diferencia de una app B2C, esta plataforma **no tendrá un botón de "Registrarse"** en la pantalla de login.

El registro de nuevos usuarios (Restaurantes o Repartidores) es un **proceso iniciado por el Admin** desde el Panel de Administración Web. El flujo es:

1. El Admin negocia con un nuevo restaurante o contrata a un repartidor.
    
2. El Admin va a su Panel Web y usa la User Story **[A-06] Gestión de Restaurantes (o Repartidores)**.
    
3. En ese panel, el Admin crea la cuenta de usuario, asigna el rol (`restaurant` o `driver`), y establece un email y una contraseña temporal.
    
4. El Admin comunica estas credenciales al usuario (Restaurante/Repartidor).
    
5. El usuario entonces usa esas credenciales para iniciar sesión por primera vez en la app móvil ([CORE-01]).
    

Por lo tanto, no hay una US de "Registro" en _esta_ aplicación móvil.

## [CORE-01] Login por Roles

> Como Usuario (Restaurante, Repartidor o Admin),
> 
> Quiero usar una única pantalla de Login con mi email y contraseña,
> 
> Para que la app me reconozca y me redirija a mi módulo correspondiente.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-C01.1] Pantalla Única de Login**
    
    - **Dado** que cualquier usuario abre la app por primera vez.
        
    - **Entonces** ve una única pantalla de Login con: `Email`, `Contraseña`, botón "Entrar", y un link/botón de "He olvidado mi contraseña" (que lleva a [CORE-02]).
        
- **[AC-C01.2: Redirección por Rol (La Clave)]**
    
    - **Dado** que el usuario pulsa "Entrar" con credenciales válidas.
        
    - **Cuando** el servidor devuelve el token de autenticación (a través del login de Supabase).
        
    - **Entonces** debemos recuperar el "Rol" del usuario (ej. `{"rol": "restaurante"}`).
        
    - **Y** la app debe leer este rol y navegar a la pantalla principal del módulo correspondiente:
        
        - Si Rol="restaurante", navegar a **[R-02] Dashboard Kanban**.
            
        - Si Rol="repartidor", navegar a **[D-01] Hoja de Ruta**.
            
        - Si Rol="admin", navegar a **[A-01] Dashboard de Operaciones**.
            
- **[AC-C01.3: Persistencia de Sesión y Rol]**
    
    - **Dado** que un usuario (ej. un Restaurante) ya ha iniciado sesión y cierra la app.
        
    - **Cuando** vuelve a abrir la app.
        
    - **Entonces** la app debe leer el token Y el rol guardados, y redirigirle directamente a su **[R-02] Dashboard Kanban** sin pasar por el Login.
        
- **[AC-C01.4: Fallo de Login]**
    
    - **Dado** que el usuario introduce credenciales incorrectas.
        
    - **Entonces** la app debe mostrar un error genérico (ej. "Email o contraseña incorrectos") sin revelar qué campo falló.
        

#### 🎨 Notas de Diseño y Técnicas

- **Tecnología:** Ideal para un _framework_ como React Native o Flutter, donde se puede definir un "Navegador" principal que, tras el login, carga el "Navegador del Restaurante", "Navegador del Repartidor" o "Navegador del Admin" según el rol.
    
- **Backend:** El _endpoint_ `POST /auth/login` ahora debe devolver `{"token": "...", "user_role": "repartidor"}`. (En Supabase, esto se gestiona con el JWT y los `app_metadata`).
    

## [CORE-02] Recuperar Contraseña (Olvido) (¡NUEVO!)

> Como Usuario (Restaurante, Repartidor o Admin),
> 
> Quiero poder resetear mi contraseña desde la pantalla de Login,
> 
> Para poder recuperar el acceso a mi cuenta si la olvido.

#### 📐 Criterios de Aceptación (Acceptance Criteria)

- **[AC-C02.1: Acceso al Flujo]**
    
    - **Dado** que el usuario está en la pantalla de Login [CORE-01].
        
    - **Cuando** pulsa el link/botón "He olvidado mi contraseña".
        
    - **Entonces** es llevado a una nueva pantalla: "Recuperar Contraseña".
        
- **[AC-C02.2: Solicitud de Reseteo]**
    
    - **Dado** que el usuario está en la pantalla "Recuperar Contraseña".
        
    - **Cuando** introduce su `email` y pulsa "Enviar Instrucciones".
        
    - **Entonces** la app debe mostrar un mensaje de confirmación genérico (ej. "Si tu email está registrado, recibirás un correo con instrucciones.").
        
    - **(Importante):** La app debe mostrar este mensaje _incluso si el email no existe en la BBDD_ (para evitar que se pueda usar para adivinar emails).
        
- **[AC-C02.3: Envío de Email (Backend)]**
    
    - **Dado** que el `email` introducido _sí_ existe en la BBDD.
        
    - **Cuando** el _backend_ (Supabase Auth) recibe la solicitud.
        
    - **Entonces** el _backend_ debe generar un token de reseteo único y enviar un email al usuario con un "magic link" (enlace de reseteo).
        
- **[AC-C02.4: Flujo de Reseteo (Web/App)]**
    
    - **Dado** que el usuario recibe el email y pulsa el "magic link".
        
    - **Cuando** abre el link.
        
    - **Entonces** es llevado a una página/pantalla (idealmente una página web o un _deep link_ de vuelta a la app) donde puede introducir su `Nueva Contraseña` y `Confirmar Nueva Contraseña`.
        
- **[AC-C02.5: Confirmación de Cambio]**
    
    - **Dado** que el usuario introduce una nueva contraseña válida y la confirma.
        
    - **Cuando** pulsa "Guardar Nueva Contraseña".
        
    - **Entonces** el `password_hash` en la BBDD se actualiza.
        
    - **Y** el usuario es redirigido a la pantalla de Login [CORE-01] con un mensaje de "Contraseña actualizada. Ya puedes iniciar sesión."
        

#### ➡️ Happy Path (Flujo Ideal)

1. Usuario pulsa "He olvidado mi contraseña".
    
2. Introduce su email (`luigi@pizzeria.com`) y pulsa "Enviar".
    
3. Ve el mensaje "Instrucciones enviadas".
    
4. Abre su email, pulsa el link de reseteo.
    
5. Introduce "NuevaClave123!" (dos veces) y pulsa "Guardar".
    
6. Es redirigido al Login y puede entrar con su nueva clave.
    

#### ⚠️ Edge Cases y Errores (Casos Borde)

- **Link Caducado:** Si el usuario pulsa un link de reseteo que ya ha caducado (ej. > 1 hora), la página de reseteo debe mostrar un error "Este enlace ha caducado. Por favor, solicita uno nuevo."
    
- **Email Inexistente:** El usuario introduce un email falso. La app dice "Instrucciones enviadas". El _backend_ no hace nada (falla silenciosamente). El usuario nunca recibe un email.
    

#### 🎨 Notas de Diseño y Técnicas (Para el Desarrollador)

- **Backend (Supabase):** Esta es una de las grandes ventajas de Supabase. Su módulo `auth` gestiona este flujo (envío de email de reseteo y actualización de contraseña) de forma nativa. Solo necesitamos llamar a la función `supabase.auth.resetPasswordForEmail()` y configurar la plantilla de email en el dashboard de Supabase.