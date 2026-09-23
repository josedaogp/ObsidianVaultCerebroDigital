backend/
├── app/
│   ├── __init__.py
│   ├── main.py           # Punto de entrada de FastAPI
│   ├── models.py         # Modelos de la base de datos
│   ├── schemas.py        # Esquemas Pydantic para validación
│   ├── crud.py           # Lógica de base de datos y operaciones
│   ├── services/         # Servicios como IA, Amazon, etc.
│   ├── auth/             # Lógica de autenticación
│   └── api/              # Rutas del API
│       ├── __init__.py
│       ├── users.py
│       ├── conversations.py
│       ├── products.py
│       ├── recommendations.py
│       ├── notifications.py
│       └── admin.py
├── config.py             # Configuración general (bases de datos, API keys)
├── .env                  # Variables de entorno
└── requirements.txt      # Dependencias del proyecto
