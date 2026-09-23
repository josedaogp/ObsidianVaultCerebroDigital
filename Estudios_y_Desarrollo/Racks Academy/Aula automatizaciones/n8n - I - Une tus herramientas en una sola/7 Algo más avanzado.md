## Descripción del Video

  
En este video, se abordan configuraciones avanzadas y la resolución de errores en n8n, detallando el proceso de automatización de contactos y mensajes mediante Google y Telegram. Los puntos principales incluyen:

- **Configuración de Credenciales**:
    - Resolución de errores por falta de credenciales configuradas.
    - Detalles sobre cómo configurar credenciales de Google y Telegram.
    - Uso de la documentación de Google para configurar OAuth 2.0 y API de Google Sheets.
- **Automatización con Google Sheets**:
    - Extracción de contactos y mensajes predefinidos desde Google Sheets.
    - Configuración de filtros y tratamiento de datos dinámicos.
    - Uso de nodos de código para procesar y filtrar la información.
- **Implementación de Flujos Complejos**:
    - Generación de un flujo que se ejecuta cada dos días con tiempos aleatorios.
    - Uso de nodos de bucles e iteraciones para procesar listas de contactos y mensajes.
    - Comprobación y actualización de datos en Google Sheets mediante nodos condicionales.
- **Envío de Correos y Mensajes**:
    - Configuración de nodos de Gmail para enviar correos automáticos.
    - Implementación de nodos de Telegram para enviar notificaciones.
    - Ejemplos prácticos de cómo referenciar y utilizar datos dentro de los flujos.
- **Consejos Prácticos y Buenas Prácticas**:
    - Importancia de validar correos y evitar duplicados.
    - Utilización de formato condicional en Google Sheets para facilitar la gestión de datos.
    - Recomendaciones para la búsqueda y uso de documentación y recursos en línea.

Este video proporciona una guía detallada y práctica para implementar flujos avanzados de automatización utilizando n8n, Google Sheets, Gmail y Telegram, ayudando a los usuarios a maximizar la eficiencia y personalización de sus tareas automatizadas.

---

#### Plantillas

No os abruméis si no os veis capaces de hacer un flujo de trabajo tan complicado. Para eso están las plantillas que la comunidad de n8n tiene disponibles  [aquí](https://n8n.io/workflows/).

---

La fórmula que he usado en google sheet seria la siguiente

=COUNTIF($D$2:$D$100; D2)>1 (si tenéis la interfaz en ingles)

=CONTAR.SI($D$2:$D$100; D2)>1 (si está en español)

---

#### Credenciales de Google

####   
[![](https://media1-production-mightynetworks.imgix.net/asset/0b56267b-8814-452c-b3b5-8f48406d23ab/1719743336124.png?ixlib=rails-4.2.0&fm=jpg&q=75&auto=format&w=1400&h=1400&fit=max&impolicy=ResizeCrop&constraint=downsize&aspect=fit)](https://media1-production-mightynetworks.imgix.net/asset/0b56267b-8814-452c-b3b5-8f48406d23ab/1719743336124.png?ixlib=rails-4.2.0&fm=jpg&q=75&auto=format)

Tenemos que crear una credencial de google en n8n.

Luego nos vamos a [Google Cloud API Credentials](https://console.cloud.google.com/apis/credentials)

Nos creamos una cuenta si no la tenemos creada y abrimos una credencial nueva de tipo OAuth clientID  
[![](https://media1-production-mightynetworks.imgix.net/asset/9aec109e-59a7-4a92-8772-37b9c4500454/1719743576562.png?ixlib=rails-4.2.0&fm=jpg&q=75&auto=format&w=1400&h=1400&fit=max&impolicy=ResizeCrop&constraint=downsize&aspect=fit)](https://media1-production-mightynetworks.imgix.net/asset/9aec109e-59a7-4a92-8772-37b9c4500454/1719743576562.png?ixlib=rails-4.2.0&fm=jpg&q=75&auto=format)

Después seleccionamos en Application type (web application), ponemos un nombre y en Authorized redirect URIs tenemos que poner...

[![](https://media1-production-mightynetworks.imgix.net/asset/7c795885-85c8-451c-87ce-2956f3238bd2/1719743695837.png?ixlib=rails-4.2.0&fm=jpg&q=75&auto=format&w=1400&h=1400&fit=max&impolicy=ResizeCrop&constraint=downsize&aspect=fit)](https://media1-production-mightynetworks.imgix.net/asset/7c795885-85c8-451c-87ce-2956f3238bd2/1719743695837.png?ixlib=rails-4.2.0&fm=jpg&q=75&auto=format)

...la que nos muestra n8n como OAuth Redirect URL

[![](https://media1-production-mightynetworks.imgix.net/asset/5be1b4e5-9412-4ac0-98f3-925d5898f0dc/1719744171071.png?ixlib=rails-4.2.0&fm=jpg&q=75&auto=format&w=1400&h=1400&fit=max&impolicy=ResizeCrop&constraint=downsize&aspect=fit)](https://media1-production-mightynetworks.imgix.net/asset/5be1b4e5-9412-4ac0-98f3-925d5898f0dc/1719744171071.png?ixlib=rails-4.2.0&fm=jpg&q=75&auto=format)

Una vez hecho esto volvemos a nuestro panel de credenciales, pinchamos sobre la que hemos creado y copiamos el Client ID y el Client Secret y lo ponemos en la ventana anterior de n8n. Después verificamos el acceso con nuestra cuenta y listo.[![](https://media1-production-mightynetworks.imgix.net/asset/9f00279e-7041-4773-849b-c19216cf29ad/1719744242114.png?ixlib=rails-4.2.0&fm=jpg&q=75&auto=format&w=1400&h=1400&fit=max&impolicy=ResizeCrop&constraint=downsize&aspect=fit)](https://media1-production-mightynetworks.imgix.net/asset/9f00279e-7041-4773-849b-c19216cf29ad/1719744242114.png?ixlib=rails-4.2.0&fm=jpg&q=75&auto=format)

Cuando hayas creado la aplicación ya tienes las credenciales. En la clase os muestro que hay que darle acceso a Sheets a esa aplicación que hemos creado de google.

Esto se hace desde [aquí](https://console.cloud.google.com/apis/library/sheets.googleapis.com) dandole al botón “enable”  
 [![](https://media1-production-mightynetworks.imgix.net/asset/e4ca0c04-4019-4f8f-8ccd-4a60470f39ea/1720956142219.png?ixlib=rails-4.2.0&fm=jpg&q=75&auto=format&w=1400&h=1400&fit=max&impolicy=ResizeCrop&constraint=downsize&aspect=fit)](https://media1-production-mightynetworks.imgix.net/asset/e4ca0c04-4019-4f8f-8ccd-4a60470f39ea/1720956142219.png?ixlib=rails-4.2.0&fm=jpg&q=75&auto=format)

---

Transcripción del vídeo:

[algo mas avanzado.zip](https://media2-production.mightynetworks.com/asset/a28cfa8f-a045-4d13-8ca4-643bbb5b4fe7/algo_mas_avanzado.zip "algo mas avanzado.zip")

## Mis notas
Explica cómo meter las credenciales de google y explica más en profundidad el ejemplo de la clase anterior de cómo enviar correos de recordatorio a los clientes.