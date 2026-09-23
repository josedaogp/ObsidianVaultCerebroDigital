## **🎯 1. VISIÓN GENERAL**

### **Propósito**

Aplicación web de gestión financiera personal basada en el modelo YNAB (You Need A Budget) que permite gestionar presupuestos mensuales, monederos por objetivos y seguimiento de activos.

### **Modelo Base: YNAB**

- **Principio fundamental**: Cada euro debe tener un propósito asignado
- **Gestión por categorías**: Presupuestos mensuales por categoría de gasto
- **Monederos por objetivos**: Fondos específicos para metas (emergencias, vacaciones, etc.)
- **Control de excesos**: Cuando se supera un presupuesto, debe compensarse desde algún lugar


---

## **🏗️ 2. ARQUITECTURA DE DATOS**

### **2.1 Entidades Principales**

#### **Categorías (Categories)**

```typescript
interface Category {
  id: string
  name: string
  type: "gasto" | "gasto_acumulativo" | "gasto_mixto" | "gasto_acumulativo_opcional"
  monthlyBudget: number
  annualBudget?: number
  active: boolean
  walletId?: string
}
```

**Tipos de Categorías:**

- **`gasto`**: Gasto normal mensual
- **`gasto_acumulativo`**: Si sobra dinero, va al monedero asociado
- **`gasto_mixto`**: Combina características de gasto normal y acumulativo
- **`gasto_acumulativo_opcional`**: Similar a acumulativo pero opcional


#### **Monederos (Wallets)**

```typescript
interface Wallet {
  id: string
  name: string
  currentBalance: number
  targetBalance?: number
}
```

#### **Bienes/Activos (Assets)**

```typescript
interface Asset {
  id: string
  name: string
  type: "cuenta_bancaria" | "efectivo" | "inversion" | "propiedad" | "otro"
  currentBalance: number
}
```

#### **Ingestas Mensuales (Monthly Ingestions)**

```typescript
interface MonthlyIngestion {
  id: string
  month: number
  year: number
  date: string
  expenses: { [categoryId: string]: number }
  incomes: Income[]
  categoryExpenses: CategoryExpense[]
  walletAdjustments: { [categoryId: string]: string }
  distributionRules: DistributionRule[]
}
```

---

## **🔧 3. FUNCIONALIDADES PRINCIPALES**

### **3.1 Gestión de Configuración**

#### **Categorías**

- ✅ CRUD completo de categorías
- ✅ Tipos diferenciados con comportamientos específicos
- ✅ Asociación opcional con monederos
- ✅ Estado activo/inactivo
- ✅ Presupuestos mensuales y anuales


#### **Monederos**

- ✅ CRUD completo de monederos
- ✅ Saldo actual y objetivo
- ✅ Cálculo automático de progreso
- ✅ Visualización de diferencias y porcentajes


#### **Bienes**

- ✅ CRUD completo de activos
- ✅ Tipos diferenciados (bancario, efectivo, inversión, etc.)
- ✅ Seguimiento de saldos


### **3.2 Ingesta Mensual**

#### **Proceso de Ingesta**

1. **Selección de período**: Mes y año (no duplicados)
2. **Gastos por categoría**:

3. Introducir gasto real vs presupuesto
4. Gestión de excesos y sobrantes
5. Asignación de monederos para cobertura



6. **Ingresos del mes**:

7. Cantidad, bien asociado, descripción
8. Validación de bien obligatorio si hay cantidad



9. **Cálculo automático del bote mensual**
10. **Distribución del bote según reglas configuradas**


#### **Reglas de Negocio Críticas**

**Gestión de Excesos:**

- Si gasto > presupuesto = EXCESO
- El usuario puede elegir:

- **Monedero específico**: El exceso se descuenta del monedero
- **Bote mensual**: El exceso se descuenta automáticamente del bote



- **Categorías acumulativas**: OBLIGATORIO asignar monedero si hay gasto


**Gestión de Sobrantes:**

- Si gasto < presupuesto en categoría acumulativa = SOBRANTE
- El sobrante va automáticamente al monedero asociado


