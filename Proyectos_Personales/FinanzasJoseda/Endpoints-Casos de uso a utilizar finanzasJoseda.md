1. **Gestión de Categorías de Gasto:**
    - **Listar Categorías de Gasto:**
        - GET /categorias/
    - **Crear Nueva Categoría de Gasto:**
        - POST /categorias/
    - **Actualizar Categoría de Gasto Existente:**
        - PUT /categorias/{id_categoria}
    - **Eliminar Categoría de Gasto:**
        - DELETE /categorias/{id_categoria}
2. **Gestión de Gastos:**
    - **Listar Gastos:**
        - GET /gastos/
    - **Crear Nuevo Gasto:**
        - POST /gastos/
    - **Actualizar Gasto Existente:**
        - PUT /gastos/{id_gasto}
    - **Eliminar Gasto:**
        - DELETE /gastos/{id_gasto}
3. **Gestión de Monederos:**
    - **Listar Monederos:**
        - GET /monederos/
    - **Crear Nuevo Monedero:**
        - POST /monederos/
    - **Actualizar Monedero Existente:**
        - PUT /monederos/{id_monedero}
    - **Eliminar Monedero:**
        - DELETE /monederos/{id_monedero}
4. **Gestión de Ingresos:**
    - **Listar Ingresos:**
        - GET /ingresos/
    - **Crear Nuevo Ingreso:**
        - POST /ingresos/
    - **Actualizar Ingreso Existente:**
        - PUT /ingresos/{id_ingreso}
    - **Eliminar Ingreso:**
        - DELETE /ingresos/{id_ingreso}
5. **Operaciones Adicionales:**
    - **Obtener Resumen Financiero:**
        - GET /resumen/
    - **Obtener Detalles de un Gasto Específico:**
        - GET /gastos/{id_gasto}/detalles/
    - **Obtener Detalles de un Monedero Específico:**
        - GET /monederos/{id_monedero}/detalles/
    - **Obtener Detalles de un Ingreso Específico:**
        - GET /ingresos/{id_ingreso}/detalles/