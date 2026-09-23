
## Prompt de Desarrollo para Audioguía Inteligente (Flutter)

**Título del Proyecto:** Flutter SmartGuide AI

**Plataforma:** Mobile (iOS y Android)

**Objetivo:** Desarrollar una aplicación de audioguía inteligente en Flutter, utilizando Clean Architecture y Supabase, que permita la exploración de monumentos y la interacción conversacional con una IA.

---

### 1. Requisitos de Arquitectura y Tecnología (NON-NEGOTIABLES)

1. **Framework:** Flutter (última versión estable).
    
2. **Arquitectura:** Implementación **obligatoria** del patrón _Clean Architecture_ con las siguientes capas:
    
    - **Data Layer:** Manejo de fuentes de datos (Supabase, APIs externas para IA). Contiene _Data Sources_ y _Implementaciones de Repositorios_.
        
    - **Domain Layer:** Contiene la lógica de negocio pura (Entities, Repositories, y Use Cases). **Debe ser independiente de Flutter y Supabase.**
        
    - **Presentation Layer:** Contiene la UI (Widgets, Screens) y el manejo de estado.
        
3. **Gestión de Estado:** Utilizar **`flutter_bloc` / `bloc` / `cubit`** para manejar el estado en la capa de Presentación, asegurando la separación de preocupaciones.
    
4. **Inyección de Dependencias (DI):** Implementar un sistema de DI (ej. **`get_it`**) para registrar Repositorios y Use Cases, garantizando que las capas dependan de abstracciones.
    
5. **Backend:** **Supabase** será el único proveedor de servicios de base de datos, autenticación y almacenamiento (para avatares/imágenes de monumentos).
    

### 2. Configuración de Backend (Supabase)

Implementar y utilizar las siguientes estructuras de datos iniciales a través del paquete `supabase_flutter`.

#### 2.1 Autenticación

- Implementar `signInWithPassword`, `signUp`, `signInWithOAuth` (Google) y `resetPasswordForEmail`.
    

#### 2.2 Tablas (Entities Mapeadas)

1. **`profiles`**:
    
    - `id` (UUID, FK a `auth.users`)
        
    - `username` (string)
        
    - `avatar_url` (string, para fotos de perfil)
        
    - `theme_preference` (string, 'light' o 'dark')
        
2. **`monuments`**:
    
    - `id` (UUID)
        
    - `city_id` (FK a `cities.id`)
        
    - `name` (string)
        
    - `location` (string/text)
        
    - `image_url` (string, portada del monumento)
        
    - `is_recommended` (boolean)
        
    - `category` (string, para filtros rápidos: 'Arte', 'Historia', etc.)
        
3. **`cities`**:
    
    - `id` (UUID)
        
    - `name` (string)
        

### 3. Integración de la Inteligencia Artificial

La IA conversacional es el núcleo de la aplicación. En la capa de Datos, se deben crear _Data Sources_ y _Repositories_ para abstraer las siguientes funciones.

1. **`AIDataSource` (Abstracción de API Externa):**
    
    - `generateNarrative(monumentId)`: Devuelve el texto inicial de la guía.
        
    - `getLLMResponse(chatHistory, newPrompt)`: Procesa la pregunta del usuario y devuelve la respuesta contextual de la IA.
        
2. **`TTSService` (Text-to-Speech):**
    
    - `synthesizeSpeech(text)`: Convierte el texto de la IA en audio (usar un plugin que simule la API o un mock si no se especifica una API concreta).
        
3. **`STTService` (Speech-to-Text):**
    
    - `recognizeSpeech(audioFile)`: Transcribe el audio grabado por el usuario a texto (usar un plugin que simule la API o un mock).
        

---

### 4. Flujo de Navegación y Diseño

- **Tema:** Material 3, totalmente sensible, con soporte para Light/Dark Mode (gestionado por US-CONF-1).
    
