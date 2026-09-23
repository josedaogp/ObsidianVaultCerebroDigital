**Objetivo General**:  
El objetivo de la aplicación es permitir a los usuarios gestionar sus presupuestos mensuales y monederos financieros de forma centralizada. El proceso clave de la app es que el usuario, al final de cada mes, introduce de una vez todos los gastos de las categorías asociadas a su plantilla de presupuesto mensual. La aplicación calcula automáticamente los sobrantes o faltantes de cada categoría y los asigna a los monederos correspondientes, todo en un solo flujo.

### **Características Principales**:

1. **Categorías y Presupuestos**:
    
    - El usuario puede crear, modificar o eliminar **categorías de gasto**. Estas categorías pueden ser **acumulativas** (el sobrante se guarda) o **no acumulativas** (el sobrante se pierde al final del mes).
    - Cada mes, el usuario tiene una **plantilla de presupuesto** donde puede ver todas las categorías, ajustar los presupuestos y al final del mes introducir los gastos de una vez para todas las categorías.
2. **Registro de Gastos Mensuales**:
    
    - Al final de cada mes, el usuario registra los gastos de todas las categorías en un solo paso.
    - La aplicación calcula automáticamente el **sobrante** o **faltante** de cada categoría.
3. **Monederos**:
    
    - El usuario puede gestionar diferentes **monederos** (crear, editar, eliminar), que representan saldos de diferentes cuentas o fondos.
    - Los **sobrantes o faltantes** de las categorías se asignan automáticamente a los monederos preconfigurados al finalizar el proceso de introducción de gastos.
    - La app permite ver el **historial de los monederos** mes a mes para analizar la progresión de saldos.
4. **Asignación Automática de Sobrantes/Faltantes**:
    
    - Una vez que se introducen los gastos mensuales, los sobrantes o faltantes se distribuyen automáticamente entre los monederos según las preferencias del usuario.
5. **Histórico y Reportes**:
    
    - El usuario puede visualizar reportes mensuales del estado de sus categorías y monederos.
    - También puede ver el **histórico de saldos** para analizar la progresión de sus finanzas personales a lo largo del tiempo.

### **Proceso Mensual**:

1. El usuario abre la app al final de cada mes.
2. Introduce de una vez todos los gastos en cada categoría.
3. La app calcula automáticamente los sobrantes/faltantes de cada categoría.
4. Los sobrantes/faltantes se asignan a los monederos configurados.
5. El usuario puede ver un resumen mensual y consultar el historial de monederos.

---

### **Estructura General de Pantallas**:

1. **Pantalla Principal (Dashboard)**:  
    Muestra un resumen del presupuesto mensual, estado de monederos y sobrantes/faltantes.
    
2. **Pantalla de Plantilla de Presupuesto Mensual**:  
    Permite ver y editar las categorías y presupuestos del mes.
    
3. **Pantalla de Registro de Gastos Mensuales**:  
    El usuario introduce los gastos de todas las categorías del mes.
    
4. **Pantalla de Sobrante/Faltante Automático**:  
    Muestra el cálculo de sobrantes/faltantes después de registrar los gastos y permite confirmar la asignación automática a los monederos.
    
5. **Pantalla de Monederos**:  
    Muestra el saldo actual de cada monedero y permite gestionar los monederos.
    
6. **Pantalla de Histórico de Monederos**:  
    Permite ver la evolución del saldo de los monederos mes a mes.
    

---

### **Requisitos Técnicos**:

- Base de datos: PostgreSQL/MySQL.
- Backend: FastAPI/Node.js.
- Frontend: Flutter.
- Autenticación: JWT.
- Seguridad: Encriptación y HTTPS para el backend.

---

### **Caso de Uso Ejemplo: Introducción de Gastos Mensuales**

1. El usuario inicia sesión en la aplicación.
2. Desde el Dashboard accede a la pantalla de **Registro de Gastos Mensuales**.
3. Introduce todos los gastos de cada categoría del mes.
4. La app calcula el sobrante/faltante de cada categoría.
5. La app asigna automáticamente estos sobrantes o faltantes a los monederos configurados.
6. El usuario revisa los datos y vuelve al Dashboard con los cambios aplicados.

---

Con este resumen, me puedes preguntar sobre cualquier funcionalidad o cambio que quieras hacer en la aplicación y te responderé en base a este contexto.