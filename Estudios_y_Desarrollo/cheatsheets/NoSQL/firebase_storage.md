# ☁️ Cheatsheet de Firebase Storage

Este cheatsheet cubre los aspectos esenciales de Firebase Storage, el servicio de almacenamiento en la nube de Firebase para archivos como imágenes, videos, audio, etc.

## 🔍 Conceptos Básicos

*   **Bucket:** 🗄️ Contenedor de nivel superior para tus archivos en Firebase Storage (similar a un directorio raíz).  Por defecto, tu proyecto Firebase tiene un bucket.
*   **Objeto (Object):** 📄 Archivo individual almacenado en Firebase Storage (ej. una imagen, un video, un documento).
*   **Referencia (Reference):** 📍 Puntero a un archivo o directorio en Firebase Storage.  Se usa para realizar operaciones (subir, descargar, eliminar, etc.).
*   **Metadatos (Metadata):** ℹ️ Información sobre un archivo (nombre, tipo MIME, tamaño, fecha de creación, etc.).  Puedes definir metadatos personalizados.
*   **Reglas de Seguridad (Security Rules):** 🛡️ Definen quién tiene acceso de lectura y escritura a tus archivos.  Son *fundamentales* para proteger tus datos.
*   **URL de Descarga (Download URL):** 🔗 URL pública que se puede usar para acceder a un archivo (si las reglas de seguridad lo permiten).

## 📦 Configuración Inicial (Web - JavaScript)

1.  **Agregar Firebase a tu proyecto web:**

    ```html
    <!-- Firebase App (siempre requerido) -->
    <script src="https://www.gstatic.com/firebasejs/9.x.x/firebase-app-compat.js"></script>

    <!-- Firebase Storage -->
    <script src="https://www.gstatic.com/firebasejs/9.x.x/firebase-storage-compat.js"></script>

    <script>
      // Configuración de Firebase (obtenida desde la consola de Firebase)
      const firebaseConfig = {
        apiKey: "TU_API_KEY",
        authDomain: "tu-proyecto.firebaseapp.com",
        projectId: "tu-proyecto",
        storageBucket: "tu-proyecto.appspot.com", // Bucket por defecto
        messagingSenderId: "TU_SENDER_ID",
        appId: "TU_APP_ID"
      };

      // Inicializar Firebase
      firebase.initializeApp(firebaseConfig);

      // Obtener una referencia al servicio de Storage
      const storage = firebase.storage();
    </script>
    ```

    *   Reemplaza `9.x.x` con la versión actual del SDK de Firebase.
    *   Obtén la configuración de Firebase ( `firebaseConfig` ) desde la consola de Firebase (Project settings > General > Your apps > Config).

## 📍 Referencias

*   Una referencia es un puntero a un archivo o directorio en Firebase Storage.

```javascript
// Referencia a la raíz del bucket
const storageRef = storage.ref();

// Referencia a un archivo específico
const imagenRef = storage.ref('images/mi_imagen.jpg'); // Ruta relativa al bucket

// Referencia a un directorio
const carpetaRef = storage.ref('images');

// Referencia a un archivo usando child()
const archivoRef = storageRef.child('images/otra_imagen.png');

// Referencia al padre
const padreRef = archivoRef.parent;

// Referencia a la raíz desde cualquier referencia
const raizRef = archivoRef.root;
```

## ⬆️ Subida de Archivos (Upload)

1.  **Desde un `Blob`, `File`, o `Uint8Array` (común en el navegador):**

    ```javascript
    const file = document.getElementById('miArchivo').files[0]; // Obtener el archivo desde un input
    const storageRef = storage.ref('images/' + file.name);

    // Subir el archivo
    const uploadTask = storageRef.put(file);

    // Monitorear el progreso (opcional)
    uploadTask.on('state_changed',
      (snapshot) => {
        // Progreso
        const progress = (snapshot.bytesTransferred / snapshot.totalBytes) * 100;
        console.log('Upload is ' + progress + '% done');
      },
      (error) => {
        // Manejar errores
        console.error("Error al subir:", error);
      },
      () => {
        // Subida completada
        uploadTask.snapshot.ref.getDownloadURL().then((downloadURL) => {
          console.log('File available at', downloadURL);
        });
      }
    );
    ```

2.  **Desde una cadena ( `String` - `data_url`, `base64`, `raw`):**

    ```javascript
    const storageRef = storage.ref('texto/mi_archivo.txt');
    const message = 'Este es mi archivo de texto.';

    storageRef.putString(message, 'raw').then((snapshot) => { //Formato raw
        console.log('Uploaded a raw string!');
    });

    const base64String = '...'; // Tu cadena base64
    storageRef.putString(base64String, 'base64').then((snapshot) => { /* ... */ });

    const dataUrlString = 'data:text/plain;base64,...';
    storageRef.putString(dataUrlString, 'data_url').then((snapshot) => { /* ... */ });
    ```

3. **Subir metadatos al mismo tiempo**

    ```javascript
        const metadata = {
            contentType: 'image/jpeg',
            customMetadata: { // Metadatos personalizados
                autor: 'Juan',
                fecha: '2024-01-26'
            }
        };

        uploadTask = storageRef.put(file, metadata);
    ```
4. **Control de la subida (pausar, reanudar, cancelar)**
    ```javascript
        uploadTask.pause(); // Pausar
        uploadTask.resume(); // Reanudar
        uploadTask.cancel(); // Cancelar
    ```

## ⬇️ Descarga de Archivos (Download)

