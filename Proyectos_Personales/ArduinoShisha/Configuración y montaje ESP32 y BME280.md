BME280 tiene cuatro cables:
**VIN:** 3.3V hasta 5V. Yo lo he conectado al 3.3
**GND:** Tierra
**SCL:** Pin 22 (G22 O IO22)
**SDA:** Pin 21 (G21 O IO21)

Código de ejemplo (Con librería Adafruit BME 280 y Adafruit Sensor):

```
#include <Adafruit_Sensor.h>
#include <Adafruit_BME280.h>

// Crear una instancia del sensor BME280
Adafruit_BME280 bme;
  
void setup(){
  // Iniciar comunicación con el sensor BME280
  while (!bme.begin(0x76)) { // 0x76 teniendo en cuenta que SCL conectamos al pin 22 del ESP32 y el SDA al pin 21

    Serial.println("No se encontró el sensor BME280. Verifique la conexión.");

    delay(2000);

  }
    Serial.println("Conectado a BME280.");

}
```

Cuando haga el código del Arduino, hay que asegurar que solo se envían datos si el dispositivo está conectado a la aplicación, si no, me he encontrado con esta casuística:
- Tengo el esp32 apagado, lo enciendo. - Enciendo la aplicación y busco dispositivos - Encuentra el esp32 - Le doy a conectar - Conecta bien - Le doy a desconectar - Aparentemente desconecta bien (luego explico por qué) - Intento conectar de nuevo (aquí debugeo y el device aparece con el status desconectado, por eso pienso que aparentemente desconecta bien) - al cabo de unos segundos, da el error FlutterBluePlusException | connect | android-code: 133 - apago el esp32 (la aplicación se queda encendida con la lista de dispositivos abierta) - vuelvo a encender el esp32 - le doy a buscar de nuevo en la aplicación - encuentra el esp32 - le doy a conectar - conecta correctamente

Lo solucioné pidíendole a ChatGPT que me actualizara el código del esp32: Podemos añadir un log para cuando me conecte desde mi aplicación, y para cuando se desconecte?

## Problemas BME280
Con la humedad que habrá en la manguera de la cachimba y/o el agua que le pueda caer, se puede romper o dejar de funcionar. Hay otras opciones como el MS5803 MS5803-01BA. [[Otras alternativas de medición]]