**Cálculo del Bote Mensual:**

```plaintext
Bote = Ingresos - Gastos + Excesos_cubiertos_por_monederos
```

- Los excesos cubiertos por monederos NO reducen el bote
- Los excesos sin monedero asignado SÍ reducen el bote


### **3.3 Configuración de Distribución**

#### **Sistema de Reglas Avanzado**

- **Tipos de reglas**:

- `percentage`: Porcentaje del bote
- `fixed`: Cantidad fija en euros



- **Prioridades**: Orden de aplicación (1 = mayor prioridad)
- **Proceso de distribución**:

1. Aplicar cantidades fijas por orden de prioridad
2. Aplicar porcentajes sobre el remanente
3. Validación: porcentajes deben sumar 100%





#### **Validaciones**

- ✅ Suma de porcentajes = 100% (exacto)
- ✅ No permitir guardar si distribución incorrecta
- ✅ Simulador en tiempo real
- ✅ Interfaz para agregar/quitar monederos


---

## **📊 4. PANTALLAS Y FUNCIONALIDADES**

### **4.1 Histórico (Página Principal)**

- ✅ Selector de mes/año
- ✅ Resumen financiero del mes seleccionado
- ✅ Comparación presupuesto vs gasto real por categoría
- ✅ Progreso visual con barras
- ✅ Estado de excesos y sobrantes
- ✅ Lista de ingresos del mes


### **4.2 Resumen Financiero**

- ✅ Consulta por fecha específica
- ✅ Estado de monederos con progreso hacia objetivos
- ✅ Estado de bienes con distribución porcentual
- ✅ Patrimonio total calculado
- ✅ Evolución mensual reciente


### **4.3 Evolución de Monederos**

- ✅ **Funcionalidad clave**: "Capturas" mensuales de monederos
- ✅ Scroll horizontal con tarjetas por mes
- ✅ Filtros por fecha y monedero específico
- ✅ Cálculo de cambios y porcentajes de crecimiento
- ✅ Resumen de tendencias del período
- ✅ Enlaces directos a ingestas específicas


### **4.4 Ingesta Mensual**

- ✅ Validación de duplicados
- ✅ Persistencia temporal de datos (no se pierden al navegar)
- ✅ Gestión avanzada de excesos con selección de cobertura
- ✅ Configuración inline de distribución
- ✅ Resumen en tiempo real de movimientos
- ✅ Simulación de distribución del bote


### **4.5 Configuración de Redistribución**

- ✅ Sistema de reglas con tipos y prioridades
- ✅ Validación en tiempo real
- ✅ Simulador de distribución
- ✅ Interfaz para agregar monederos con selector
- ✅ Explicación clara del funcionamiento


### **4.6 Importación/Exportación**

#### **Importar Meses**

- ✅ Importación de ingestas mensuales en JSON
- ✅ Validación completa de estructura
- ✅ Prevención de duplicados
- ✅ Generación de ejemplos
- ✅ Exportación de datos actuales


#### **Importación Inicial**

- ✅ Importación masiva de configuración inicial
- ✅ Categorías, monederos y bienes en un solo JSON
- ✅ Validación específica por tipo de entidad
- ✅ Reemplazo completo con advertencias
- ✅ Documentación de formato integrada


---

## **⚙️ 5. REGLAS DE NEGOCIO ESPECÍFICAS**

### **5.1 Validaciones Críticas**

1. **No duplicar meses**: Un mes/año solo puede tener una ingesta
2. **Categorías acumulativas**: Si tienen gasto, DEBEN tener monedero
3. **Ingresos**: Si tienen cantidad > 0, DEBEN tener bien asociado
4. **Distribución**: Debe sumar exactamente 100% si hay reglas activas
5. **Excesos**: Pueden ir a monedero específico o al bote mensual


### **5.2 Cálculos Automáticos**

