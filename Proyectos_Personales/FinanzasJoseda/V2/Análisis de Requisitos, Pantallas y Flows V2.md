### 1. **Análisis de Requisitos Funcionales**

**Requisitos de Categorías y Presupuestos**

1. **Registro de Categorías**: El usuario debe poder crear, editar o eliminar categorías. Cada categoría puede ser acumulativa (se guarda el sobrante) o no acumulativa (no se guarda el sobrante).
2. **Plantilla de Presupuesto Mensual**: El usuario debe poder ver y modificar una plantilla de presupuesto mensual con las categorías establecidas previamente.
    - **Dependencia**: Requisito 1.
3. **Introducción de Gastos Mensuales por Categorías**: Al final de cada mes, el usuario introducirá todos los gastos realizados para cada categoría de una vez, basándose en la plantilla mensual.
4. **Cálculo de Sobrante/Faltante Automático**: Tras registrar los gastos mensuales, la aplicación debe calcular automáticamente el sobrante o faltante de cada categoría.
    - **Dependencia**: Requisito 3.

**Requisitos de Monederos** 5. **Registro de Monederos**: El usuario puede crear, editar y eliminar monederos con saldo inicial. 6. **Asignación Automática de Sobrante/Faltante a Monederos**: Tras registrar los gastos mensuales, la aplicación asignará automáticamente el sobrante o faltante a los monederos correspondientes, según las preferencias configuradas.

- **Dependencia**: Requisito 4 y 5.

7. **Historial de Monederos**: El usuario debe poder ver el historial mes a mes de los movimientos de sus monederos.
    - **Dependencia**: Requisito 5 y 6.

**Requisitos de Visualización y Reportes** 8. **Resumen Mensual de Categorías y Monederos**: El usuario debe poder ver un resumen mensual de todas las categorías y los movimientos de monederos tras la asignación automática.

- **Dependencia**: Requisito 6.

9. **Histórico de Saldos Mensuales**: El usuario debe poder ver la progresión mes a mes de los saldos de sus monederos y la evolución de sus presupuestos.
    - **Dependencia**: Requisito 7.

---

### 2. **Dependencias entre Requisitos**

- **Requisito 2** (Plantilla de Presupuesto Mensual) depende de **Requisito 1** (Registro de Categorías), ya que las categorías se gestionan en la plantilla.
- **Requisito 4** (Cálculo de Sobrante/Faltante Automático) depende de la introducción de gastos mensuales en **Requisito 3**.
- **Requisito 6** (Asignación Automática de Sobrante/Faltante) depende de **Requisito 4** para realizar las asignaciones automáticas.
- **Requisito 7** (Historial de Monederos) depende del correcto funcionamiento de la gestión de monederos en **Requisito 5** y la asignación automática en **Requisito 6**.
- **Requisito 8** (Resumen Mensual de Categorías y Monederos) depende del correcto cálculo de sobrantes/faltantes en **Requisito 6**.
- **Requisito 9** (Histórico de Saldos Mensuales) depende del funcionamiento del historial de monederos en **Requisito 7**.

---

### 3. **Requisitos Técnicos**

- **Base de datos**: PostgreSQL o MySQL como sistema de base de datos, con las siguientes tablas principales:
    
    - **Usuarios**: Información básica y autenticación.
    - **Categorías**: Información de las categorías de gasto.
    - **Monederos**: Saldos, movimientos y asignaciones automáticas.
    - **Plantilla de Presupuesto Mensual**: Registro del presupuesto para cada mes.
    - **Transacciones Mensuales**: Registro de todos los gastos mensuales en cada categoría.
    - **Historial de Monederos**: Saldos históricos mes a mes.
- **API Backend**: Desarrollada con **FastAPI** o **Node.js** para gestionar:
    
    - CRUD de categorías y monederos.
    - Registro de gastos mensuales y cálculo de sobrantes/faltantes.
    - Asignación automática de sobrantes/faltantes a los monederos.
    - Generación de reportes mensuales e históricos.
- **Frontend**: Utilizar **Flutter** para la interfaz móvil con una navegación simple y centrada en los pasos mensuales.
    
- **Autenticación**: Implementar autenticación mediante JWT para asegurar la privacidad de los datos.
    
- **Automatización de Cálculos**: Algoritmos que procesen el sobrante/faltante tras la introducción de gastos mensuales y lo asignen a los monederos configurados por el usuario.
    
