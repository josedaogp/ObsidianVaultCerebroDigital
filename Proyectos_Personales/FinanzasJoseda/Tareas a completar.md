## Generales
- [ ] Corregir y armonizar la nomenclatura de vistas, páginas, modelos, etc.
- [ ] Pensar y hacer cómo conseguir registrar los cambios en los monederos. Quizá dejar la tabla Monederos y crear otra "registros Monederos" con la fecha de los monederos y su cantidad en dicha fecha.
- [ ] Añadir la fecha a la tabla sobrantes para que haya registro temporal
- [ ] Incluir los ingresos y el reasignado del sobrante total a categorías predefinidas por porcentajes
- [ ] Manejar los errores de la aplicación/back
- [x] Crear la pantalla principal bonita y con un menú con el resto de opciones. La pantalla principal será en un principio la de historial de gastos, pero en un futuro quizá sería interesante que fuera la de historial de monederos/bienes ✅ 2024-10-29
- [ ] Incluir SUBcategorias
- [ ] Incluir una estadística de descompensación entre monederos y bienes
- [ ] limitar la resta para obtener el sobrante en el back en create_bulk_gastos a tres o cuatro cifras
## Gestionar Categorías
- [x] Realizar el backend ✅ 2024-10-23
- [x] Arreglar los acentos ✅ 2024-10-29
- [ ] Meter en bbdd la propiedad booleana de si está activa o no e implementarlo en el front
- [x] Hacer la pantalla de Insertar Categoria ✅ 2024-10-24
- [ ] Hacer que la página de Gestionar Categorias se recargue automáticamente cuando se vuelve del Insertar Categoria
- [x] Mezclar con la tabla de tipoCategorias para que en vez de aparecer el identificador de la categoría, aparezca el nombre. ✅ 2024-10-30
- [ ] Que se pueda modificar también el tipo de categoría
- [ ] Ordenar las categorias por nombre
## Gestionar Monederos (Hecho)
- [ ] Ordenar listas de monederos por orden alfabético para que si se modifica alguno, no aparezca el último 
- [x] Hacer el Insertar un Monedero ✅ 2024-10-23
- [ ] Hacer que la página de Gestionar Monederos se recargue automáticamente cuando se vuelve del insertar Monedero
- [ ] Meter que un Monedero pueda estar activo o no, como en las categorías
- [ ] Añadir la fecha objetivo y cuándo se alcanzará
- [ ] Si edito el saldo objetivo y no lo modifico, que no lance la query al back
## Gestionar Bienes (Hecho)
- [ ] Hacer que la página de Gestionar Bienes se recargue automáticamente cuando se vuelve del insertar Bien
## Prueba histórico de meses ya registrados
- [x] Realizar el backend ✅ 2024-10-24
- [x] Arreglar que cuando se muestra un mes, se está filtrando solo por el mes y no tiene en cuenta el año, por lo que trae la misma categoría dos veces ✅ 2024-10-29
- [x] Mejorar el datapicker de historial de gastos ✅ 2024-10-29
## insertar mes
- [x] Realizar el backend ✅ 2024-10-24
- [x] Adaptarlo a la nueva interfaz ✅ 2024-10-24
- [x] Añadir la selección del monedero al que va el sobrante o de donde se coge el exceso ✅ 2024-10-25
- [x] Actualizar los monederos con los sobrantes/excesos de los gastos ✅ 2024-10-28
- [ ] Hacer que el saldo restante sea común a todos los gastos. Actualmente, se muestra en tiempo real cómo quedaría el saldo del monedero X si se le añade el sobrante o excedente de un gasto, pero no se tiene en cuenta si el sobrante/excedente de un gasto anterior se ha asignado también a ese mismo monedero.
- [x] Solo mostrar el desplegable de seleccionar monedero cuando el sobrante sea distinto a cero ✅ 2024-10-28
- [x] Ver la lógica de "eliminar un sobrante" después de que se seleccione un monedero al que se quiere que vaya ✅ 2024-10-30
- [x] Ver cómo hacer para meter el id del gasto asociado. ✅ 2024-10-30
- [ ] Hacer que no se pueda enviar si no se han metido también los monederos a los que se quiere destinar el sobrante. SOLO cuando se haya implementado el tipo de categoría (que es lo que dirá si el sobrante va a un monedero o no)
- [ ] Que si una categoría se llama igual que el monedero y es una categoría acumulativa propia, se autoseleccione el monedero que se llama igual
### Ingresos
- [x] Probar el insertado. El json que genera entra bien si lo meto en swagger ✅ 2025-02-05
- [x] El monto que se está asignando no es correcto ✅ 2025-02-04
- [x] Debuggear en el back a ver qué hace, y modificar el retorno ✅ 2025-02-05
- [ ]
## Monederos 
- [x] Realizar backend --> Ya está hecho porque ya tenemos la pantalla de gestioanr monederos ✅ 2024-10-28


## Próximas tareas
- [x] Crear modelos de excesoGasto y sobranteGasto para poder utilizarlos en el front ✅ 2024-10-28
- [x] Cuando se vaya a insertar un gasto, desgranar y crear la lista de excesos/sobrantes para enviarlos al back ✅ 2024-10-28
- [x] En el back, cuando se reciba un listado de gastos y otro de excedentes/sobrantes, actualizar los monederos ✅ 2024-10-28
- [x] Cambiar la lista de meses (fila_lista_meses) por un desplegable para seleccionar el mes. Esto seguramente incluya un cambio en la estructura de toda la pantalla. ✅ 2024-10-29
- [x] crear tipocategoria en introducir categoria, ✅ 2024-10-30
- [x] Arreglar el insertar un mes gasto. El sobrante no encuentra el id del gasto que acaba de crear en teoría. Eso está en línea 134 de intro_gasto_categoria_widget.dart ✅ 2024-10-30
- [ ] Si al meter un mes la fecha se selecciona después de meter un gasto, la fecha del gasto queda vacía
- [ ] aplicar el tipo de categoria en la seleccion o no del monedero al Introducir un Gasto ( Tener en cuenta el tipo de categoría del gasto)
- [ ] Crear la lógica de los ingresos (back y front) implicará también actualizar los bienes
- [ ] Que se pueda seleccionar el bien al que va el ingreso y al meter el ingreso se añada al bien correspondiente, y hacer los bienes editables (esto antes, porque con eso ya sería funcional). Cuando haga el mes, tendré que revisar el dinero que tengo en cada bien y actualizarlo.