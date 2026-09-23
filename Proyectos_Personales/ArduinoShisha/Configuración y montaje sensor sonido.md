NO VALE PORQUE NO DETECTA CALADAS O SOPLIDOS, SOLO FLUJO DE AIRE (PERO NO SU DIRECCIÓN, COMO SI HACE UN SENSOR DE PRESIÓN).

| Módulo MAX9814 pin | Conexión ESP32 (que sí tienes) |
| ------------------ | ------------------------------ |
| VDD                | 3.3V                           |
| GND                | GND                            |
| OUT                | **G35** (GPIO35, entrada ADC)  |
| GAIN               | Libre                          |
| AR                 | Libre                          |

## Codigo ejemplo
```c
const int micPin = 35;  // GPIO35 conectado al OUT del MAX9814

void setup() {
  Serial.begin(115200);
}

void loop() {
  int micValue = analogRead(micPin); // Leer la señal analógica
  Serial.println(micValue);          // Imprimir la lectura
  delay(10);
}

```
