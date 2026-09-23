### Esquema de la Base de Datos

#### 1. **Usuarios**

- **user_id** (PK): Identificador único para cada usuario.
- **nombre**: Nombre del usuario.
- **email**: Correo electrónico del usuario.
- **fecha_creacion**: Fecha de creación del usuario.

#### 2. **Categorías** - categoria_gasto

- **categoria_id** (PK): Identificador único de la categoría.
- **nombre**: Nombre de la categoría (Ejemplo: Restaurantes, Gasolina, etc.).
- **tipo**: Tipo de categoría (acumulativa, no acumulativa, mixta). Según el color que tienes asignado en la hoja.
- **es_acumulativa**: Booleano que indica si es una categoría que acumula sobrante o no.
- **usuario_id** (FK): Relación con el usuario propietario de la categoría.

#### 3. **Monederos**

- **monedero_id** (PK): Identificador único del monedero.
- **nombre**: Nombre del monedero (Ejemplo: Viaje, Coche nuevo, etc.).
- **saldo_actual**: El saldo actual del monedero.
- **usuario_id** (FK): Relación con el usuario propietario del monedero.

#### 4. **Presupuestos Mensuales**

- **presupuesto_id** (PK): Identificador único del presupuesto.
- **categoria_id** (FK): Relación con la tabla de categorías.
- **usuario_id** (FK): Relación con el usuario.
- **monto_presupuestado**: Monto presupuestado para la categoría en el mes.
- **mes**: Mes del presupuesto (en formato YYYY-MM).
- **gasto_total**: El gasto total registrado en esa categoría para el mes.

#### 5. **Transacciones**

- **transaccion_id** (PK): Identificador único de la transacción.
- **usuario_id** (FK): Relación con el usuario propietario.
- **categoria_id** (FK): Relación con la tabla de categorías.
- **monedero_id** (FK) (nullable): Relación opcional con un monedero (para categorías acumulativas).
- **monto**: Monto de la transacción (gasto o ingreso).
- **fecha**: Fecha de la transacción.
- **tipo_transaccion**: Tipo de transacción (gasto, ingreso).
- **mes**: Mes de la transacción (en formato YYYY-MM), para asociarla a un mes concreto.

#### 6. **Historial de Monederos**

- **historial_id** (PK): Identificador único del historial.
- **monedero_id** (FK): Relación con la tabla de monederos.
- **mes**: Mes en que se registró el estado del monedero (en formato YYYY-MM).
- **saldo_inicial**: El saldo del monedero al inicio del mes.
- **saldo_final**: El saldo del monedero al finalizar el mes.
- **variacion_saldo**: Diferencia de saldo del mes (positivo o negativo).
- **usuario_id** (FK): Relación con el usuario propietario del monedero.

---

### Explicación del Esquema

1. **Usuarios**: Mantenemos la información básica de los usuarios.
    
2. **Categorías**: Las categorías se dividen en **acumulativas** y **no acumulativas** (o mixtas). Para las acumulativas, si hay sobrante al final del mes, se puede transferir el saldo sobrante a un **monedero**.
    
3. **Monederos**: Son las "cuentas" donde se acumulan o de donde se descuentan los sobrantes de las categorías. En la tabla de transacciones, se podrá vincular una transacción con un monedero para reflejar las transferencias de sobrantes.
    
4. **Presupuestos Mensuales**: Contiene el **presupuesto** asignado para cada categoría cada mes. Refleja cuánto se ha asignado a la categoría y cuánto se ha gastado.
    
5. **Transacciones**: Cada registro de transacción (gasto o ingreso) vinculado a una categoría y opcionalmente a un monedero. Esto permite gestionar tanto los gastos como los ingresos en cada categoría y actualizar el saldo de los monederos.
    
6. **Historial de Monederos**: Almacena un histórico del saldo de cada monedero, con las variaciones mes a mes para que puedas analizar la progresión de los saldos en cada uno.
    

### Implementación de la lógica de "Exceso compensado"

- Para **categorías acumulativas**, si sobra dinero al final del mes, puedes asignar este sobrante a un monedero de forma manual. Esto se puede reflejar en la **tabla de transacciones** vinculando el movimiento de sobra de la categoría al monedero correspondiente.
- Para **categorías no acumulativas**, el gasto de más o de menos se considera solo para el balance del mes sin impacto en monederos.

### Funcionalidades

1. **Registro de Presupuesto y Gasto Mensual**: Registrar cuánto se asigna y se gasta en cada categoría.
2. **Visualización del Estado de los Monederos**: Mostrar cuánto dinero hay en cada monedero al inicio y final de cada mes.
3. **Historial de Monederos**: Permite consultar la evolución de los monederos a lo largo del tiempo.
4. **Registro de Transacciones**: Para cada gasto o ingreso en una categoría y la posibilidad de transferir excedentes a monederos.