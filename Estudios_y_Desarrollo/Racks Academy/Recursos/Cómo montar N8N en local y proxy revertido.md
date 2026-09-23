https://sabio-moderno.notion.site/Modo-DOCKER-e924bfabc57c4902b4655af584846750
### Modo DOCKER

### Requisitos

- Instalar Docker: [](https://www.docker.com/)[https://www.docker.com](https://www.docker.com)
- DuckDNS - DNS gratuito [https://www.duckdns.org/](https://www.duckdns.org/)
- Nginx Proxy Manager - Proxy inverso [https://nginxproxymanager.com/](https://nginxproxymanager.com/)
- Tener acceso a tu Router. Credenciales suelen estar también debajo del dispositivo.
- Tú editor favorito, preferiblemente CURSOR. [https://www.cursor.com/](https://www.cursor.com/)

### Procedimiento

1. Descarga e instala Docker desde la web oficial.
    
2. Crea una carpeta donde más te guste. La vas a utilizar para guardar todo el contenido de la instalación del contenedor. Ej: _C:\Users\PerroSanxe\Downloads\Contenedor_n8n_
    
3. Vamos a [https://www.duckdns.org/](https://www.duckdns.org/) te registras y creas un dominio que es el que utilizarás para acceder al proxy. Te dará el nombre de dominio y un token. ❗ **Quédate con eso.**
    
4. Ahora necesitamos crear un fichero “docker-compose.yml”. Para crearlo lo mejor es hacerlo con el editor o IDE Cursor. Lo instalas y abres con la carpeta que creaste antes para partir desde ahí. Creas el fichero, arriba en File, o click derecho en el menú o en el botón del menú arriba para crear el fichero, le das el nombre exacto: docker-compose.yml
    
5. En el fichero has de tener la siguiente información:
    
    [docker-compose.yml](https://www.notion.so/docker-compose-yml-5f7224f84bb94998ad4ddf4aaa0ef950?pvs=21)
    
6. Desde **CURSOR** podemos acceder a la terminal, está escondida pero tú haces la combinación de atajo **Ctrl + ñ** y te sale directamente. Y ejecutas el siguiente comando:
    
    ```docker
    docker-compose up -d
    ```
    
    Y empezará a crear el contenedor con sus respectivas imágenes (n8n y nginx)
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/05379b4a-6623-497a-94c7-919dd9a97f2d/3e2c8cfc-ecfb-46e1-a427-53dd8cb93131/Untitled.png)
    
7. Si todo ha salido bien podrás tener acceso a n8n y al Proxy Server accediendo a la direcciones: n8n → [http://localhost:5678](http://localhost:5678) (Comprueba que el webhook (como hacías en Railway) tengas el enlace comenzando por https://)
    
    Proxy → [http://localhost:81](http://localhost:81) Y este último saldrá el login de Nginx Proxy Manager. Las credenciales son: Usuario: [admin@example.com](mailto:admin@example.com) Pass: changeme Te dirá que te registres y cambies la contraseña.
    
8. Desde Nginx Proxy Server vas arriba a SSL Certificates. Y añades un nuevo certificado y has de rellenar el formulario tal que así:
    
    - Imagen ejemplo:
        
        ⚠️ En **Credentials File Content** TIENES QUE pegar el token de DuckDNS que conseguiste anteriormente. Ejemplo: dns_duckdns_token=[tu_token]
        
        ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/05379b4a-6623-497a-94c7-919dd9a97f2d/843d2fb0-e837-4149-a66f-ce23bbc362ab/Untitled.png)
        
    
    Si te da error, añade en **Propagation Seconds**: 120 y tratas de Guardar de nuevo. Tardará un poco más pero ha de darte resultado final.
    
9. Ahora te diriges a Hosts de la barra de navegación superior otra vez y pulsas Proxy Hosts. Pulsas añadir Host y rellenas tal que así:
    
    - Pestaña Details:
        
        ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/05379b4a-6623-497a-94c7-919dd9a97f2d/c8752157-1f0d-462a-ba6d-64097e3c390a/Untitled.png)
        
    - Pestaña SSL:
        
        Te saldrá un desplegable con el certificado SSL, simplemente seleccionas y activas esas 2 opciones y Guardar.
        
        ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/05379b4a-6623-497a-94c7-919dd9a97f2d/840e155d-a8f6-456c-9f79-c951815e5b12/Untitled.png)
        
10. Ahora haces lo mismo, añades otro host pero añadirás el n8n delante del dominio y rellenas tal que así:
    
    - Host n8n
        
        Y la pestaña SSL exactamente igual que en el caso anterior.
        
        ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/05379b4a-6623-497a-94c7-919dd9a97f2d/b6cade12-a9ed-4a4b-aecd-493b2acfafe0/Untitled.png)
        
11. Toca configurar el router, tienes que entrar a la configuración del mismo que normalmente es accediendo a la dirección: 192.168.1.1 y te pedirá unas credenciales que depende mucho del router, el modelo, y la compañía proveedora de internet.
    

En mi caso me salía la contraseña debajo del router al igual que la del WiFi. 12. Toca abrir puertos (Varía según el modelo) pero es algo que va relacionado con Puertos así que puede que veais opciones en el menú como:

- Puertos
    
- Reenvío de puertos
    
- NAT
    
- Port Fowarding Y cuando lo encuentres, añades 2 reglas, una por una, que simplemente ha de tener esta información: 🚧 **INCISO:** La dirección IP que tienes que indicar es la de tu PC. **La IP PRIVADA.** 👇 Cómo saber mi IP privada? 👇
    
    [Saber mi IP privada](https://www.notion.so/Saber-mi-IP-privada-2b34adac240a4050b4228250928599f6?pvs=21)
    
    🚧
    
- Ejemplo 1
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/05379b4a-6623-497a-94c7-919dd9a97f2d/b308296b-943e-4486-a0b6-cfa56e8721d7/Untitled.png)
    
- Ejemplo 2
    
    ![Ports.PNG](https://prod-files-secure.s3.us-west-2.amazonaws.com/05379b4a-6623-497a-94c7-919dd9a97f2d/78a14938-37a0-415c-baea-7aafab2bfac5/Ports.png)
    

Guardas y queda ya configurado.

1. Si todo lo anterior ha salido bien ya puedes acceder a:

[tudominio.duckdns.org](http://tudominio.duckdns.org) y [n8n.tudominio.duckdns.org](http://n8n.tudominio.duckdns.org) desde el navegador y te saldrá nginx (proxy inverso) y n8n respectivamente sin tener que indicar [localhost:81](http://localhost:81) y [localhost:5678](http://localhost:5678)

### Ya puedes usar n8n en modo local y con Docker 😎

## CHECK ✅

### Bibliografía:

[Quick and Easy Local SSL Certificates for Your Homelab!](https://youtu.be/qlcVx-k-02E?si=YsOBFzS31WmFWCDj)

[https://notthebe.ee/blog/easy-ssl-in-homelab-dns01/](https://notthebe.ee/blog/easy-ssl-in-homelab-dns01/)