1. **Progreso de monederos**: `(saldo_actual / saldo_objetivo) * 100`
2. **Bote mensual**: `ingresos - gastos + excesos_cubiertos_por_monederos`
3. **Distribución**: Primero cantidades fijas, luego porcentajes
4. **Sobrantes acumulativos**: `presupuesto - gasto_real` → monedero


### **5.3 Actualizaciones de Saldos**

- **Al completar ingesta**: Actualizar saldos de monederos y bienes
- **Monederos**: Aplicar excesos, sobrantes y distribución del bote
- **Bienes**: Sumar ingresos del mes
- **Persistencia**: Guardar en localStorage (preparado para BD)


---

## **🎨 6. EXPERIENCIA DE USUARIO**

### **6.1 Navegación**

- ✅ Menú lateral fijo con orden lógico
- ✅ Estados activos claros
- ✅ Iconos consistentes


### **6.2 Feedback Visual**

- ✅ Alertas contextuales (éxito, error, advertencia)
- ✅ Barras de progreso para objetivos
- ✅ Colores semánticos (verde=positivo, rojo=negativo)
- ✅ Badges para estados y tipos


### **6.3 Validación en Tiempo Real**

- ✅ Errores mostrados inmediatamente
- ✅ Botones deshabilitados si hay errores
- ✅ Mensajes explicativos claros
- ✅ Simuladores para previsualizar resultados


---

## **🔄 7. FLUJO DE TRABAJO TÍPICO**

### **7.1 Configuración Inicial**

1. **Importación inicial** o creación manual de:

2. Categorías con tipos y presupuestos
3. Monederos con objetivos
4. Bienes con saldos iniciales



5. **Configurar distribución** del bote mensual
6. **Validar** que todo suma 100%


### **7.2 Uso Mensual**

1. **Ingesta mensual**:

2. Introducir gastos reales por categoría
3. Gestionar excesos (monedero o bote)
4. Introducir ingresos con bienes asociados
5. Revisar distribución del bote



6. **Guardar ingesta** → actualización automática de saldos
7. **Revisar evolución** en pantallas de resumen


### **7.3 Seguimiento**

1. **Histórico**: Ver meses anteriores
2. **Evolución**: Analizar tendencias de monederos
3. **Resumen**: Estado actual del patrimonio
4. **Ajustes**: Modificar presupuestos o distribución según necesidad


---

## **🚀 8. CONSIDERACIONES TÉCNICAS**

### **8.1 Persistencia**

- **Actual**: localStorage (funcional para demo/desarrollo)
- **Futuro**: Supabase con esquema proporcionado
- **Migración**: Funciones de importación/exportación facilitan transición


### **8.2 Validación**

- **Frontend**: Validación inmediata para UX
- **Backend**: Validación en BD con constraints
- **Consistencia**: Mismas reglas en ambos lados


### **8.3 Escalabilidad**

- **Estructura modular**: Fácil agregar nuevas funcionalidades
- **Componentes reutilizables**: UI consistente
- **Separación de responsabilidades**: Lógica de negocio separada de UI


---

## **✅ 9. FUNCIONALIDADES IMPLEMENTADAS**

### **9.1 Correcciones Evolutivas Aplicadas**

1. **Validación de distribución**: Debe sumar exactamente 100%
2. **Gestión de excesos mejorada**: Opción de monedero o bote mensual
3. **Cálculo correcto del bote**: Excesos cubiertos por monederos no lo reducen
4. **Evolución como capturas**: Vista horizontal de "fotos" mensuales
5. **Importación/exportación**: Sistema completo de migración de datos
6. **Persistencia temporal**: No se pierden datos al navegar durante ingesta
7. **Validaciones en tiempo real**: Feedback inmediato al usuario


### **9.2 Estado Actual**

- ✅ **100% funcional** para uso personal/familiar
- ✅ **Preparado para producción** con Supabase
- ✅ **Documentado completamente** para mantenimiento
- ✅ **Validado** con casos de uso reales
- ✅ **Escalable** para nuevas funcionalidades


---

**🎯 Esta aplicación implementa completamente el modelo YNAB con mejoras específicas para gestión de monederos por objetivos y seguimiento detallado de patrimonio.**