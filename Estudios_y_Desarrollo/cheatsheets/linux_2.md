# 🐧 Cheatsheet de Linux: Tu Guía Rápida de Comandos 🚀

## 🧭 **Navegación**

*   `pwd` 🖨️ (Print Working Directory): Muestra el directorio actual.
*   `cd` 📂 (Change Directory): Cambia de directorio.
    *   `cd <directorio>`:  Va al directorio especificado (ruta absoluta o relativa).
    *   `cd ..`: Sube un nivel (al directorio padre).
    *   `cd ~`: Va al directorio home del usuario actual.
    *   `cd -`: Va al directorio anterior.
    *   `cd`: Sin argumentos, va al directorio home.
*   `ls` 📑 (List): Lista los archivos y directorios en el directorio actual.
    *   `ls -l`: Formato largo (permisos, propietario, tamaño, fecha, etc.).
    *   `ls -a`: Muestra *todos* los archivos y directorios, incluyendo los ocultos (los que empiezan con `.`).
    *   `ls -h`: Muestra los tamaños en formato "humano" (K, M, G).
    *   `ls -t`: Ordena por fecha de modificación (más reciente primero).
    *   `ls -r`: Invierte el orden de clasificación.
    *   `ls -lh`: Combinación común: formato largo y tamaños "humanos".
    *   `ls -la`: Combinación común: formato largo y muestra todos los archivos.
    *   `ls <directorio>`:  Lista el contenido de un directorio específico (sin cambiar al directorio).

## 📁 **Manipulación de Archivos y Directorios**

*   `touch` ✋: Crea un archivo vacío o actualiza la fecha de modificación de un archivo existente.  `touch <archivo>`
*   `mkdir` 🔨 (Make Directory): Crea un directorio.  `mkdir <nombre_directorio>`
    *  `mkdir -p <ruta/con/subdirectorios>`:  Crea directorios anidados; crea los directorios intermedios si no existen.
*   `cp` 📝 (Copy): Copia archivos y directorios.
    *   `cp <origen> <destino>`:  Copia el archivo/directorio `<origen>` a `<destino>`.
    *   `cp -r <directorio_origen> <directorio_destino>`: Copia un directorio de forma *recursiva* (incluyendo todos sus subdirectorios y archivos).
    *   `cp -i <origen> <destino>`:  Pregunta antes de sobrescribir archivos existentes.
    *   `cp -u <origen> <destino>`: Copia solo si el origen es más nuevo que el destino, o si el destino no existe.
*   `mv` 🚚 (Move): Mueve o renombra archivos y directorios.
    *   `mv <origen> <destino>`:  Mueve o renombra.
    *   `mv -i <origen> <destino>`: Pregunta antes de sobrescribir.
*   `rm` 🗑️ (Remove): Elimina archivos y directorios.  ¡Ten cuidado!  No hay papelera de reciclaje por defecto.
    *   `rm <archivo>`: Elimina un archivo.
    *   `rm -r <directorio>`: Elimina un directorio de forma recursiva (¡muy peligroso!).
    *   `rm -f <archivo>`: Fuerza la eliminación (no pregunta, no muestra errores si el archivo no existe).
    *   `rm -rf <directorio>`:  Elimina un directorio de forma recursiva y forzada (¡extremadamente peligroso!).
    *   `rm -i <archivo>`:  Pregunta antes de eliminar cada archivo.  Útil como precaución.
*   `ln` 🔗 (Link): Crea enlaces (accesos directos).
    *   `ln -s <objetivo> <enlace>`: Crea un *enlace simbólico* (similar a un acceso directo en Windows).  El `<enlace>` apunta a `<objetivo>`.  Si se borra el `<objetivo>`, el enlace queda roto.
    *   `ln <objetivo> <enlace>`:  Crea un *enlace duro*.  El `<enlace>` es otra entrada en el sistema de archivos que apunta a los mismos datos que `<objetivo>`.  Si se borra el `<objetivo>`, el `<enlace>` sigue funcionando (porque apunta directamente a los datos).
