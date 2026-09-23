# 🧠 **Canvas Explicativo: Página de Ingesta Mensual (IngestaPage)**

---

## **1. Propósito y Flujo Global**

Esta pantalla es el _core_ mensual del modelo YNAB personalizado. Aquí es donde:

- El usuario selecciona **mes y año** (prevención de duplicados).
    
- Registra **gastos reales** en cada categoría, y especifica de dónde se cubren excesos (bote o monedero).
    
- Registra **ingresos** del mes, y los asocia a un activo.
    
- Visualiza **cómo se mueven los fondos** entre categorías, monederos y activos.
    
- Define y simula **cómo se distribuye el "bote mensual"** restante según reglas (porcentaje/cantidad fija, prioridades).
    
- Guarda todo en `localStorage`, actualiza balances y lanza eventos de actualización.
    

---

## **2. Estado y Datos Locales**

Usa `useState` para toda la gestión de datos en la página:

- `month`, `year` – periodo seleccionado
    
- `categories`, `wallets`, `assets` – entidades base cargadas de localStorage
    
- `categoryExpenses` – cuánto se ha gastado realmente por categoría y a qué monedero está asociado el exceso/sobrante
    
- `incomes` – ingresos del mes, cada uno con cantidad, bien y descripción
    
- `distributionRules` – cómo repartir el bote mensual entre monederos (tipo y prioridad)
    
- `errors` – errores de validación
    

Se hace **persistencia temporal** en `localStorage` para no perder datos si el usuario navega.

---

## **3. Carga y Validación de Datos**

- **Al cargar la página**: lee datos temporales o los del mes/año seleccionados, y actualiza todo el estado.
    
- **Carga de entidades base**: `categories`, `wallets`, `assets` de localStorage (como JSON).
    
- **Carga o inicialización de reglas de distribución**: Si no existen, reparte por defecto el bote mensual a todos los monederos activos en partes iguales.
    

---

## **4. Gestión de Gastos por Categoría**

Por cada **categoría activa**:

- El usuario introduce el gasto real (`amount`).
    
- Si es **acumulativa** o tiene exceso, debe elegir a qué monedero se asigna el movimiento (exceso o sobrante).
    
- Se calcula:
    
    - **Exceso**: gasto real > presupuesto → debe ser cubierto (por monedero o bote).
        
    - **Sobrante**: gasto real < presupuesto (solo acumulativas) → va al monedero.
        
- UI muestra alertas de validación, selección obligatoria de monedero, y barras de progreso.
    

---

## **5. Gestión de Ingresos**

- Lista editable de ingresos:
    
    - **Cantidad**, **bien asociado** (obligatorio si cantidad > 0), **descripción**.
        
- Añadir/quitar ingresos.
    
- Validaciones automáticas (no se puede guardar si algún ingreso no tiene bien).
    

---

## **6. Distribución y Movimientos de Monederos**

### **Cálculo de movimientos**:

- Por cada categoría:
    
    - Si hay **exceso** y está cubierto por monedero, se descuenta de ese monedero.
        
    - Si hay **sobrante** y la categoría es acumulativa, se suma al monedero.
        
- Muestra resumen de todos los movimientos (entradas/salidas) por monedero.
    

### **Cálculo del Bote Mensual**:

plaintext

CopiarEditar

`Bote = Ingresos - Gastos + Excesos cubiertos por monederos`

- Los excesos no cubiertos por monedero se descuentan del bote directamente.
    

### **Distribución del Bote**:

- Aplicación de reglas: primero cantidades fijas por prioridad, luego porcentajes sobre el remanente.
    
- La configuración se hace vía un diálogo, editable en tiempo real.
    
- Validación: porcentajes deben sumar 100% si hay reglas activas.
    

---

## **7. Validaciones Críticas**

- **No duplicar mes/año** (clave primaria lógica): impide guardar si ya existe ingesta para ese mes/año.
    
- **Categorías acumulativas**: con gasto deben tener monedero asignado.
    
- **Ingresos**: cada ingreso con cantidad > 0 debe tener bien.
    
- **Distribución**: suma porcentual = 100%.
    
- Todos los errores se muestran en alertas y bloquean el guardado.
    

---

## **8. Guardado y Actualización de Saldos**

Al pulsar **Guardar Ingesta**:

1. Valida todos los datos.
    
2. Convierte los gastos por categoría y reglas a la estructura final.
    
3. Actualiza:
    
    - **Monederos**: aplica movimientos de excesos/sobrantes y suma la distribución del bote mensual.
        
    - **Activos**: suma los ingresos asociados a cada uno.
        
4. Persiste los cambios en localStorage.
    
5. Lanza evento para refrescar otras pantallas (ej: histórico).
    
6. Limpia datos temporales.
    

---

## **9. Experiencia de Usuario y Visualización**

- **Resumen mensual**: muestra presupuesto, gastado, ingresos y bote.
    
- **Movimientos**: breakdown de movimientos en monederos.
    
- **Distribución del bote**: vista clara, editable y simulada en tiempo real.
    
- **Feedback visual**: alertas, badges, progreso, deshabilita botones si hay errores.
    

---

## **10. Componentes y UI**

- Usa **Shadcn/UI** para todos los componentes.
    
- Separación visual por tarjetas.
    
- Dialogo para edición avanzada de reglas de distribución.
    
- **ProgressBar** para visualización del gasto vs presupuesto.
    
- **Select** para elegir monederos y bienes.
    
- **Alert** para avisos y errores.
    
- **Badge** para tipos de categoría y estados especiales.
    

---

# **Resumen Visual del Flujo (Canvas rápido)**

plaintext

CopiarEditar

`┌───────────────┐        ┌───────────────┐       ┌───────────────┐ │ Selector mes  │──────▶│ Gastos/Cats   │─────▶ │ Movimientos   │ │ y año         │       │ (inputs +     │       │ de monederos  │ │               │       │ validaciones) │       │ (excesos/     │ └───────────────┘       └───────────────┘       │ sobrantes)    │                                                  └────┬─────────┘                                                       │ ┌───────────────┐        ┌───────────────┐       ┌────▼─────────┐ │ Ingresos      │──────▶│ Reglas        │─────▶ │ Resumen      │ │ (asociados a  │       │ de distribución│      │ y distribución│ │ bienes)       │       │ (modal editable)│     │ del bote      │ └───────────────┘       └───────────────┘       └───────────────┘`

---

# **11. Consejos y Puntos de Mantenimiento**

- **Todas las reglas de negocio críticas** están centralizadas en las funciones de validación y cálculo.
    
- **Persistencia**: Se puede migrar fácilmente a Supabase porque la estructura está bien modularizada.
    
- **Experiencia UX**: Persistencia temporal + feedback inmediato = imposible perder datos.
    
- **Fácil de ampliar**: Añadir tipos de movimientos, nuevas reglas, o visualizaciones.
    
- **Separación**: Lógica y visual bien separadas. Ideal para refactorizar a componentes pequeños si quieres.