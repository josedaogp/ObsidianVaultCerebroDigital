Tengo una aplicación flutter que tiene que leer datos por bluetooth BLE de un esp32. Hasta ahora tengo hecha ya toda la gestión de conexión y desconexión de bluetooth y quiero hacer una pantalla para leer los datos que me envía el esp32.

Tengo este provider que gestiona el bluetooth:

import 'dart:async';

import 'package:flutter/material.dart';

import 'package:flutter_blue_plus/flutter_blue_plus.dart';

import '../../../../domain/repositories/bluetooth_repository.dart';

class BluetoothSettingsController extends ChangeNotifier {

final BluetoothRepository _repository;

bool _isBluetoothOn = false;

bool get isBluetoothOn => _isBluetoothOn;

void set isBluetoothOn(bool value) {

_isBluetoothOn = value;

}

bool _isScanning = false;

bool get isScanning => _isScanning;

void set isScanning(bool value) {

_isScanning = value;

}

bool _isDeviceConnected = false;

bool get isDeviceConnected => _isDeviceConnected;

void set isDeviceConnected(bool value) {

_isDeviceConnected = value;

}

List<ScanResult> _scanResults = [];

List<ScanResult> get scanResults => _scanResults;

StreamSubscription? _adapterStateSubscription;

StreamSubscription? _scanResultsSubscription;

BluetoothDevice? _connectedDevice;

BluetoothDevice? get connectedDevice => _connectedDevice;

BluetoothSettingsController(this._repository) {

_initialize();

}

void _initialize() async {

// Escuchar el estado del adaptador Bluetooth

_adapterStateSubscription =

_repository.listenToAdapterState().listen((state) {

_isBluetoothOn = state == BluetoothAdapterState.on;

notifyListeners();

});

// Escuchar los resultados de escaneo

_scanResultsSubscription =

_repository.listenToScanResults().listen((results) {

_scanResults = results;

notifyListeners();

});

}

Future<void> toggleBluetooth(bool value) async {

if (value) {

await _repository.turnOnBluetooth();

} else {

// Aquí podrías apagarlo si tu repositorio lo soporta

}

}

Future<void> startScan() async {

if (!_isBluetoothOn) return;

_isScanning = true;

notifyListeners();

await _repository.startScan(timeout: const Duration(seconds: 10));

_isScanning =

false; //TO-DO: No funciona porque no tengo forma de saber cuándo ha terminado de escanear realmente. Cuando sale de la llamada startScan, en realidad no ha terminado. Quizá hay que hacer un cancelWhenScanComplete en algún sitio.

notifyListeners();

}

Future<void> stopScan() async {

_isScanning = false;

notifyListeners();

await _repository.stopScan();

}

Future<void> connectToDevice(BluetoothDevice device,

{int retries = 3}) async {

if (!device.isConnected && _connectedDevice == null) {

int attempt = 0;

while (attempt < retries && !_isDeviceConnected) {

try {

await _repository.connectToDevice(device);

print("Connected to device ${device.advName} on attempt $attempt");

_isDeviceConnected = true;

_connectedDevice = device;

// Solicitar un MTU mayor (por ejemplo, 512 bytes)

const desiredMtu = 512;

await device.requestMtu(desiredMtu);

notifyListeners();

} catch (e) {

// Manejo de errores si la conexión falla

print("Error connecting to device: $e");

attempt++;

}

}

} else {

print("El dispositivo ${device.advName} ya está conectado");

}

}

Future<void> disconnectDevice() async {

if (_connectedDevice != null && _isDeviceConnected) {

try {

await _repository.disconnectFromDevice(_connectedDevice!);

_connectedDevice = null;

_isDeviceConnected = false;

notifyListeners();

} catch (e) {

// Manejo de errores si la desconexión falla

print("Error disconnecting from device: $e");

}

}

}

@override

void dispose() {

_adapterStateSubscription?.cancel();

_scanResultsSubscription?.cancel();

disconnectDevice();

_repository.dispose();

super.dispose();

}

}

Y este es el código del esp32 en arduino ide:

#include <Wire.h>

#include <Adafruit_Sensor.h>

#include <Adafruit_BME280.h>

#include <BLEDevice.h>

#include <BLEServer.h>

#include <BLEUtils.h>

#include <BLE2902.h>

// Crear una instancia del sensor BME280

Adafruit_BME280 bme;

// Definir el UUID del servicio y la característica BLE

#define SERVICE_UUID "12345678-1234-1234-1234-123456789abc"

#define CHARACTERISTIC_UUID "87654321-4321-4321-4321-cba987654321"

