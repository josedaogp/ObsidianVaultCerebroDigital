Entendido, vamos a explorar **Flutter Bluetooth Serial** para detallar las acciones principales que puedes realizar, qué objetos se involucran y qué métodos de la biblioteca debes usar. Aquí tienes un desglose de las funciones clave:

---

### 1. **Buscar dispositivos emparejados**

#### **Método**:

dart

Copiar código

`Future<List<BluetoothDevice>> getBondedDevices()`

#### **Descripción**:

Obtiene una lista de dispositivos emparejados.

#### **Tipo de objeto recibido**:

`List<BluetoothDevice>`  
Cada `BluetoothDevice` tiene propiedades como:

- `name`: Nombre del dispositivo (puede ser `null` si no está disponible).
- `address`: Dirección MAC del dispositivo.
- `type`: Tipo de dispositivo (`BluetoothDeviceType`).
- `isConnected`: Si está conectado actualmente (`bool`).

#### **Ejemplo**:

dart

Copiar código

`final List<BluetoothDevice> devices = await FlutterBluetoothSerial.instance.getBondedDevices(); for (BluetoothDevice device in devices) {   print("Device: ${device.name}, Address: ${device.address}"); }`

---

### 2. **Conectar a un dispositivo**

#### **Método**:

dart

Copiar código

`Future<BluetoothConnection> connect(String address)`

#### **Descripción**:

Establece una conexión con un dispositivo por su dirección MAC.

#### **Tipo de objeto recibido**:

`BluetoothConnection`  
Este objeto tiene métodos y propiedades importantes:

- `isConnected`: Indica si la conexión está activa.
- `input`: Un `Stream<Uint8List>` con los datos recibidos.
- `output`: Un `BluetoothConnectionOutput` para enviar datos.

#### **Ejemplo**:

dart

Copiar código

`BluetoothConnection connection; try {   connection = await BluetoothConnection.toAddress("00:11:22:33:44:55");   print('Connected to the device'); } catch (e) {   print('Could not connect: $e'); }`

---

### 3. **Leer datos del dispositivo**

#### **Método**:

Usa el `Stream<Uint8List>` proporcionado por `BluetoothConnection.input`.

#### **Descripción**:

Se suscribe al flujo de datos recibidos desde el dispositivo.

#### **Tipo de objeto recibido**:

`Uint8List` (lista de bytes). Puedes convertirlo en texto o procesarlo según lo esperado.

#### **Ejemplo**:

dart

Copiar código

`connection.input!.listen((Uint8List data) {   print('Data received: ${String.fromCharCodes(data)}'); }).onDone(() {   print('Disconnected by remote device'); });`

---

### 4. **Enviar datos al dispositivo**

#### **Método**:

Usa `BluetoothConnection.output.add()`.

#### **Descripción**:

Envía un conjunto de bytes al dispositivo conectado.

#### **Tipo de objeto enviado**:

`Uint8List` (lista de bytes). Convierte texto a bytes si es necesario.

#### **Ejemplo**:

dart

Copiar código

`String message = "Hello ESP32"; connection.output.add(Uint8List.fromList(message.codeUnits)); await connection.output.allSent; // Espera a que se envíen todos los datos`

---

### 5. **Desconectar un dispositivo**

#### **Método**:

dart

Copiar código

`Future<void> close()`

#### **Descripción**:

Cierra la conexión con el dispositivo.

#### **Ejemplo**:

dart

Copiar código

`await connection.close(); print("Connection closed");`

---

### 6. **Habilitar Bluetooth**

#### **Método**:

dart

Copiar código

`Future<bool> requestEnable()`

#### **Descripción**:

Solicita al usuario que habilite Bluetooth si no está activo.

#### **Ejemplo**:

dart

Copiar código

`bool isEnabled = await FlutterBluetoothSerial.instance.requestEnable(); if (isEnabled) {   print("Bluetooth enabled"); } else {   print("Bluetooth not enabled"); }`

---

### Resumen de Métodos Principales y su Relación

| **Acción**                | **Método**                           | **Objeto Involucrado**    |
| ------------------------- | ------------------------------------ | ------------------------- |
| Buscar dispositivos       | `getBondedDevices()`                 | `List<BluetoothDevice>`   |
| Conectar a un dispositivo | `connect(String address)`            | `BluetoothConnection`     |
| Leer datos                | `BluetoothConnection.input.listen()` | `Stream<Uint8List>`       |
| Enviar datos              | `BluetoothConnection.output.add()`   | `Uint8List`               |
| Desconectar dispositivo   | `BluetoothConnection.close()`        | Ninguno (cierra conexión) |
| Habilitar Bluetooth       | `requestEnable()`                    | `bool`                    |