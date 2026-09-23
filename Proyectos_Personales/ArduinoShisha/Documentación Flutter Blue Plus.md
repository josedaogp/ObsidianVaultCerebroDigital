#### Chequear si el dispositivo soporta bluetooth
```
await FlutterBluePls.isSupported
```

---
#### Conocer el estado del bluetooth
Devuelve un Stream que contínuamente devuelve el estado del bluetooth
```
var subscription = FlutterBluePlus.adapterState.listen((BluetoothAdapterState state) {  
	print(state);  
	if (state == BluetoothAdapterState.on) {  
	// Bluetooth is enabled, proceed with BLE operations  
	} else {  
	// Bluetooth is off or in an error state, handle appropriately  
	}  
	});
```
Claves:
*FlutterBluePlus.adapterState* (Stream\<BluetoothAdapterState\>) --> El stream
*BluetoothAdapterState* --> Lo que emite el stream. Puede ser *BluetoothAdapterState.on* o .*of*, o otros como .*unknown* o *unauthorized* (problemas con los permisos)
var *subscription* es del tipo *StreamSubscription*

**Importante:**
Cuando terminemos de escuchar el estado, cerrar el stream con:
```
subscription.cancel();
```

---
#### Cómo encender el bluetooth
En ios es automático
En android:
```
if (Platform.isAndroid) {  
await FlutterBluePlus.turnOn(); // Request the user to turn on Bluetooth  
}
```

---
#### Empezar y parar una búsqueda de dispositivos
```
// Start scanning  
await FlutterBluePlus.startScan(  
timeout: Duration(seconds: 15),  
withServices: [Guid("180D")], // Filter by service UUID (optional)  
withNames: ["Bluno"], // Filter by device name (optional)  
);
```
Claves:
- withServices y withNames son filtros en la búsqueda aplicados con un OR. Puede obviarse.
- Esto solo comienza el proceso de escaneado, y solo después de hacer esto se podrá capturar los resultados. Hay que controlar que siempre esto vaya lo primero.
##### Cómo parar el escaneo antes de que acabe el timeout
```
FlutterBluePlus.stopScan()
```

---
#### Capturar resultados de la búsqueda de dispositivos
```
var subscription = FlutterBluePlus.onScanResults.listen((results) {  
if (results.isNotEmpty) {  
ScanResult r = results.last;  
print('${r.device.remoteId}: "${r.advertisementData.localName}" found!');  
}  
}, onError: (e) => print(e));

...
...

FlutterBluePlus.cancelWhenScanComplete(subscription); // Cancel the subscription when the scan stops
```
Claves:
- *FlutterBluePlus.onScanResults* --> Es el stream\<ScanResult\> que saca los resultados en forma *ScanResult*.
- *ScanResult* tiene propiedades. La más importante es *ScanResult.device*, que contiene información del dispositivo leído.
- El nombre del dispositivo se pinta desde *ScanResult.advertisementData.localName*
##### Cómo parar el escaneo antes de que acabe el timeout
```
FlutterBluePlus.stopScan()
```
##### onScanResults vs scanResults
- *onScanResults* es un stream que devuelve solo los NUEVOS dispositivos encontrados
- *scanResults* es un stream que devuelve también los dispositivos encontrados ANTES. Por si se quiere hacer un historial

---
#### Modos de escaneo (Solo Android)
Hay tres modos, de menos a más consumo y de más a menos tiempo empleado:
- *ScanMode.lowPower*
- *ScanMode.balanced*
- *ScanMode.lowLatency*
Se utilizan como parámetro en el startScan:
```
await FlutterBluePlus.startScan(  
// ... other parameters  
scanMode: ScanMode.lowPower, // or ScanMode.balanced or ScanMode.lowLatency  
);
```

---
#### Cómo saber cuándo ha terminado de buscar dispositivos
```
// Start scanning (previous code)  
await FlutterBluePlus.startScan(...);

// Wait for scanning to stop  
await FlutterBluePlus.isScanning.where((val) => val == false).first;
```
Claves:
- *isScanning* es un stream que devuelve el estado de escaneo (true si está escaneando, false si no)
- el where se utiliza para que, cuando lo que nos devuelva sea un stream, se "pare".
- el *.first* es un Future de bool. Usamos el await para esperar a que termine el escaneo, y devolvemos el Future\<bool\>

También podemos pararlo manualmente en vez de esperar a que termine:
```
FlutterBluePlus.stopScan();
```

---
#### Cómo establecer conexión y desconectar con un dispositivo
La clase para un dispositivo es *BluetoothDevice*. El método utilizado, .connect():
```
BluetoothDevice device; // Your discovered device  
// Connect to the device  
await device.connect();
```
Apunte:
Se le puede pasar el parámetro *autoConnect: true* para que si perdemos la conexión con el dispositivo, reconecte solo:
```
await device.connect(autoConnect: true);
```
##### Conocer el estado del dispositivo (si está conectado o no)
Se hace a través del stream connectionState del dispositivo:
```
device.connectionState.listen((BluetoothConnectionState state) {  
	if (state == BluetoothConnectionState.connected) {  
		print('Connected to the device!');  
	// Proceed with discovering services  
	} else if (state == BluetoothConnectionState.disconnected) {  
		print('Disconnected from the device!');  
	// Handle disconnection  
	}  
});
```
**IMPORTANTE:**
Aunque el método connect es un Future y se hace en "una operación", tenemos que leer el estado con el stream *connectionState*.
##### Desconectar de un dispositivo:
```
await device.disconnect()
```