BLEServer *pServer = NULL;

BLECharacteristic *pCharacteristic = NULL;

bool deviceConnected = false;

float temperature = 0;

float humidity = 0;

float pressure = 0;

int id = 0;

// Clase para gestionar los eventos de conexión y desconexión

class MyServerCallbacks : public BLEServerCallbacks {

void onConnect(BLEServer* pServer) {

deviceConnected = true;

Serial.println("Dispositivo conectado.");

}

void onDisconnect(BLEServer* pServer) {

deviceConnected = false;

Serial.println("Dispositivo desconectado.");

// Reiniciar la publicidad BLE para permitir nuevas conexiones

BLEDevice::startAdvertising();

Serial.println("Esperando nuevas conexiones...");

}

};

void setup() {

Serial.begin(9600); // Iniciar el monitor serial

Serial.println("Iniciando BLE...");

// Inicializar BLE

BLEDevice::init("ArduinoShishaJD");

pServer = BLEDevice::createServer();

pServer->setCallbacks(new MyServerCallbacks()); // Asignar los callbacks de conexión y desconexión

// Crear un servicio BLE

BLEService *pService = pServer->createService(SERVICE_UUID);

// Crear una característica BLE

pCharacteristic = pService->createCharacteristic(

CHARACTERISTIC_UUID,

BLECharacteristic::PROPERTY_READ | BLECharacteristic::PROPERTY_NOTIFY

);

// Habilitar las notificaciones para la característica

pCharacteristic->addDescriptor(new BLE2902());

// Iniciar el servicio

pService->start();

// Iniciar publicidad BLE

BLEAdvertising *pAdvertising = BLEDevice::getAdvertising();

pAdvertising->addServiceUUID(SERVICE_UUID);

pAdvertising->start();

Serial.println("BLE iniciado. Esperando conexiones...");

// Iniciar comunicación con el sensor BME280

while (!bme.begin(0x76)) { // Dirección I2C del BME280

Serial.println("No se encontró el sensor BME280. Verifique la conexión.");

delay(2000);

}

Serial.println("Conectado a BME280.");

}

void loop() {

if (deviceConnected) { // Solo enviar datos si hay un dispositivo conectado

// Leer datos del sensor BME280

temperature = bme.readTemperature();

humidity = bme.readHumidity();

pressure = bme.readPressure() / 100.0F; // Conversión a hPa

id++;

// Crear la cadena de datos

String data = "INICIO" + String(temperature) + ";" + String(humidity) + ";" + String(pressure) + ";" + String(id) + "FIN";

//Serial.println(data);

// Enviar los datos a través de BLE

pCharacteristic->setValue(data.c_str());

pCharacteristic->notify();

// Esperar 500 ms

delay(500);

}

}

También tengo un repositorio que gestiona toda la lógica:

import 'dart:async';

import 'package:flutter_blue_plus/flutter_blue_plus.dart';

abstract class BluetoothRepository {

// Métodos de escaneo

Future<void> startScan({

List<Guid>? withServices,

List<String>? withNames,

Duration timeout,

});

Future<void> stopScan();

Stream<List<ScanResult>> listenToScanResults();

Stream<bool> isScanning();

// Métodos de conexión

Future<void> connectToDevice(BluetoothDevice device, {bool autoConnect});

Future<void> disconnectFromDevice(BluetoothDevice device);

Stream<BluetoothConnectionState> listenToConnectionState(

BluetoothDevice device);

// Métodos de servicios y características

Future<List<BluetoothService>> discoverServices(BluetoothDevice device);

Future<List<int>> readCharacteristic(BluetoothCharacteristic characteristic);

Future<void> writeCharacteristic(

BluetoothCharacteristic characteristic, List<int> data);

Future<void> subscribeToNotifications(BluetoothCharacteristic characteristic);

Stream<List<int>> listenToCharacteristicNotifications(

BluetoothCharacteristic characteristic);

Future<void> unsubscribeFromNotifications(

BluetoothCharacteristic characteristic);

Future<List<int>> readDescriptor(BluetoothDescriptor descriptor);

Future<void> writeDescriptor(BluetoothDescriptor descriptor, List<int> data);

// Métodos de estado

Future<bool> isBluetoothSupported();

Stream<BluetoothAdapterState> listenToAdapterState();

Future<void> turnOnBluetooth();

Future<void> dispose();

}

Y tengo una pantalla que funcionaba con bluetooth serial. De aquí coge solo el diseño:

import 'package:flutter/material.dart';

// import 'package:flutter_bluetooth_serial/flutter_bluetooth_serial.dart';

