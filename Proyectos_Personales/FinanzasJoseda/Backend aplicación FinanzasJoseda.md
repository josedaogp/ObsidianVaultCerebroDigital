## Estructura de la aplicación
1. **main.py:** Este sería el punto de entrada principal de tu aplicación FastAPI. Aquí configurarás la aplicación FastAPI y montarás tus rutas API.
    
2. **routers/:** En esta carpeta, colocarás los archivos de enrutadores FastAPI que definirán las rutas y controladores para diferentes partes de tu API. Por ejemplo:
    - **categorias_router.py:** Definiría las rutas relacionadas con las categorías de gastos.
    - **gastos_router.py:** Definiría las rutas relacionadas con los gastos.
    - **monederos_router.py:** Definiría las rutas relacionadas con los monederos.
    - **ingresos_router.py:** Definiría las rutas relacionadas con los ingresos.
3. **models/:** En esta carpeta, colocarías los modelos de datos Pydantic que representan las estructuras de datos de tu aplicación. Por ejemplo:
    - **categoria_model.py:** Definiría el modelo de datos para una categoría de gasto.
    - **gasto_model.py:** Definiría el modelo de datos para un gasto.
    - **monedero_model.py:** Definiría el modelo de datos para un monedero.
    - **ingreso_model.py:** Definiría el modelo de datos para un ingreso.
4. **services/:** Aquí colocarías los archivos que contienen la lógica de negocio de tu aplicación. Por ejemplo:
    - **categoria_service.py:** Contendría funciones para interactuar con las categorías de gastos en la base de datos.
    - **gasto_service.py:** Contendría funciones para interactuar con los gastos en la base de datos.
    - **monedero_service.py:** Contendría funciones para interactuar con los monederos en la base de datos.
    - **ingreso_service.py:** Contendría funciones para interactuar con los ingresos en la base de datos.
5. **database/:** En esta carpeta, colocarías los archivos relacionados con la configuración y la interacción con la base de datos. Por ejemplo:
    - **database.py:** Contendría la configuración de la conexión a la base de datos.
    - **schemas.py:** Contendría la definición de los esquemas Pydantic utilizados en la validación de datos.
6. **utils/:** Aquí colocarías archivos de utilidades o funciones auxiliares que puedan ser utilizadas en múltiples partes de tu aplicación.
    
7. **config/:** Si necesitas configuraciones específicas para tu aplicación, puedes colocarlas aquí.
    
8. **tests/:** Esta carpeta contendría los archivos de prueba para tu aplicación.