## Contenidos del directorio

```folder-index-content
```

| #  | Componente                     | Descripción                           | Collar (Emisor)   | Estación Base (Receptor) | Precio Aproximado (USD) |
|----|---------------------------------|---------------------------------------|-------------------|--------------------------|-------------------------|
| 1  | **ESP32**                       | Microcontrolador con Wi-Fi y Bluetooth | X                 | X                        | $6 - $10               |
| 2  | **Módulo LoRa (SX1276/RFM95)**  | Comunicación LoRa (frecuencia 915 MHz o 433 MHz) | X                 | X                        | $5 - $10               |
| 3  | **Módulo GPS (Neo-6M)**         | Módulo GPS para obtener coordenadas    | X                 |                          | $7 - $12               |
| 4  | **Batería Li-ion 18650**        | Batería recargable Li-ion 3.7V 3000mAh | X                 | X                        | $4 - $8                |
| 5  | **Portabatería Li-ion 18650**   | Soporte para la batería Li-ion 18650  | X                 | X                        | $2 - $4                |
| 6  | **TP4056**                      | Módulo de carga y protección para Li-ion | X                 | X                        | $1 - $2                |
| 7  | **Regulador de Voltaje (DC-DC)**| Regulador Step-down 3.3V o 5V        | X (si es necesario) | X (si es necesario)      | $1 - $3                |
| 8  | **Cables Jumper**               | Cables para conectar los componentes  | X                 | X                        | $2 - $5                |
| 9  | **Antena LoRa**                 | Antena para la transmisión LoRa       | X                 | X                        | $2 - $5                |
| 10 | **Módulo Bluetooth (BLE)**      | Para comunicación Bluetooth con el móvil (si es necesario) |                   | X                        | $2 - $4                |
| 11 | **Conector USB**                | Para carga de la batería Li-ion       | X                 | X                        | $1 - $3                |

### Resumen de Precios Aproximados por Dispositivo:

- **Collar (Emisor)**:  
  **$27 - $50 USD**  
  (incluye ESP32, LoRa, GPS, batería, portabatería, TP4056, cables, antena)

- **Estación Base (Receptor)**:  
  **$20 - $39 USD**  
  (incluye ESP32, LoRa, regulador de voltaje, Bluetooth, cables, antena)

### Notas Importantes:
- **Módulo LoRa**: Hay varias versiones del módulo LoRa (SX1276, RFM95, etc.), por lo que el precio puede variar un poco según el modelo.
- **GPS**: El **Neo-6M** es uno de los más comunes y económicos. Puedes encontrar módulos de GPS con o sin antena externa.
- **Batería y Carga**: El módulo **TP4056** es muy asequible y viene con protección para la batería Li-ion, pero asegúrate de que esté bien conectado y que la batería esté protegida contra sobrecarga.
- **Regulador de Voltaje**: Si decides usar un regulador step-down (DC-DC), asegúrate de que la salida sea adecuada para el ESP32 (3.3V o 5V, según el caso).

### Total Aproximado:

- Para el **collar** y la **estación base**, el precio total rondaría entre **$50 y $90 USD** en función de los componentes que escojas y las variaciones de precio en los proveedores.

## Documentación
- **Rastreador GPS basado en LoRa usando Arduino**: https://ecuarobot.com/2020/06/16/rastreador-gps-basado-en-lora-usando-arduino-y-lora-shield/
- Cómo integrar LoRa en arduino: https://www.somosmakers.cl/como-usar-lora-con-arduino-lgpl/
- Cómo utilizar GPS en arduino: https://www.luisllamas.es/localizacion-gps-con-arduino-y-los-modulos-gps-neo-6/