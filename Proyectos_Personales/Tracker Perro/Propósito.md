Este proyecto consiste en desarrollar un **dispositivo de rastreo GPS para perros**, similar a **Tractive**, pero con una implementación personalizada usando hardware de bajo costo y software propio.

🔹 **Objetivo:** Crear un dispositivo portátil que se acople al collar del perro, permitiendo rastrear su ubicación en tiempo real o con intervalos configurables.  
🔹 **Componentes principales:**

- **Hardware:** ESP32/Arduino, GPS, módulo de comunicación GSM/LoRa, batería recargable.
- **Software:** Firmware del dispositivo, ~~backend para recibir y almacenar datos~~, app móvil para visualizar la ubicación.
- **Optimización:** Deep Sleep para ahorro de batería, geocercas, posible integración con NB-IoT o paneles solares.

La idea es utilizar un GPS y un LoRa para enviar los datos (iría en el collar) y un LoRa como "estación base" para recibir las señales GPS del LoRa del collar. Luego, la estación tendría que estar cerca de un móvil con la aplicación del proyecto, que recibirá mediante bluetooth las señales recibidas por el collar, y las mostraría por pantalla