## Front Flutter
- [x] Cuando emparejo el esp32, le doy a mostrar lecturas, lee bien, vuelvo atrás, y intento volver a darle a Mostrar Lecturas, me da error en lectura_dispositivo_conectado.dart diciendo que el stream ya ha sido escuchado. ✅ 2025-01-09
- [x] Estaba haciendo la plantilla para bluetooth ble. En el proyecto pruebas_ble ya he conseguido mostrar la lista de dispositivos encontrados. Faltaría estudiarlo y migrarlo a la arquitectura buena en la aplicación plantilla_bluetooth_ble ✅ 2025-01-10
- [x] Subir las plantillas a git ✅ 2025-01-10
	- [ ] Subir la plantilla de SOLO bluetooth a git
- [x] Ya he hecho la plantilla con pantalla. He hecho la pantlla HomeScreen. Habría que sustituir el botón "Conectar" por "Ir a ajustes bluetooth", y hacer esa pantalla. Será el próximo paso, para gestionar todo el bluetooth. ✅ 2025-01-10
- [x] Ya he hecho la pantalla BluetoothSettings y está funcionando la búsqueda de dispositivos. Ahora habrá que pulirlo y hacer por ejemplo el botón desconectar (hay que ver cómo pasar el dispositivo a desconectar, si lo voy a guardar en un estado general o cómo). ✅ 2025-01-10
- [x] Me he quedado en el siguiente punto: He implementado la bbdd sqlite, y ya al finalizar una sesión de cachimba, se guardan los datos en la sqlite. Tengo que mostrar un popup de advertencia antes de finalizar la cachimba, solucioanr problema de scroll en la pantalla HOme cuando hay varias sesiones o directamente ponerlo más bonito, y solucionar que de primeras cuando abro la aplicación no me aparece ningún registro ✅ 2025-01-17
- [ ] El siguiente paso podría ser añadir una pantalla específica para listar todas las sesiones existentes, y que se pueda filtrar por fecha y demás. (punto tres sin hacer en el siguiente apartado). Además puedo hacer que se autocalibre el umbral de caladas (último punto en apartado Sensores)
- [ ] Si el sensor da temperatura superior a 100 grados o presión negativa, es que ha habido un problema con el sensor. Notificarlo al usuario.
- [ ] Si se pierde la conexión con el esp32 y/o el sensor, que haya un botón de reconectar sin tener que finalizar la sesión de cachimba, o poner un botón de reanudar sesión.

## Estadísticas
- [ ] Que al finalizar la sesión de cachimba, se muestre un resumen de la sesión (tiempo total, tiempo total fumando y purgando, número total de caladas, etc)
- [ ] Incluir en el detalle de una sesión, una gráfica con la distribución de caladas y pulgados por tiempo, para saber cuándo se ha fumado más la cachimba, si al principio, al final, etc. Para ello, hay que guardar la hora de cada calada y purgado. La gráfica debe poderse configurar en la aplicación para añadir la línea de pulgados y/o caladas etc.
- [ ] Incluir una pantalla de estadísticas generales, no solo de una sesión en concreto. Aquí se podrán ver la media de cachimbas (sesiones) hechas el último mes, día, año, o fecha que el usuario elija, media de caladas de todas las sesiones, etc.
- [x] Guardar las estadísticas en una base de datos ✅ 2025-01-11
## Bluetooth
- [ ] Ya tengo los datos leyendo. Ahora me gustaría hacer que, si la aplicación pierde la conexión con el bluetooth, se actualice el estado a desconectado.
## Sensores
- [ ] Añadir una calibración manual y/o automática del umbral de presión necesario para detectar una calada y purgado
## Hardware
- [ ] Comparar sensores, por el problema de humedad/agua con el bme280. Una opción es el MS5803 MS5803-01BA. Mirar opciones de recubrimiento como PTFE o ePTFE: https://es.aliexpress.com/item/1005003427738053.html?pdp_npi=4%40dis%21EUR%218.10%217.29%21%21%2160.27%2154.24%21%402141155017362221245676996d13b2%2112000025736876598%21affd%21%21%21&dp=Cj0KCQiA-aK8BhCDARIsAL_-H9lCpGxerQ3r6gOovt1VFRMO8ksUlnZ8fMQILSIrpG2N8hDOoxXTLwcaAnDqEALw_wcB&gad_source=1&aff_fcid=250cbd5d3f654bf69bea4d56e661e5b8-1737071727325-03621&aff_fsk&aff_platform=api-new-product-query&sk&aff_trace_key=250cbd5d3f654bf69bea4d56e661e5b8-1737071727325-03621&terminal_id=25a37199797d4023baf0197e27a90f75&afSmartRedirect=y ------ https://es.aliexpress.com/item/1005004909273512.html?pdp_npi=4%40dis%21EUR%213.10%211.89%21%21%213.12%211.90%21%402140eaab17361892004397629d135f%2112000030984241615%21affd%21%21%21&dp=Cj0KCQiA-aK8BhCDARIsAL_-H9mVI2g68R9DF7z_QbbbYLJanHxF1IwuY0DB7aw084PVdZOxPR4-GdAaAgoGEALw_wcB&gad_source=1&aff_fcid=2d1722990ede45fb8605960d0b9d01c8-1737071863341-04036&aff_fsk&aff_platform=api-new-product-query&sk&aff_trace_key=2d1722990ede45fb8605960d0b9d01c8-1737071863341-04036&terminal_id=25a37199797d4023baf0197e27a90f75&afSmartRedirect=y#nav-description
