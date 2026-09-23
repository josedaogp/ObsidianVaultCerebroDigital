## Enlace de referencia
https://naylampmechatronics.com/blog/24_configuracion-del-modulo-bluetooth-hc-05-usando-comandos-at.html
## Cómo conectar el módulo bluetooth HC-05 con el PC y configurarlo
Sketch de arduino:

```
#include <SoftwareSerial.h>   // Incluimos la librería  SoftwareSerial  

SoftwareSerial BT(10,11);    // Definimos los pines RX y TX del Arduino conectados al Bluetooth

void setup()

{

  BT.begin(9600);       // Inicializamos el puerto serie BT (Para Modo AT 2)

  Serial.begin(9600);   // Inicializamos  el puerto serie  

  Serial.println("El módulo BT está listo.");

}

void loop()

{

  if(BT.available())    // Si llega un dato por el puerto BT se envía al monitor serial

  {

    //Serial.println("BT.available");

    Serial.write(BT.read());

  }

  if(Serial.available())  // Si llega un dato por el monitor serial se envía al puerto BT

  {

    //Serial.println("Serial.available");

     BT.write(Serial.read());

  }

}
```

Tendremos que enchufar el Arduino con el botón del HC-05 pulsado para que entre en modo AT2 (parpadeo lento). En este estado, en teoría el HC-05 está a 38400 baudios.

En el serial monitor, poner Both NL & CR y 9600 baud.

Ahora si enviamos AT deberemos recibir OK.