---
#### Descubrir servicios del dispositivo después de conectar
```
List<BluetoothService> services = await device.discoverServices();
```
Claves:
- *device.discoverServices()* --> Es el método utilizado. Devuelve una lista de *BluetoothService*.
- *BluetoothService* es la clase utilizada para un servicio del BLE

---
#### Descubrir características de un servicio
Después de obtener la lista de servicios de un dispositivo:
```
for (BluetoothService service in services) {  
	List<BluetoothCharacteristic> characteristics = service.characteristics;  
	for (BluetoothCharacteristic characteristic in characteristics) {  
		// Access characteristic properties (UUID, value, etc.)  
	}  
}
```
##### Leer una característica
```
List<int> value = await characteristic.read();
```
Nota:
- Será una lista de enteros o lo que sea que recibamos de la característica.
- Devuelve un Future\<List\<int>\> , sustituyendo int por lo que sea que devuelva 
##### Escribir en una característica
```
await characteristic.write([0x01, 0x02, 0x03]); // Example data
```
##### UUIDs: Unique Identifiers
Cada servicio y característica tiene un identificador único que lo identifica. Es un 128bit.

---
#### Notifications vs Indications
- **Notifications:** Son servicios o características (o mensajes, no se exactamente) que envía el BLE sin pedirlo. Ej: Notificación de Nivel de batería
- **Indications:** Es lo mismo que una notification, pero la app debe saber que la va a recibir. Ej: Lecturas de sensores intencionadamente.
---
#### Cómo suscribirse a una Notifications/Indications
Partiendo de que tenemos una *BluetoothCharacteristic* :
```
BluetoothCharacteristic characteristic; // Your target characteristic  
await characteristic.setNotifyValue(true);
```

Una vez estamos suscritos a la característica mediante notificación:
##### Cómo recibir datos de la característica
```
characteristic.onValueReceived.listen((value) {  
	// Process the received data (value) here  
});
```
onValueReceived es un stream que devuelve los datos.
##### Ejemplo de lectura de una característica notificada
```
heartRateCharacteristic.onValueReceived.listen((value) {  
	int heartRate = value[1]; // Assuming heart rate is in the second byte  
	print('Heart rate: $heartRate bpm');  
	// Update your UI or perform other actions based on the heart rate  
});
```
Además:
Tenemos el stream *lastValueStream* en sustitución del *onValueReceived*, que devuelve solo el último valor de la característica.
##### Dessuscribirse a una característica
```
await characteristic.setNotifyValue(false);
```
---
#### Descriptores de las características
Son como metadatos de las características, y podemos leerlos:
```
BluetoothDescriptor descriptor; // Your target descriptor  
// Read  
List<int> value = await descriptor.read();  
// Write  
await descriptor.write([0x01, 0x00]); // Example data (enabling notifications)
```
---
## Tabla resumen
| **Categoría**                 | **Acción**                                      | **Método**                 | **Objeto**                | **Retorno**                                    |
| ----------------------------- | ----------------------------------------------- | -------------------------- | ------------------------- | ---------------------------------------------- |
| **Buscar dispositivos**       | Iniciar búsqueda de dispositivos                | `startScan()`              | `FlutterBluePlus`         | `Future<void>`                                 |
|                               | Parar búsqueda de dispositivos                  | `stopScan()`               | `FlutterBluePlus`         | `Future<void>`                                 |
|                               | Capturar resultados de búsqueda                 | `onScanResults.listen()`   | `FlutterBluePlus`         | `StreamSubscription<List<ScanResult>>`         |
|                               | Saber si se está escaneando                     | `isScanning`               | `FlutterBluePlus`         | `Stream<bool>`                                 |
| **Conectar a un dispositivo** | Establecer conexión con un dispositivo          | `connect()`                | `BluetoothDevice`         | `Future<void>`                                 |
|                               | Conocer el estado de conexión de un dispositivo | `connectionState.listen()` | `BluetoothDevice`         | `StreamSubscription<BluetoothConnectionState>` |
| **Leer datos**                | Leer una característica                         | `read()`                   | `BluetoothCharacteristic` | `Future<List<int>>`                            |
|                               | Recibir datos de notificaciones/indications     | `onValueReceived.listen()` | `BluetoothCharacteristic` | `StreamSubscription<List<int>>`                |
|                               | Leer un descriptor                              | `read()`                   | `BluetoothDescriptor`     | `Future<List<int>>`                            |
| **Enviar datos**              | Escribir en una característica                  | `write()`                  | `BluetoothCharacteristic` | `Future<void>`                                 |
|                               | Escribir en un descriptor                       | `write()`                  | `BluetoothDescriptor`     | `Future<void>`                                 |
| **Desconectar dispositivo**   | Desconectar de un dispositivo                   | `disconnect()`             | `BluetoothDevice`         | `Future<void>`                                 |
| **Habilitar Bluetooth**       | Chequear si el dispositivo soporta Bluetooth    | `isSupported`              | `FlutterBluePlus`         | `Future<bool>`                                 |
|                               | Obtener el estado del Bluetooth                 | `adapterState.listen()`    | `FlutterBluePlus`         | `StreamSubscription<BluetoothAdapterState>`    |
|                               | Encender el Bluetooth (Android)                 | `turnOn()`                 | `FlutterBluePlus`         | `Future<void>`                                 |
## Resumen de pasos
1. `startScan()` --> `onScanResults` (`Stream<ScanResult>`) --> `ScanResult.device` (`BluetoothDevice`)
2. `BluetoothDevice.connect()` --> `connectionState` (`Stream<BluetoothConnectionState>`)
3. `BluetoothDevice.discoverServices()` --> `List<BluetoothService>`
4. `BluetoothService.characteristics` --> `List<BluetoothCharacteristic>`
5. `BluetoothCharacteristic.read()` --> `Future<List<int>>`
6. `BluetoothCharacteristic.write()` --> `Future<void>`
7. `BluetoothCharacteristic.setNotifyValue(true)` --> `onValueReceived` (`Stream<List<int>>`)
8. `BluetoothDevice.disconnect()` --> `Future<void>`
---
## Resumen dependencias entre métodos
### **1. Buscar dispositivos**