import 'package:front_arduino_shisha/app/presentation/pages/lectura_dispositivo_conectado/controller/lectura_dispositivo_conectado_controller.dart';

import 'package:provider/provider.dart';

import 'widgets/sensor_data_card.dart';

import 'widgets/stat_card.dart';

class LecturaDispConectado extends StatelessWidget {

const LecturaDispConectado({super.key});

@override

Widget build(BuildContext context) {

//ME HE QUEDADO INTENTANDO LLAMAR DOS VECES AL MOSTRAR LECTURAS PERO DA ERROR AQUI

LecturaDispConectadoController controller = context.watch();

// BluetoothDevice dispositivo = controller.dispConectado;

return Scaffold(

appBar: AppBar(

leading: IconButton(

icon: Icon(Icons.arrow_back),

onPressed: () async {

await controller.pararLectura();

Navigator.of(context).pop();

},

),

),

body: SingleChildScrollView(

child: Padding(

padding: const EdgeInsets.all(16.0),

child: Column(

crossAxisAlignment: CrossAxisAlignment.stretch,

children: [

SensorDataCard(

temperature: controller.temperatura,

pressure: controller.presion,

humidity: controller.humedad,

messageId: controller.idMensaje,

),

const SizedBox(height: 16),

GridView.count(

crossAxisCount: 2,

crossAxisSpacing: 16,

mainAxisSpacing: 16,

shrinkWrap: true,

physics: const NeverScrollableScrollPhysics(),

children: [

StatCard(

title: 'Session Duration',

value: "100", //TO-DO: Meter la duración de la sesión

icon: Icons.timer,

),

StatCard(

title: 'Número de caladas',

value: controller.caladas.toString(),

icon: Icons.air,

),

StatCard(

title: 'Duración de última calada',

value: '${controller.duracionCaladaActual.inSeconds}s',

icon: Icons.timelapse,

),

StatCard(

title: 'Número de purgados',

value: controller.purgados.toString(),

icon: Icons.air,

),

StatCard(

title: 'Duración de último purgado',

value: '${controller.duracionSoplidoActual.inSeconds}s',

icon: Icons.timelapse,

),

],

),

const SizedBox(height: 16),

ElevatedButton(

onPressed: () async {

await controller.pararLectura();

},

child: const Text('Finish Session'),

),

],

),

),

),

);

}

Widget _buildDataRow(String label, String value) {

return Padding(

padding: const EdgeInsets.symmetric(vertical: 4.0),

child: Row(

children: [

Text('$label: ', style: TextStyle(fontWeight: FontWeight.bold)),

Text(value),

],

),

);

}

}

Esta pantalla utiliza dos widgets propios:

import 'package:flutter/material.dart';

class SensorDataCard extends StatelessWidget {

final double temperature;

final double pressure;

final double humidity;

final String messageId;

const SensorDataCard({

Key? key,

required this.temperature,

required this.pressure,

required this.humidity,

required this.messageId,

}) : super(key: key);

@override

Widget build(BuildContext context) {

return Card(

child: Padding(

padding: const EdgeInsets.all(16.0),

child: Column(

crossAxisAlignment: CrossAxisAlignment.start,

children: [

const Text(

'Real-time Sensor Data',

style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold),

),

const SizedBox(height: 16),

Row(

mainAxisAlignment: MainAxisAlignment.spaceBetween,

children: [

_buildSensorItem(Icons.thermostat, 'Temperature',

'${temperature.toStringAsFixed(2)}°C'),

_buildSensorItem(Icons.compress, 'Pressure',

'${pressure.toStringAsFixed(2)} bar'),

],

),

const SizedBox(height: 16),

Row(

mainAxisAlignment: MainAxisAlignment.spaceBetween,

children: [

_buildSensorItem(Icons.opacity, 'Humidity',

'${humidity.toStringAsFixed(2)}%'),

_buildSensorItem(Icons.message, 'Message ID', messageId),

],

),

],

),

),

);

}

Widget _buildSensorItem(IconData icon, String label, String value) {

return Column(

children: [

Icon(icon, size: 24),

const SizedBox(height: 4),

Text(label, style: const TextStyle(fontSize: 12)),

const SizedBox(height: 4),

Text(value,

style: const TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),

],

);

}

}

Y este:

import 'package:flutter/material.dart';

