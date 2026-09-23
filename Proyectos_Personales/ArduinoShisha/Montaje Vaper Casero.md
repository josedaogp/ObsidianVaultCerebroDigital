## 1. Lista de componentes

| Componente                                    | Cantidad    | Especificación / Ejemplo                                              | Precio aproximado |
| --------------------------------------------- | ----------- | --------------------------------------------------------------------- | ----------------- |
| **ESP32 Dev Board**                           | 1           | DOIT, WROOM-32, con pines expuestos                                   | 6 €               |
| **Pulsador DPDT momentáneo**                  | 1           | 2× circuitos (uno para el coil, otro para detección); 5 A@12 V        | 2.50 €            |
| **Resistencia de coil OXVA Xlim Pro**         | 1           | 1.2–1.4 Ω, imán + bornes positivo/negativo                            | ya la tienes      |
| **Portacarga / pines magnéticos (pogo pins)** | 1           | Módulo con 2 pines spring-loaded, 2 mm de paso                        | 4 €               |
| **Batería 18650**                             | 1–2         | LG/MuRata INR18650 3.7 V, 3000 mAh                                    | 4 € / ud.         |
| **Porta 18650 con protección PCB**            | 1           | Protección contra sobre-descarga y cortocircuito                      | 3 €               |
| **Cables**                                    | —           | • AWG 20 (2 hilos) para potencia  <br>• AWG 28 (3–4 hilos) para señal | 5 € (pack)        |
| **Resistencias de pull-down/pull-up**         | 1 pack      | 10 kΩ, 1/4 W                                                          | 1 €               |
| **Conectores macho-hembra dupont**            | según pines | Para protoboard / soldar al ESP32                                     | 1 €               |
| **PCB o placa prototipo**                     | 1           | 5×7 cm, montaje TH (tornillos/soldadura)                              | 2 €               |
| **Tornillería y separadores**                 | —           | Nylon, M2.5 para fijar ESP32 y PCB                                    | 1 €               |
| **Filamento PLA / PETG**                      | —           | Para carcasa 3D                                                       | 15 € / kg         |
## 2. Recomendaciones de compra

- **ESP32**: elige uno con antena cerámica y castellanos pines. Ej: “Lolin D32”.
    
- **Pulsador DPDT**: busca momentáneo (no bistable) para detectar cada calada. 5 A de contacto es suficiente para 3–4 A de coil.
    
- **Pogo pins**: módulos ya soldados a PCB: p.ej. “2-pin Magnetic Charging Connector”.
    
- **Protección para 18650**: imprescindible si cargas externamente, para evitar sobredescarga (< 2.5 V) o cortos.
    
- **Cables**: AWG 20 para circuito de potencia; AWG 28 para I/O del ESP32.
    
- **PCB**: una plaquita universa tipo “protoboard rígida” evita cables sueltos.
    

Puedes encontrar todo en **AliExpress**, **Amazon** o **eBay**, buscando por referencia. En tiendas locales de electrónica (p.ej. Aragón Electrónica, BricoGeek) suele haber ESP32 y componentes pasivos.
## 3. Esquema de montaje

```
   ┌─────────────────────────────────────────────┐
   │                                             │
   │   [ Batería 18650 + Protección PCB ]        │
   │             │                               │
   │             │ (+)                           │
   │             │                               │
   │         ┌───┴───┐       ┌───────────┐        │
   │         │ DPDT  │──────▶│ Coil pod  │        │
   │         │Push   │       └──┬────────┘        │
   │         └───┬───┘          │                 │
   │             │ (–)          │                 │
   │             │              ▼                 │
   │             │        ┌───────────┐           │
   │             │        │  GND ESP  │           │
   │             │        └───────────┘           │
   │             │              ▲                 │
   │             │              │                 │
   │             │    ┌─────────┴────────┐        │
   │             │    │ DPDT 2º polo     │        │
   │             └───▶│ al ESP32 INPUT   │        │
   │                  └──────────────────┘        │
   │                                             │
   │   [      ESP32 Dev Board      ]              │
   │   • Detecta cierre del push ➔ cuenta calada  │
   │   • Mide duración entre pulsos               │
   │   • Envía por BLE a Flutter app             │
   │                                             │
   └─────────────────────────────────────────────┘

```

## 4. Diseño 3D de la carcasa

- **Compartimentos**: uno para la pila (con tapa deslizante), otro para ESP32 + PCB.
    
- **Ranura magnética**: posicionado donde encaje el pod.
    
- **Soporte de pulsador**: frontal, alineado con tu dedo índice.
    
- **Ventilación**: orificios pequeños para disipar calor del coil.
    
- **Ajustes/Tornillos**: esquinas con separadores para PCB y módulo imán.
    

Te sugiero diseñarlo en **Fusion 360** o **FreeCAD**, exportar como STL y probar iteraciones con impresora FDM. Utiliza paredes de 2 – 3 mm y relleno 20%.