* `find` 🔍: Busca archivos y directorios.  Comando muy potente.
    * `find <directorio_inicio> -name "<patron>"`: Busca archivos/directorios que coincidan con un patrón (usando comodines).  Ej: `find . -name "*.txt"` (busca archivos .txt en el directorio actual y subdirectorios).
     * `find <directorio> -type f`: Encuentra solo archivos.
     * `find <directorio> -type d`: Encuentra solo directorios.
     * `find <directorio> -mtime -7`: Encuentra archivos modificados en los últimos 7 días
      * `find <directorio> -size +10M`: Encuentra archivos mayores a 10 megabytes.
    * `find <directorio> -name "*.txt" -exec rm {} \;`: Busca archivos .txt y los elimina (¡cuidado!).  `-exec` ejecuta un comando en cada resultado encontrado.  `{}` se reemplaza por el nombre del archivo encontrado, y `\;` termina el comando `-exec`.

*   `du` (disk usage): Calcula el uso de espacio en disco.
  *    `du -h <archivo o directorio>`:  Muestra el espacio ocupado en formato "humano".
  *    `du -sh <directorio>`:  Muestra el tamaño *total* del directorio.
  *   `du -ah`: muestra el tamaño de todos los archivos y carpetas.

*   `df` (disk free): Muestra el espacio libre en los sistemas de archivos montados.
 *    `df -h`:  Formato "humano".

## 📝 **Visualización y Edición de Texto**

*   `cat` 🐱: Muestra el contenido de un archivo en la terminal.  `cat <archivo>`
    *   `cat <archivo1> <archivo2> > <archivo3>`:  Concatena varios archivos y guarda el resultado en un nuevo archivo.
*   `less` 📄 (o `more`):  Visualiza el contenido de un archivo página por página (permite desplazarse).  Presiona `q` para salir.  `less <archivo>`
*   `head` 🤕: Muestra las primeras líneas de un archivo (por defecto, 10).  `head <archivo>`
    *   `head -n 20 <archivo>`: Muestra las primeras 20 líneas.
*   `tail` 🐒: Muestra las últimas líneas de un archivo (por defecto, 10).  `tail <archivo>`
    *   `tail -n 5 <archivo>`: Muestra las últimas 5 líneas.
    *   `tail -f <archivo>`: Muestra las últimas líneas y *sigue mostrando* las nuevas líneas a medida que se añaden al archivo (muy útil para ver logs en tiempo real).
*   `grep` 🔎 (Global Regular Expression Print): Busca líneas que coincidan con un patrón.
    *   `grep "<patron>" <archivo>`: Busca el patrón en el archivo.
    *   `grep -i "<patron>" <archivo>`:  Búsqueda *insensible* a mayúsculas/minúsculas.
    *   `grep -v "<patron>" <archivo>`: Muestra las líneas que *no* coinciden con el patrón.
    *   `grep -r "<patron>" <directorio>`: Busca recursivamente en un directorio.
    *   `grep -c "<patron>" <archivo>`: Cuenta el número de líneas que coinciden.
    *   `grep -n "<patron>" <archivo>`: Muestra los números de línea junto con las líneas coincidentes.
    *   `<comando> | grep "<patron>"`:  Filtra la salida de otro comando (muy útil).
*   `wc` 🧮 (Word Count): Cuenta líneas, palabras y caracteres.  `wc <archivo>`
    *   `wc -l <archivo>`: Cuenta solo líneas.
    *   `wc -w <archivo>`: Cuenta solo palabras.
    *   `wc -c <archivo>`: Cuenta solo caracteres.

*   **Editores de texto:**
    *   `nano` ✏️: Editor de texto sencillo (fácil de usar para principiantes).  `nano <archivo>`
    *   `vim` (o `vi`) ⌨️: Editor de texto potente y muy configurable (curva de aprendizaje más pronunciada).  `vim <archivo>`
    *   `emacs` 📝: Otro editor de texto potente y extensible (también con su propia curva de aprendizaje).

## ⚙️ **Procesos**

*   `ps` 🕵️ (Process Status): Muestra información sobre los procesos en ejecución.
    *   `ps aux`: Muestra *todos* los procesos de *todos* los usuarios, con información detallada.  (Combinación muy común).
    *   `ps -ef`: Otra forma común de ver todos los procesos (formato ligeramente diferente).