class StatCard extends StatelessWidget {

final String title;

final String value;

final IconData icon;

const StatCard({

Key? key,

required this.title,

required this.value,

required this.icon,

}) : super(key: key);

@override

Widget build(BuildContext context) {

return Card(

child: Padding(

padding: const EdgeInsets.all(16.0),

child: Column(

mainAxisAlignment: MainAxisAlignment.center,

children: [

Icon(icon, size: 32),

const SizedBox(height: 8),

Text(

title,

style: const TextStyle(fontSize: 14),

textAlign: TextAlign.center,

),

const SizedBox(height: 8),

Text(

value,

style: const TextStyle(fontSize: 24, fontWeight: FontWeight.bold),

textAlign: TextAlign.center,

),

],

),

),

);

}

}
Además la pantalla tenía este controlador, que a mi la mitad no me servirá porque la conexión ya lo hago en mi provider:

import 'dart:async';

import 'dart:typed_data';

import 'package:flutter/material.dart';

import 'package:flutter_bluetooth_serial/flutter_bluetooth_serial.dart';

class LecturaDispConectadoController extends ChangeNotifier {

LecturaDispConectadoController({

required this.dispConectado,

required this.conexionBl,

}) {

leerDatos(); // Iniciar la lectura de datos cuando el controlador se crea

}

//Parámetros iniciales controlador

BluetoothDevice dispConectado;

BluetoothConnection conexionBl;

//Para la lectura de datos

String buffer = '';

List<String> datosParseados = List.empty();

//Datos parseados

double presionInicial = 0;

double presion = 0;

double temperatura = 0;

double humedad = 0;

String idMensaje = '';

int caladas = 0;

int purgados = 0;

bool enCalada = false;

bool enSoplido = false;

DateTime? inicioCalada;

DateTime? inicioSoplido;

Duration duracionCaladaActual = Duration.zero;

Duration duracionSoplidoActual = Duration.zero;

StreamSubscription<Uint8List>? streamDatos;

Stream<String>? _dataStream;

void leerDatos() async {

// Verifica si el streamDatos ya está activo antes de escuchar nuevamente

if (streamDatos != null) return; // No crear un nuevo listener si ya existe

print("Antes del delay");

// await Future.delayed(Duration(milliseconds: 2000));

print("Despue´s del delay");

_dataStream = conexionBl.input!

.map((data) => String.fromCharCodes(data))

.asBroadcastStream();

_dataStream!.listen((data) {

// String dataStr = String.fromCharCodes(data);

String dataStr = data;

buffer += dataStr;

int startIndex = buffer.indexOf("INICIO");

int endIndex = buffer.indexOf("FIN");

if (startIndex != -1 && endIndex != -1 && endIndex > startIndex) {

String packet = buffer.substring(startIndex + 6, endIndex);

buffer = buffer.substring(endIndex + 3);

datosParseados = packet.split(";");

if (datosParseados.length == 4) {

// _dataStreamController.add(splitData); //En vez de enviarlo a un stream para pintarlos, llamo a notifyListeners

actualizarValores();

notifyListeners();

}

}

});

}

void actualizarValores() {

//Actualizar las variables del controlador (lo que será el estado)

temperatura = double.tryParse(datosParseados[0]) ?? 0.0;

humedad = double.tryParse(datosParseados[1]) ?? 0.0;

presion = double.tryParse(datosParseados[2]) ?? 0.0;

idMensaje = datosParseados[3];

//Si es la primera vez que inicio, seteo la presión inicial

if (presionInicial == 0) {

presionInicial = presion;

print('Presión inicial establecida en: $presionInicial');

}

//Actualizao el contador de caladas

actualizarContador(presion);

notifyListeners();

}

void actualizarContador(double presion) {

const double UMBRAL = 0.9; // Ajustar según la sensibilidad requerida

DateTime ahora = DateTime.now();

// Detectar inicio y fin de calada

// print('if $presion < $presionInicial - $UMBRAL && !$enCalada');

if (presion < presionInicial - UMBRAL && !enCalada) {

enCalada = true;

inicioCalada = ahora;

}

if (presion >= presionInicial - UMBRAL && enCalada) {

caladas++;

duracionCaladaActual = ahora.difference(inicioCalada!);

enCalada = false;

}

// Detectar inicio y fin de soplido

if (presion > presionInicial + UMBRAL && !enSoplido) {

enSoplido = true;

inicioSoplido = ahora;

}

if (presion <= presionInicial + UMBRAL && enSoplido) {

purgados++;

duracionSoplidoActual = ahora.difference(inicioSoplido!);

enSoplido = false;

}

}

Future<void> pararLectura() async {

// conexionBl.finish();

if (streamDatos != null) {

await streamDatos!.cancel();

streamDatos = null;

}

}

// @override

// void dispose() async {

// if (streamDatos != null) {

// await streamDatos!.cancel();

// }

// super.dispose();

// }

}