1.  **Obtener la URL de descarga (recomendado):**

    ```javascript
    const storageRef = storage.ref('images/mi_imagen.jpg');

    storageRef.getDownloadURL().then((url) => {
      // Usar la URL para mostrar la imagen, descargarla, etc.
      console.log('URL de descarga:', url);
      const img = document.getElementById('miImagen');
      img.src = url;
    }).catch((error) => {
      // Manejar errores
      console.error("Error al obtener la URL:", error);
    });
    ```

2.  **Descargar directamente a un `Blob` (menos común):**

    ```javascript
    storageRef.getBlob().then((blob) => {
      // Crear un objeto URL para el Blob
      const url = URL.createObjectURL(blob);
      // ... usar la URL ...
      URL.revokeObjectURL(url); // Liberar la URL cuando ya no se necesite
    }).catch((error) => { /* ... */ });
    ```
3.  **Descargar a un array de bytes:**

    ```javascript
        storageRef.getBytes(1024 * 1024).then((data) => { // 1MB maximo
            // data es un Uint8Array
        }).catch((error) => {/* ... */});
    ```

## 🗑️ Eliminación de Archivos

```javascript
const storageRef = storage.ref('images/mi_imagen.jpg');

storageRef.delete().then(() => {
  // Archivo eliminado
  console.log("Archivo eliminado.");
}).catch((error) => {
  // Manejar errores
  console.error("Error al eliminar:", error);
});
```

## ℹ️ Gestión de Metadatos

1.  **Obtener metadatos:**

    ```javascript
    storageRef.getMetadata().then((metadata) => {
      console.log('Tipo MIME:', metadata.contentType);
      console.log('Tamaño:', metadata.size);
      console.log('Metadatos personalizados:', metadata.customMetadata);
    }).catch((error) => { /* ... */ });
    ```

2.  **Actualizar metadatos:**

    ```javascript
    const newMetadata = {
      contentType: 'image/png',  // Actualizar el tipo MIME
      customMetadata: {
        comentario: 'Nueva descripción'
      }
    };

    storageRef.updateMetadata(newMetadata).then((metadata) => {
      console.log('Metadatos actualizados:', metadata);
    }).catch((error) => { /* ... */ });
    ```

## 📃 Listar Archivos

```javascript
const storageRef = storage.ref('images');

// Listar todos los archivos
storageRef.listAll().then((result) => {
  result.items.forEach((itemRef) => {
    // itemRef es una referencia a un archivo
    console.log('Archivo:', itemRef.fullPath);
  });

    result.prefixes.forEach((prefixRef)=>{
        // prefixRef son las carpetas
    })
}).catch((error) => { /* ... */ });

// Listar con paginación
storageRef.list({ maxResults: 10, pageToken: null }).then((result) => {
    // result.items son los archivos
    // result.prefixes son las carpetas
    // result.pageToken es el token para la siguiente página
});
```

## 🛡️ Reglas de Seguridad (Security Rules)

*   *Fundamentales* para proteger tus archivos.
*   Se definen en la consola de Firebase (Storage > Rules).
*   Usan un lenguaje propio de Firebase.

**Ejemplo:**

```firebase
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /images/{imageId} {
      // Permitir lectura si el archivo es una imagen
      allow read: if resource.contentType.matches('image/.*');

      // Permitir escritura solo a usuarios autenticados
      allow write: if request.auth != null;

        // Permitir escritura si el archivo es menor de 5MB
        allow write: if request.resource.size < 5 * 1024 * 1024;
    }

     match /documents/{documentId} {
        // Solo el propietario puede leer/escribir
        allow read, write: if request.auth.uid == resource.metadata.uid;
     }

      match /{document=**} { //Regla por defecto
        allow read, write: if false; // Denegar todo
      }
  }
}
```

*   `request`:  Información sobre la petición (ej. `request.auth`, `request.resource`).
*   `resource`:  Información sobre el archivo existente (ej. `resource.contentType`, `resource.size`, `resource.metadata`).
*  `request.auth.uid`: ID del usuario autenticado
*  `get()` y `exists()`: Para acceder a otros archivos.

## 🌐 Uso en Otras Plataformas (Node.js, Flutter, etc.)

*   **Node.js (Admin SDK):**

    ```javascript
    const admin = require('firebase-admin');

    const serviceAccount = require('./ruta/a/tu/credencial.json');

    admin.initializeApp({
      credential: admin.credential.cert(serviceAccount),
      storageBucket: 'tu-proyecto.appspot.com'
    });

    const bucket = admin.storage().bucket();

    // Subir un archivo
    bucket.upload('./mi_archivo.jpg', {
      destination: 'images/mi_archivo.jpg',
      metadata: { /* ... */ }
    }).then((file) => { /* ... */ });

    //Obtener la URL firmada
    const options = {
        version: 'v4',
        action: 'read',
        expires: Date.now() + 15 * 60 * 1000, // 15 minutes
      };

      const [url] = await file.getSignedUrl(options); //Para un archivo que ya existe
    ```

*   **Flutter:**

    ```dart
    import 'package:firebase_storage/firebase_storage.dart';

    // Obtener una referencia
    final storageRef = FirebaseStorage.instance.ref();
    final imagenRef = storageRef.child('images/mi_imagen.jpg');

    // Subir un archivo
    final file = File('ruta/a/mi_archivo.jpg');
    final uploadTask = imagenRef.putFile(file);
    // o putData para bytes

    // Obtener la URL de descarga
    final downloadURL = await (await uploadTask).ref.getDownloadURL();
    ```

*   **Otros lenguajes (Python, Java, Go, C++, Unity, etc.):**  Firebase proporciona SDKs para muchas plataformas.  La lógica es similar.

Este cheatsheet cubre los aspectos más importantes de Firebase Storage.  ¡Recuerda consultar la documentación oficial de Firebase para más detalles!