*   `top` 📊: Muestra una vista dinámica de los procesos en ejecución (uso de CPU, memoria, etc.).  Presiona `q` para salir.
*   `htop` (si está instalado):  Versión mejorada de `top` (más interactiva, con colores, etc.).
*   `kill` 🔪: Envía una señal a un proceso (generalmente para terminarlo).
    *   `kill <PID>`: Envía la señal SIGTERM (terminación "amigable") al proceso con el ID `<PID>`.
    *   `kill -9 <PID>`:  Envía la señal SIGKILL (terminación forzada) al proceso con el ID `<PID>`.  Úsalo solo como último recurso.
*   `pkill`:  Mata procesos por nombre (o patrón).  `pkill <nombre_proceso>`
    *   `pkill -9 <nombre_proceso>`: Envía SIGKILL.
*   `jobs`: Muestra los trabajos en segundo plano en la shell actual.
*   `bg` (Background):  Envía un trabajo detenido al segundo plano.
*   `fg` (Foreground):  Trae un trabajo en segundo plano al primer plano.
*   `&`:  Ejecuta un comando en segundo plano.  Ej:  `sleep 10 &`
*  `nohup`: Ejecuta un comando que no se detendrá si cierras la terminal.  Ej: `nohup my_script.py &`

## 👤 **Usuarios y Permisos**

*   `whoami` 🙋: Muestra el nombre de usuario actual.
*   `sudo` (Superuser Do): Ejecuta un comando como superusuario (root) o como otro usuario.
*   `su` (Switch User): Cambia de usuario.
    *   `su -`: Cambia a root (y carga el entorno de root).  Te pedirá la contraseña de root.
    *   `su <nombre_usuario>`:  Cambia a otro usuario.  Te pedirá la contraseña del otro usuario.
*   `passwd` 🔑: Cambia la contraseña del usuario actual (o de otro usuario si eres root).
    *   `sudo passwd <usuario>`:  Cambia la contraseña de otro usuario (como root).
*   `useradd` ➕:  Crea un nuevo usuario.  `sudo useradd <nombre_usuario>`
*   `userdel` ➖:  Elimina un usuario.  `sudo userdel <nombre_usuario>`
    *   `sudo userdel -r <nombre_usuario>`: Elimina el usuario y su directorio home.
*   `usermod`:  Modifica un usuario existente.  `sudo usermod <opciones> <nombre_usuario>`
*   `groups`:  Muestra los grupos a los que pertenece el usuario actual.
*   `groupadd`:  Crea un nuevo grupo.  `sudo groupadd <nombre_grupo>`
*   `groupdel`:  Elimina un grupo.  `sudo groupdel <nombre_grupo>`
*   `chmod` (Change Mode): Cambia los permisos de un archivo o directorio.
    *   **Permisos:**
        *   `r` (read):  Lectura.
        *   `w` (write):  Escritura.
        *   `x` (execute):  Ejecución (o acceso, en el caso de directorios).
    *   **Usuarios:**
        *   `u` (user):  Propietario del archivo.
        *   `g` (group):  Grupo propietario del archivo.
        *   `o` (others):  Otros usuarios.
        *   `a` (all):  Todos los usuarios (u+g+o).
    *   **Forma numérica (más común):**
        *   `4`:  Permiso de lectura (r).
        *   `2`:  Permiso de escritura (w).
        *   `1`:  Permiso de ejecución (x).
        *   Se suman los valores para dar los permisos deseados.  Ej: `7` (4+2+1) = `rwx` (todos los permisos).
        *   Se usan tres números:  uno para el propietario, uno para el grupo y uno para otros.  Ej: `chmod 755 <archivo>` (el propietario tiene todos los permisos, el grupo y otros tienen permiso de lectura y ejecución).
    *   **Forma simbólica:**
        *   `chmod u+x <archivo>`:  Añade el permiso de ejecución (x) al *propietario* (u).
        *   `chmod g-w <archivo>`:  Quita el permiso de escritura (w) al *grupo* (g).
        *   `chmod o=r <archivo>`:  Establece el permiso de lectura (r) para *otros* (o), quitando cualquier otro permiso que pudieran tener.
        *   `chmod a+r <archivo>`: Añade permiso de lectura para *todos*
        *   `chmod ug+rwx,o-rwx`: añade todos los permisos a usuario y grupo, y quita todos los permisos a otros.
    * `chmod -R`:  Cambia los permisos de forma recursiva (para directorios y su contenido).
