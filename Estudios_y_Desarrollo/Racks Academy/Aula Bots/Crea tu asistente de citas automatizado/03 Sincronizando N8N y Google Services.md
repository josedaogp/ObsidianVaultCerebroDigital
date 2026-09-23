En este video, aprenderás a configurar las credenciales de Google para acceder correctamente al módulo de calendario de tu aplicación. A través de un proceso detallado, se te guiará desde la creación de una cuenta en Google Cloud hasta la habilitación de las APIs necesarias, asegurando que minimices errores y comprendas cada paso del proceso. Además, se abordarán aspectos importantes como la configuración de la pantalla de consentimiento y la gestión de permisos, para que puedas integrar de manera efectiva los servicios de Google en tu proyecto.

Lista de herramientas mencionadas:

- Google Cloud
- Google Calendar API
- Google Drive API
- Google Sheets API
- [**Github**](https://github.com/leifermendez/bot-google-n8n-calendar)
- **[Railway Plantilla](https://railway.app/template/ApzZZF?referralCode=jyd_0y)**
- **[Chat PDF API](https://www.chatpdf.com/)**
- **[Hoppscotch](https://hoppscotch.io/)** 

Permisos:

[https://www.googleapis.com/auth/calendar](https://www.googleapis.com/auth/calendar)  
[https://www.googleapis.com/auth/calendar.readonly](https://www.googleapis.com/auth/calendar.readonly)  
[https://www.googleapis.com/auth/calendar.events.readonly](https://www.googleapis.com/auth/calendar.events.readonly)  
[https://www.googleapis.com/auth/spreadsheets](https://www.googleapis.com/auth/spreadsheets)  
[https://www.googleapis.com/auth/spreadsheets.readonly](https://www.googleapis.com/auth/spreadsheets.readonly)  
[https://www.googleapis.com/auth/drive.file](https://www.googleapis.com/auth/drive.file)

Topics del video

- Configuración de credenciales de Google
- Creación de una cuenta en Google Cloud
- Habilitación de APIs
- Configuración de la pantalla de consentimiento
- Gestión de permisos de usuario
- Integración de servicios de Google en aplicaciones

[cap_3.zip](https://media2-production.mightynetworks.com/asset/e80fcfb7-8fb6-4288-9ddd-c253b56e28a7/cap_3.zip "cap_3.zip")

## Leifer explica cómo integrar los sistemas de google con n8n
1. Google Cloud console
2. Menu desplegable. Soluciones-Todos Los productos - Apis y servicios - pantalla de consentimiento
3. Aplicación externa, crear. 
4. Poner nombre y correo. Logotipo no es necesario.
5. Dominio railway.app
6. guardar y continuar
7. Pedirá permisos
8. Agregar o quitar permisos. Pegar los permisos que están arriba
9. Pide los test users, usuarios de prueba
10. Poner las cuentas de gmail de los calendarios a los que queramos acceder.
11. listo
12. Ir a credenciales-id oauth
13. Decirle tipo aplicación web
14. Copiar url de raillway y ponerlo en URL
15. Va a n8n.
16. Crea un workflow con google. Al crear credencial en el nodo de google te da la oauth redirect URL. IMPORTANTE QUE SEA HTTPS. La copiamos
17. Volvemos a google
18. Pegamos la url en uri de redireccionamiento
19. Crear
20. Guardar el id del cliente y el secreto del cliente
21. Esos dos serán los que pongamos en n8n
22. asegurarnos que el usuario con el que iniciemos sesión en n8n sea el que pusimos antes en test users en google cloud.
23. por último, en APIs y servicios habilitados, habilitar calendar, drive y sheets
24. 