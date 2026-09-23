- [x] Arreglar dar de alta un cliente ✅ 2024-11-29
- [x] Filtrar cliente por nombre en la pantalla de clientes ✅ 2024-11-29
- [ ] Mostrar albaranes, anticipos y liquidaciones de un cliente una vez seleccionado en la pantalla de cliente 
- [x] Añadir socio en la tabla de albaranes de getAlbaranes ⏫ ✅ 2024-11-29
- [ ] Que cuando se introduzca un albarán se muestren bien (había que llamar a alguna funcion de clear o algo así⏫ )
- [x] Meter los socios en el get de los albaranes, como está en la liquidacion ✅ 2024-11-29
- [x] Incluir una función para imprimir todas las liquidaciones de un dia (preguntar a Mere si sería útil/necesario). NO ✅ 2024-12-02  
- [x] Hacer NO editables los campos de las tablas. ✅ 2024-12-02
- [ ] Añadir en la pantalla de socios para que se muestren los albaranes del socio que tengas seleccionado.⏫ 
- [ ] Generar un modelo con todas las propiedades de la liquidacion que se imprime, por tenerlo contemplado. El modelo de la bbdd es inútil.
- [ ] Cuando se meta el precio de la aceituna, te tiene que pedir también esos tres datos. Una vez metidos, se actualizarán todos los albaranes.⏫ 
- [ ] Poner la pantalla de parámetros generales en la pantalla de selección de temporada
- [ ] Utilizar los modelos para el CRUD de todas las pantallas y no un diccionario
- [x] Ahora mismo en la liquidación, el subtotal, haber, deber y demás lo calculo complejamente. En realidad, ahora que he añadido que cuando se cambie el precio de la aceituna se añada el subtotal y demás del albarán, al calcular la liquidación solo tendría que hacer sumas. En cualquier caso, tendría que dar lo mismo. ✅ 2024-12-19
- [x] Que solo aparezcan los anticipos de la temporada seleccionada ✅ 2024-12-30
- [ ] Revisar si se puede meter un albarán sin numero de albaran

## Anotaciones
- Tener en cuenta en la liquidación final, que solo saque las liquidaciones de los socios que hayan participado en la temporada.
- En la liquidación total no aparecen todos los albaranes si no el total de kg por cada tipo de aceituna que ha llevado el socio.
- En el albarán, no hay que meter el iva. El iva, retencion y comisiones va en el tipo de aceituna. Cuando se meta el precio de la aceituna, te tiene que pedir también esos tres datos. Una vez metidos, se actualizarán todos los albaranes. -->
	neto*precioaceituna = base_imponible_sin_comisiones
	base_imponible_sin_comisiones  - porcent.comisiones = base_imponible_con_comisiones
	base_imponible_con_comisiones - neto * gastos = base_imponible
	base_imponible * 12 iva = subtotal
	subtotal - porcentaje IRPF = totalfinal

Insertar anticipo
- [x] COmpensacion sobra ✅ 2024-12-01
- la retención irpf no tiene na que ver con el parámetro general
- [x] Meter el socio al mostrar un anticipo ✅ 2024-12-01

Insertar tipo de aceituna
- La comisión es un porcentaje
- [ ] Al editar un tipo de aceituna, que no siempre tenga que actualizar los albaranes.

Inertar albaran
- que el numero de albaran salga solo con el numero y el año de temporada --> Más adelante
- Cuando se meta el albarán, poner como activo al socio.
- [x] Meter neto que es bruto - tara. ✅ 2024-12-01

Gestión Socios
- [ ] Meter un botón para mostrar los anticipos y albaranes del socio seleccionado.
- [ ] Que cuando se meta un albarán, se ponga al socio como activo. Al iniciar una nueva temporada, poner todos los socios en desactivos.⏫ 
- [ ] Hacer bien el insertar socio. Quitar id_localidad

Seleccion Temporada
- [x] Que no te deje ir al menú principal habiendo seleccionado una temporada ya cerrada ✅ 2024-12-02