*   `chown` (Change Owner): Cambia el propietario de un archivo o directorio.  `sudo chown <nuevo_propietario> <archivo>`
    *  `chown <usuario>:<grupo> <archivo>`:  Cambia el propietario y el grupo.
    *   `chown -R`:  Cambia el propietario de forma recursiva.
*   `chgrp` (Change Group): Cambia el grupo propietario de un archivo o directorio.  `sudo chgrp <nuevo_grupo> <archivo>`

## 🌐 **Red**

*   `ping` 🏓: Envía paquetes ICMP a un host para comprobar la conectividad.  `ping <host>` (ej: `ping google.com`)
*   `ifconfig` (o `ip addr` en sistemas más modernos):  Muestra información sobre las interfaces de red (dirección IP, máscara de subred, etc.).
*   `netstat` (o `ss` en sistemas más modernos):  Muestra conexiones de red, tablas de enrutamiento, estadísticas de interfaces, etc.
    *   `netstat -tulnp`:  Muestra los puertos TCP y UDP que están escuchando (listening) y los programas que los usan.  (Combinación muy útil).
    * `ss -tulnp`: la versión moderna equivalente.
*   `ssh` (Secure Shell):  Conexión remota segura a otro sistema.  `ssh <usuario>@<host>`
*   `scp` (Secure Copy):  Copia archivos de forma segura entre sistemas.  `scp <origen> <usuario>@<host>:<destino>`
*   `wget`:  Descarga archivos desde la web. `wget <URL>`
*   `curl`:  Transfiere datos con URLs (puede hacer muchas más cosas que `wget`).  `curl <URL>`
* `hostname`: Muestra o establece el nombre del host
* `route`: Muestra o manipula la tabla de routing.

## 📦 **Gestión de Paquetes (¡Depende de la distribución!)**

*   **Debian/Ubuntu (y derivados):** `apt`
    *   `sudo apt update`:  Actualiza la lista de paquetes disponibles.  ¡Haz esto *antes* de instalar o actualizar!
    *   `sudo apt upgrade`:  Actualiza todos los paquetes instalados a sus últimas versiones.
    *   `sudo apt install <paquete>`:  Instala un paquete.
    *   `sudo apt remove <paquete>`:  Desinstala un paquete.
    *   `sudo apt purge <paquete>`:  Desinstala un paquete y sus archivos de configuración.
    *   `sudo apt autoremove`:  Elimina paquetes que ya no son necesarios (dependencias huérfanas).
    *   `apt search <paquete>`:  Busca un paquete.
*   **Red Hat/CentOS/Fedora (y derivados):** `yum` (más antiguo) o `dnf` (más moderno)
    *   `sudo yum update` / `sudo dnf update`: Actualiza todos los paquetes.
    *   `sudo yum install <paquete>` / `sudo dnf install <paquete>`: Instala un paquete.
    *   `sudo yum remove <paquete>` / `sudo dnf remove <paquete>`: Desinstala un paquete.
    *   `yum search <paquete>` / `dnf search <paquete>`: Busca un paquete.
*   **Arch Linux (y derivados):** `pacman`
    *   `sudo pacman -Syu`:  Actualiza *todo* el sistema (incluyendo la lista de paquetes).
    *   `sudo pacman -S <paquete>`:  Instala un paquete.
    *   `sudo pacman -R <paquete>`:  Elimina un paquete.
    *   `sudo pacman -Rs <paquete>`:  Elimina un paquete y sus dependencias que no son necesarias por otros paquetes.
    *   `pacman -Ss <paquete>`:  Busca un paquete.

## ℹ️ **Información del Sistema**