- **Método:** `startScan()`
    
    - No retorna un objeto directamente. Inicia la búsqueda de dispositivos BLE.
- **Stream:** `onScanResults`
    
    - **Retorno:** `Stream<List<ScanResult>>`
    - Cada `ScanResult` tiene la propiedad `device` de tipo `BluetoothDevice`.
    - **Uso:** Extraes el `BluetoothDevice` del último resultado o del que te interese:
        
        dart
        
        Copiar código
        
        `ScanResult r = results.last;   BluetoothDevice device = r.device;`  
        

---

### **2. Conectar a un dispositivo**

- **Objeto Necesario:** `BluetoothDevice`
    
    - **Origen:** Obtenido desde un `ScanResult.device` (ver paso anterior).
- **Método:** `connect()`
    
    - Retorna un `Future<void>` cuando la conexión se establece exitosamente.
- **Stream para monitorear estado:** `connectionState`
    
    - **Retorno:** `Stream<BluetoothConnectionState>`
    - Útil para saber si el dispositivo está conectado o desconectado.

---

### **3. Descubrir servicios y características de un dispositivo conectado**

- **Método:** `discoverServices()`
    
    - **Retorno:** `Future<List<BluetoothService>>`
    - Cada `BluetoothService` representa un servicio BLE del dispositivo.
- **Propiedad de servicio:** `characteristics`
    
    - **Tipo:** `List<BluetoothCharacteristic>`
    - Cada `BluetoothCharacteristic` representa una característica del servicio.

---

### **4. Leer datos de una característica**

- **Objeto Necesario:** `BluetoothCharacteristic`
    
    - **Origen:** Obtenido desde `service.characteristics` (ver paso anterior).
- **Método:** `read()`
    
    - **Retorno:** `Future<List<int>>`
    - Contiene los datos leídos de la característica.

---

### **5. Escribir datos en una característica**

- **Objeto Necesario:** `BluetoothCharacteristic`
    
    - **Origen:** Igual que en el paso anterior.
- **Método:** `write()`
    
    - **Retorno:** `Future<void>`
    - Envia los datos al dispositivo.

---

### **6. Suscribirse a notificaciones de una característica**

- **Objeto Necesario:** `BluetoothCharacteristic`
    
    - **Origen:** Igual que en pasos anteriores.
- **Método:** `setNotifyValue(true)`
    
    - **Retorno:** `Future<void>`
- **Stream:** `onValueReceived`
    
    - **Retorno:** `Stream<List<int>>`
    - Emite los datos enviados por el dispositivo automáticamente.

---

### **7. Desconectar de un dispositivo**

- **Objeto Necesario:** `BluetoothDevice`
    
    - **Origen:** Obtenido en el paso de "Buscar dispositivos".
- **Método:** `disconnect()`
    
    - **Retorno:** `Future<void>`
---

## Bibliografía
- https://medium.com/@sparkleo/the-essentials-core-ble-concepts-and-flutter-blue-plus-1df8820b9651
- https://medium.com/@sparkleo/services-and-characteristics-the-building-blocks-of-ble-communication-3ac2dcf19d1e
- https://medium.com/@sparkleo/advanced-ble-development-with-flutter-blue-plus-ec6dd17bf275 (NO AÑADIDA EN ESTE DOCUMENTO. EXPLICA COSAS COMO CORRER EN BACKGROUND)
- https://medium.com/@martijn.van.welie/making-android-ble-work-part-1-a736dcd53b02 (POR REVISAR)


