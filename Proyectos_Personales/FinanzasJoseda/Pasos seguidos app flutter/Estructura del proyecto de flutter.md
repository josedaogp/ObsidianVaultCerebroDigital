Aquí pongo un ejemplo de estructura de directorios de un proyecto flutter. Habrá que añadir o quitar según se quiera.

## Ejemplo simple (inicio app)

lib
└───app
    ├───data
    │   └───services
    │       ├───devices
    │       ├───local
    │       └───remote
    ├───domain
    │   ├───either
    │   ├───models
    │   └───repositories
    └───presentation
        ├───global
        │   └───widgets
        ├───pages
        │   └───resumen_financiero
        │       ├───controller
        │       │   └───state
        │       └───widgets
        └───routes
## Ejemplo complejo (app final)
lib
└───app
    ├───data
    │   ├───http
    │   ├───repositories_implementation
    │   └───services
    │       ├───local
    │       ├───remote
    │       └───utils
    ├───domain
    │   ├───either
    │   ├───failures
    │   │   ├───http_request
    │   │   └───sign_in
    │   ├───models
    │   │   ├───genre
    │   │   ├───media
    │   │   ├───movie
    │   │   ├───peformer
    │   │   └───user
    │   └───repositories
    └───presentation
        ├───global
        │   ├───controllers
        │   │   └───favorites
        │   │       └───state
        │   ├───dialogs
        │   ├───extensions
        │   ├───utils
        │   └───widgets
        ├───modules
        │   ├───favorites
        │   │   └───views
        │   │       └───widgets
        │   ├───home
        │   │   ├───controller
        │   │   │   └───state
        │   │   └───views
        │   │       └───widgets
        │   │           ├───movies_and_series
        │   │           └───performers
        │   ├───movie
        │   │   ├───controller
        │   │   │   └───state
        │   │   └───views
        │   │       └───widgets
        │   ├───offline
        │   │   └───views
        │   ├───profile
        │   │   └───views
        │   ├───sign_in
        │   │   ├───controller
        │   │   │   └───state
        │   │   └───views
        │   │       └───widgets
        │   └───splash
        │       └───views
        ├───routes
        └───utils