Imprimir liquidaciones
- [x] Que cree la carpeta de la temporada si no existe ✅ 2024-12-09
- [x] Incluir iva y irpf de parametrosgenerales en la impresion de liquidacion ✅ 2024-12-09
- [x] Incluir la temporada dinámica. ✅ 2024-12-09
- [ ] Añadir la propiedad "final" a la liquidacion para ver si es la de cierre de temporada o no --> Da igual porque creo que no voy a guardar las liquidaciones en bbdd
- [ ] Crear tupla en la bbdd --> Creo que no lo voy a hacer
- [ ] Cuando se hagan las liquidaciones finales, insertarlas como finales en la bbdd y borrar todos los registros de liquidaciones de esa temporada que no sean finales. Añadir por tanto un parámetro a la función que las imprime para que las finales las meta en otro directorio
- [ ] Si hay algún problema al generar la liquidación, que lo saque por pantalla.

Parametros
- [ ] Quitar tipo de impresora
- [ ] No poner como editables parámetros como el último backup y eso

## Por donde me he quedado:
- [x] Hacer que el botón de ir al menú principal funcione. ✅ 2024-12-02
- [x] Añadir temporada que aparezca en una pantalla aparte y no ahí en medio. ✅ 2024-12-02
- [x] Cerrar temporada no funciona. ✅ 2024-12-02
	- [x] Y añadirle que haga la liquidacion ✅ 2024-12-03
- [x] Imprimir liquidaciones ✅ 2024-12-09
- [x] Estaba metiendo que cuando se edite precio aceituna, se modifiquen todos los albaranes. Mirar git para ver lo que llevaba, pero tenía que cambiar el edit_tipo_aceituna de tipos_aceituna_screen.py ✅ 2024-12-18
- [ ] La gestión de anticipos solo muestra un anticipo pero hay dos en la base de datos. (cuando se selecciona el socio del anticipo que no sale y se le da a mostrar anticipos del socio, si aparece). Será 100% seguro porque da la excepción del siguiente punto que he apuntado y se sale del for.
	- [ ] Además, da un error porque no puede sumar bruto mas algo no se que
	- [ ] Probar que mostrar los anticipos de un socio funciona
- [ ] Que al volver atrás en la pantalla de albaranes o anticipos abiertas desde la pantalla del socio, vuelva a la del socio y no al menú principal

## Release
- [x] Me he quedado viendo cómo exportar las fuentes cuando genero el .exe. En el liquidaciones_service.py y en otros sitios, tengo la ruta ABSOLUTA y hay que ponerla relativa. Línea 217. ✅ 2025-01-21
	- [ ] Meter el cambio en todos las fuentes
- [ ] Anotar todo el proceso. Dejar anotados los pasos de crear una carpeta con el nombre concreto cooperativaaceitehinojoslgpl o el que sea si lo hago de cero, probar a ponerlo en /C y dejar anotados los pasos para generar la base de datos y importar los socios desde el csv.
- [ ] Para más adelante, puedo hacer un fichero de configuración de donde se pillen las cosas, como el POSSIST. Ahí metería la ruta de la fuente (dependiendo de si está empaquetado en el .exe o o no), el nombre del contenedor de la base de datos, etc.
## Pruebas Mere
- [x] ordenar albaranes y socios alfabéticamente por defecto y en las pantallas de insertar albarán, anticipo y eso ✅ 2025-01-27
- [x] las fechas, quitar el almanaque, ponerlo para escribir y dejarlo en blanco para meterlo a mano ✅ 2025-01-27
- [x] que el ultimo albaran que se meta salga el primero y no el último, lo mismo para anticipos y demás, porque al meterlo sale abajo y para verificar que lo ha metido bien se tiene que ir abajo. ✅ 2025-01-28
- [ ] añadir un campo para el iva ya calculado que se llame "Cuota IVA" (el que está sería el porcentaje). 
- [x] Y añadir en anticipos y albaranes el símbolo de porcentaje, euros, kilos y demás unidades para que el no tenga que meterlas. ✅ 2025-01-27
- [ ] Y que se autocalcule todo (eso verlo él), aunque eso último no hace falta.
- [x] si metes un albaran con id duplicado, te salta un mensaje pero se cierra la pantalla de inserción. Controlarlo para que no tenga que volver a meter todos los datos ✅ 2025-01-28
- [x] resaltar cuando un socio y fila están seleccionados ✅ 2025-01-28

## Cómo sacar los requerimientos de python
pip freeze > requirements.txt