*   `uname` ℹ️: Muestra información sobre el sistema.
    *   `uname -a`:  Muestra toda la información (kernel, arquitectura, etc.).
*   `lsb_release -a`: Muestra información sobre la distribución de Linux (si está disponible).
*   `cat /etc/os-release` (o `/etc/*-release`):  Otra forma de obtener información sobre la distribución.
*   `free` 💾: Muestra la cantidad de memoria RAM libre y usada.
    *   `free -h`:  Formato "humano".
*   `uptime`:  Muestra cuánto tiempo ha estado funcionando el sistema.
*   `date`: Muestra o establece la fecha y hora del sistema.
* `cal`: Muestra el calendario
*   `who`: Muestra quién está conectado al sistema.
* `history`: Muestra el historial de comandos.
    * `!<número>`: Ejecuta el comando número `número` del historial.
    *  `!!`:  Ejecuta el último comando.
    *  `!cadena`:  Ejecuta el último comando que *comienza* con `cadena`.
    * `CTRL + R` + `cadena`: Búsqueda *inversa* en el historial.  Busca hacia atrás comandos que contengan `cadena`.  Presiona `CTRL + R` repetidamente para buscar coincidencias anteriores.

## 📚 **Ayuda**

*   `man` 📖 (Manual): Muestra el manual de un comando.  `man <comando>`  (ej: `man ls`).  Presiona `q` para salir.
*   `<comando> --help` (o `-h`):  Muestra un resumen de las opciones del comando (no todos los comandos lo soportan).
*   `info` ℹ️:  Otro sistema de documentación (similar a `man`, pero a veces más detallado).  `info <comando>`

## ✨ **Trucos y Consejos**

*   **Tab completion:**  Presiona la tecla `Tab` para autocompletar nombres de archivos, directorios y comandos.  Si hay varias opciones, presiona `Tab` dos veces para verlas.
*   **Redirección:**
    *   `>`:  Redirige la salida estándar (stdout) a un archivo (sobrescribiendo el contenido).  Ej:  `ls > archivos.txt`
    *   `>>`:  Redirige la salida estándar a un archivo (añadiendo al final).  Ej:  `ls >> archivos.txt`
    *   `<`:  Redirige la entrada estándar (stdin) desde un archivo.  Ej:  `sort < palabras.txt`
    *   `2>`:  Redirige la salida de error estándar (stderr) a un archivo.  Ej:  `comando_con_error 2> errores.txt`
    *   `&>`: Redirige tanto stdout como stderr a un archivo.  Ej: `comando_con_error &> salida.txt`
    *   `2>&1`:  Redirige stderr a stdout (para que ambos se capturen juntos).  Ej:  `comando_con_error > salida.txt 2>&1`
*   **Pipes (tuberías):**  `|`  Conecta la salida de un comando a la entrada de otro.  Ej:  `ls -l | grep ".txt"` (lista los archivos y luego filtra los que terminan en ".txt").
*   **Comodines:**
    *   `*`:  Representa cualquier secuencia de caracteres (o ningún carácter).  Ej:  `ls *.txt` (lista todos los archivos .txt).
    *   `?`:  Representa un único carácter.  Ej:  `ls documento?.txt` (lista `documento1.txt`, `documento2.txt`, etc., pero no `documento10.txt`).
    *   `[abc]`:  Representa un carácter de un conjunto (a, b o c en este caso).  Ej:  `ls archivo[123].txt`
    *   `[a-z]`: Representa un rango de carácteres.
    *   `[!abc]`: Representa un carácter que *no* está en un conjunto.
*   **Alias:** Crea atajos para comandos.
    *   `alias lista='ls -la'`  (define un alias).  Ahora, al escribir `lista`, se ejecutará `ls -la`.
    *   Para hacer que los alias sean permanentes, agrégalos a tu archivo de configuración de shell (ej: `~/.bashrc`, `~/.zshrc`).
* **Variables de entorno**
    *   `echo $VARIABLE`: muestra el valor de una variable de entorno.
    *  `export VARIABLE=valor`:  Establece o modifica una variable de entorno.
    *   `env`:  Muestra todas las variables de entorno.