- **Seguridad**: Encriptación de los datos sensibles y protección del API con HTTPS.
    

---

### 4. **Esquema de Pantallas con Requisitos Asociados**

1. **Pantalla de Inicio de Sesión/Registro**:
    
    - El usuario puede iniciar sesión o registrarse en la aplicación.
    - **Requisitos asociados**: N/A (Requisito técnico).
2. **Pantalla Principal (Dashboard)**:
    
    - Resumen del presupuesto mensual, saldo de monederos y estado de sobrantes/faltantes.
    - Acceso directo a la introducción de gastos del mes actual.
    - **Requisitos asociados**: 8.
3. **Pantalla de Plantilla de Presupuesto Mensual**:
    
    - Mostrar la plantilla con las categorías y presupuestos establecidos.
    - Permite editar presupuestos para el mes en curso.
    - **Requisitos asociados**: 2.
4. **Pantalla de Registro de Gastos Mensuales**:
    
    - Permite al usuario introducir los gastos de todas las categorías a la vez al finalizar el mes.
    - **Requisitos asociados**: 3.
5. **Pantalla de Sobrante/Faltante Automático**:
    
    - Después de introducir los gastos mensuales, se muestran los sobrantes o faltantes.
    - El usuario confirma la asignación automática a los monederos.
    - **Requisitos asociados**: 4, 6.
6. **Pantalla de Monederos**:
    
    - Visualización de saldos actuales y variación de los monederos.
    - Acceso al historial de monederos mes a mes.
    - **Requisitos asociados**: 5, 7.
7. **Pantalla de Histórico de Monederos**:
    
    - Visualización de la evolución de los monederos en meses anteriores.
    - **Requisitos asociados**: 9.

---

### Árbol de Navegación

- **Inicio de sesión/Registro**
    - Acceso a → **Pantalla Principal (Dashboard)**
        - Acceso a → **Pantalla de Plantilla de Presupuesto Mensual**
        - Acceso a → **Pantalla de Registro de Gastos Mensuales**
            - Después de registrar gastos → **Pantalla de Sobrante/Faltante Automático**
        - Acceso a → **Pantalla de Monederos**
            - Acceso a → **Pantalla de Histórico de Monederos**

---

### 5. **Flows de Usuario**

#### **Flow 1: Registro de Gastos Mensuales y Asignación de Sobrantes/Faltantes**

1. El usuario accede a la aplicación y llega a la **Pantalla Principal (Dashboard)**.
2. Selecciona la opción de introducir gastos del mes.
3. Es redirigido a la **Pantalla de Registro de Gastos Mensuales**, donde introduce los gastos de todas las categorías.
4. Al terminar, la aplicación lo redirige automáticamente a la **Pantalla de Sobrante/Faltante Automático**.
5. La aplicación calcula el sobrante o faltante de cada categoría y lo asigna a los monederos preconfigurados.
6. El usuario confirma el proceso y vuelve al **Dashboard** con los saldos actualizados.

#### **Flow 2: Modificar Plantilla de Presupuesto Mensual**

1. El usuario accede al **Dashboard**.
2. Selecciona la opción de ver o editar la plantilla del presupuesto.
3. Es llevado a la **Pantalla de Plantilla de Presupuesto Mensual**.
4. El usuario ajusta los presupuestos para las categorías deseadas.
5. Guarda los cambios y regresa al **Dashboard**.

#### **Flow 3: Consultar Historial de Monederos**

1. El usuario accede al **Dashboard**.
2. Selecciona la opción de ver monederos.
3. Es llevado a la **Pantalla de Monederos**, donde ve el saldo actual de cada monedero.
4. Desde aquí, selecciona la opción de ver el historial de un monedero.
5. Es redirigido a la **Pantalla de Histórico de Monederos** donde puede ver la evolución mes a mes de los saldos.
6. Regresa al **Dashboard**.

#### **Flow 4: Resumen Mensual y Estado de Monederos**

1. El usuario entra a la aplicación y accede al **Dashboard**.
2. Ve el resumen mensual que incluye el presupuesto, gastos totales y movimientos en los monederos.
3. Navega a la **Pantalla de Monederos** para obtener más detalles si es necesario.
4. Regresa al **Dashboard**.

---

Este esquema de la aplicación optimiza la interacción del usuario con el nuevo proceso de registrar los gastos mensuales de una sola vez y la asignación automática de sobrantes o faltantes a monederos.