- **Barra de Navegación:** Incluir un `BottomNavigationBar` o `Drawer` con acceso a **Tienda** y **Configuración**.
    

---

### 5. Requisitos Detallados por Módulo (Mapeo de US)

#### A. Módulo de Autenticación (US-AUTH-1 a 4)

- **Pantalla de Login:** Debe ser visualmente atractiva , incluir campos validados (email, password), toggle para `Mostrar Contraseña`, y botones para **Login**, **Registro** y **Login con Google**.
    
- **Recuperación:** Implementar el flujo de Supabase para `resetPasswordForEmail`.
    

#### B. Módulo de Tienda y Exploración (US-SHOP-1 a 6)

- **Pantalla `ShopScreen`:**
    
    1. **Selector de Ciudad (US-SHOP-2):** Un _Dropdown_ o selector que filtra _todos_ los datos de la pantalla.
        
    2. **Buscador (US-SHOP-3):** Barra de texto que realiza búsquedas en tiempo real en los listados filtrados por ciudad.
        
    3. **Filtros Rápidos (US-SHOP-4):** Fila horizontal de `Chip` widgets para filtrar por `monuments.category`.
        
    4. **Carrusel Recomendados (US-SHOP-5):** `ListView.builder` horizontal, mostrando monumentos donde `is_recommended = true`.
        
    5. **Rejilla de Museos (US-SHOP-6):** `GridView.count` con **dos columnas** para la visualización principal. Cada tarjeta debe mostrar **Imagen, Nombre y Localización**.
        

#### C. Módulo de Audioguía Inteligente (US-IA-1 a 4)

- **Pantalla `MonumentChatScreen`:**
    
    - **UI:** Interfaz de chat moderna.
        
    - **Flujo de Inicio (US-IA-2):** Al cargar, el **`Bloc`** debe llamar al `Use Case` para obtener la narración inicial de la IA y enviarla al servicio TTS para la reproducción automática. Mostrar un indicador de reproducción de audio.
        
    - **Entrada de Texto (US-IA-3):** Campo de texto estándar. Al enviar, el mensaje del usuario se añade a la historia del chat y se envía al LLM.
        
    - **Entrada de Voz (US-IA-4):** Botón flotante o icono de micrófono que, al ser presionado, simula la grabación de audio. El audio grabado se convierte a texto (STT) antes de enviarse al LLM.
        
    - **Contexto:** El `Bloc` de chat debe manejar el historial de mensajes para que la IA mantenga el contexto sobre el monumento específico.
        

#### D. Módulo de Configuración (US-CONF-1 a 3, US-AUTH-5)

- **Pantalla `SettingsScreen`:**
    
    - **Sección Perfil (US-CONF-2):** Mostrar datos del usuario (`profiles` tabla) con la opción de editar el nombre y el avatar (usando Supabase Storage).
        
    - **Opción Tema (US-CONF-1):** `Switch` o `Toggle` para cambiar el tema de la aplicación (Light/Dark Mode). El cambio debe persistir en la tabla `profiles`.
        
    - **Botón Cerrar Sesión (US-CONF-3):** Botón destacado que llama al método de _sign out_ de Supabase y navega de vuelta al Login.
        

### 6. Calidad y Mantenibilidad

- **Manejo de Errores:** Todos los Repositorios y Use Cases deben manejar excepciones de Supabase y transformarlas en errores de dominio amigables. Mostrar estos errores al usuario mediante `Snackbars` o diálogos.
    
- **Estados de Carga:** Implementar y mostrar estados de carga (`CircularProgressIndicator`) para todas las operaciones asíncronas (Login, carga de monumentos, respuestas de IA).
    
- **Testing:** Generar _Unit Tests_ para al menos el 50% de los **Use Cases** y **Repositories** para garantizar la lógica de negocio.
    
- **Documentación:** Comentar la estructura de carpetas y las clases principales, especialmente las que definen las _Entities_ y _Use Cases_ de la capa de Dominio.