# 🐧🔥 Cheatsheet de Linux Avanzado: Domina la Terminal 🚀

## 🛡️ **Administración de Sistemas**

*   **`systemd` (system and service manager):** El sistema de inicio y gestión de servicios en la mayoría de las distribuciones modernas.

    *   `systemctl status <servicio>`:  Verifica el estado de un servicio.
    *   `systemctl start <servicio>`:  Inicia un servicio.
    *   `systemctl stop <servicio>`:  Detiene un servicio.
    *   `systemctl restart <servicio>`:  Reinicia un servicio.
    *   `systemctl enable <servicio>`:  Habilita un servicio para que se inicie automáticamente al arrancar.
    *   `systemctl disable <servicio>`:  Deshabilita el inicio automático de un servicio.
    *   `systemctl is-enabled <servicio>`: Comprueba si un servicio está habilitado.
    *   `systemctl list-units --type=service`:  Lista todos los servicios (activos e inactivos).
    *   `systemctl list-unit-files --type=service`:  Lista todos los archivos de unidad de servicio (y su estado de habilitación).
    *   `journalctl`:  Accede a los logs del sistema gestionados por `systemd`.
        *   `journalctl -u <servicio>`:  Muestra los logs de un servicio específico.
        *   `journalctl -f`:  Sigue los logs en tiempo real (como `tail -f`).
        *   `journalctl -b`:  Muestra los logs desde el último arranque.
        *   `journalctl -p err`:  Muestra solo los mensajes de error.
        *   `journalctl --since "yesterday"`:  Muestra los logs desde ayer.
        *   `journalctl --until "1 hour ago"`:  Muestra los logs hasta hace una hora.

*   **`crontab` (cron):**  Programa tareas para que se ejecuten automáticamente.

    *   `crontab -e`:  Edita el archivo `crontab` del usuario actual.
    *   `crontab -l`:  Lista las tareas programadas del usuario actual.
    *   `crontab -r`:  Elimina el archivo `crontab` del usuario actual.
    *   **Formato de una línea en `crontab`:**
        ```
        *     *     *     *     *  comando_a_ejecutar
        -     -     -     -     -
        |     |     |     |     |
        |     |     |     |     +----- Día de la semana (0 - 7) (Domingo es 0 o 7)
        |     |     |     +------- Mes (1 - 12)
        |     |     +--------- Día del mes (1 - 31)
        |     +----------- Hora (0 - 23)
        +------------- Minuto (0 - 59)
        ```
        *   Ejemplos:
            *   `0 0 * * *  comando`:  Ejecuta `comando` todos los días a medianoche.
            *   `*/5 * * * *  comando`:  Ejecuta `comando` cada 5 minutos.
            *   `0 22 * * 1-5  comando`:  Ejecuta `comando` a las 10 PM de lunes a viernes.
    *   `/etc/crontab`:  Archivo `crontab` del sistema (generalmente no se edita directamente; se usan archivos en `/etc/cron.d/`).
    *   `/etc/cron.d/`:  Directorio donde se pueden colocar archivos con tareas programadas (con el mismo formato que `crontab`).
    * `/etc/cron.{daily,hourly,weekly,monthly}`

*   **`at`:**  Ejecuta comandos *una sola vez* en un momento específico.

    *   `at now + 5 minutes`:  Ejecuta comandos en 5 minutos.  Después de escribir este comando, se abre un prompt donde puedes ingresar los comandos a ejecutar.  Presiona `Ctrl+D` para finalizar.
    *   `at 10:00 PM tomorrow`:  Ejecuta comandos mañana a las 10 PM.
    *   `atq`:  Lista los trabajos pendientes de `at`.
    *   `atrm <número_de_trabajo>`:  Elimina un trabajo pendiente de `at`.

*   **`lsof` (List Open Files):**  Lista los archivos abiertos y los procesos que los usan.

    *   `lsof -i :<puerto>`:  Muestra qué proceso está escuchando en un puerto específico.  Ej: `lsof -i :80`
    *   `lsof -p <PID>`:  Muestra los archivos abiertos por un proceso específico.
    *   `lsof -u <usuario>`:  Muestra los archivos abiertos por un usuario específico.
    * `lsof +D <directorio>`: Encuentra los procesos con archivos abiertos en un directorio y subdirectorios

* **`strace`:** Rastrea las llamadas al sistema (system calls) y señales de un proceso.  Muy útil para debugging.

    *   `strace <comando>`:  Ejecuta un comando y muestra las llamadas al sistema que realiza.
    *   `strace -p <PID>`:  Se adjunta a un proceso en ejecución y muestra sus llamadas al sistema.
    *   `strace -o <archivo_salida> <comando>`:  Guarda la salida de `strace` en un archivo.
    *   `strace -c <comando>`:  Cuenta el tiempo, las llamadas y los errores de cada llamada al sistema y muestra un resumen al finalizar.
    * `strace -f`: Sigue los procesos hijos

*   **`ltrace`:**  Similar a `strace`, pero rastrea las llamadas a *bibliotecas* (library calls).

    *   `ltrace <comando>`
    *   `ltrace -p <PID>`

*   **`dmesg` (Display Message):**  Muestra los mensajes del kernel (útil para diagnosticar problemas de hardware).

    *   `dmesg | less`:  Pasa la salida a `less` para facilitar la lectura.
    *   `dmesg -T`:  Muestra las marcas de tiempo (timestamps) legibles.
    * `dmesg -w`: Sigue el registro en tiempo real

* **`fdisk, parted, gdisk`** Herramientas para particionar discos
 * `sudo fdisk -l`: Lista las particiones de todos los discos.
 * `sudo parted /dev/sda`: Abre parted para manipular las particiones del disco /dev/sda

* **`mkfs`**: Crea un sistema de archivos (formatea) en una partición.  Ej: `sudo mkfs.ext4 /dev/sda1` (crea un sistema de archivos ext4 en `/dev/sda1`).  ¡Ten mucho cuidado!

* **`mount` / `umount`**:  Monta y desmonta sistemas de archivos.
    * `mount`: Lista los sistemas de archivos montados.
    * `sudo mount /dev/sda1 /mnt`: Monta la partición `/dev/sda1` en el directorio `/mnt`.
    * `sudo umount /mnt`: Desmonta el sistema de archivos montado en `/mnt`.

* **`df`, `du`** (ya mencionados en la sección básica, pero son fundamentales).

## 🧮 **Rendimiento y Monitoreo**

*   **`vmstat` (Virtual Memory Statistics):**  Muestra estadísticas sobre memoria virtual, procesos, CPU, etc.
    *   `vmstat 1 5`:  Muestra estadísticas cada segundo, 5 veces.
*   **`iostat` (Input/Output Statistics):**  Muestra estadísticas de E/S de disco y CPU.
    *   `iostat -x 1`:  Muestra estadísticas extendidas de E/S cada segundo.
*   **`iotop` (si está instalado):**  Similar a `top`, pero para E/S de disco (muestra qué procesos están leyendo/escribiendo más).
*   **`netstat` / `ss` (ya mencionados, pero importantes para el rendimiento de red).**
*   **`sar` (System Activity Reporter):**  Recopila y reporta información sobre la actividad del sistema (CPU, memoria, E/S, red, etc.).  Parte del paquete `sysstat`.
    *   `sar -u 1 5`:  Muestra el uso de CPU cada segundo, 5 veces.
    *   `sar -r 1 5`:  Muestra el uso de memoria cada segundo, 5 veces.
    *   `sar -n DEV 1 5`:  Muestra estadísticas de red cada segundo, 5 veces.
*   **`nmon` (Nigel's Monitor) (si está instalado):**  Herramienta interactiva para monitorear el rendimiento.
*   **`glances` (si está instalado):**  Otra herramienta interactiva de monitoreo del sistema (similar a `top` y `htop`, pero con más información).

## 🔒 **Seguridad**

*   **`iptables` (firewall):**  Configura el firewall de Linux (Netfilter).  Complejo, pero muy potente.  Muchas distribuciones ahora usan `firewalld` (que es una interfaz más amigable para `iptables`).
    *   `iptables -L`:  Lista las reglas actuales.
    *   `iptables -F`:  Elimina todas las reglas (¡ten cuidado!).
    *   `iptables -A INPUT -p tcp --dport 22 -j ACCEPT`:  Permite el tráfico SSH (puerto 22) entrante.
    *   `iptables -A INPUT -j DROP`:  Bloquea todo el tráfico entrante (¡ten cuidado!  Podrías bloquearte a ti mismo).
*   **`firewalld` (firewall):**  Interfaz más amigable para `iptables` (usada en Fedora, CentOS, RHEL, etc.).
    *   `firewall-cmd --list-all`:  Muestra la configuración actual.
    *   `firewall-cmd --add-service=ssh --permanent`:  Permite el tráfico SSH permanentemente.
    *   `firewall-cmd --reload`:  Recarga la configuración del firewall.
*   **`ufw` (Uncomplicated Firewall):**  Otra interfaz más amigable para `iptables` (usada en Ubuntu y Debian).
    *   `sudo ufw status`:  Muestra el estado del firewall.
    *   `sudo ufw allow ssh`:  Permite el tráfico SSH.
    *   `sudo ufw enable`:  Habilita el firewall.
    *   `sudo ufw disable`:  Deshabilita el firewall.
* **`fail2ban`:**  Protege contra ataques de fuerza bruta (monitorea logs y banea IPs).
*   **`SELinux` (Security-Enhanced Linux):**  Módulo de seguridad del kernel que proporciona un control de acceso obligatorio (MAC).  Complejo, pero aumenta significativamente la seguridad.
    *   `getenforce`:  Muestra el estado actual de SELinux (Enforcing, Permissive o Disabled).
    *   `setenforce 0`:  Cambia temporalmente a modo Permissive (registra violaciones pero no las bloquea).
    *   `setenforce 1`:  Cambia temporalmente a modo Enforcing (bloquea violaciones).
*   **`AppArmor`:**  Similar a SELinux (otro sistema MAC), pero generalmente considerado más fácil de usar.  Usado por defecto en Ubuntu y SUSE.
    *   `aa-status`:  Muestra el estado de AppArmor.
* **Auditoría**:
    * `auditd`:  Servicio de auditoría del kernel.
    * `ausearch`:  Busca eventos en los logs de auditoría.
    * `aureport`:  Genera informes a partir de los logs de auditoría.

## 🗜️ **Compresión y Archivado**

*   **`tar` (Tape Archive):**  Crea archivos `.tar` (archivos que contienen otros archivos y directorios, *sin comprimir* por defecto).
    *   `tar -cvf archivo.tar directorio/`:  Crea un archivo `.tar` llamado `archivo.tar` a partir del directorio `directorio/`.
    *   `tar -xvf archivo.tar`:  Extrae los archivos de `archivo.tar`.
    *   `tar -tf archivo.tar`:  Lista el contenido de `archivo.tar` sin extraerlo.
    *   `tar -czvf archivo.tar.gz directorio/`:  Crea un archivo `.tar.gz` (comprimido con `gzip`).
    *   `tar -xzvf archivo.tar.gz`:  Extrae un archivo `.tar.gz`.
    *   `tar -cjvf archivo.tar.bz2 directorio/`:  Crea un archivo `.tar.bz2` (comprimido con `bzip2`, mejor compresión que `gzip`).
    *   `tar -xjvf archivo.tar.bz2`:  Extrae un archivo `.tar.bz2`.
    * `tar -cJvf archivo.tar.xz directorio`: Crea un archivo .tar.xz (comprimido con xz, mejor compresión que gzip y bzip2)
    * `tar -xJvf archivo.tar.xz`: Extrae un archivo .tar.xz
*   **`gzip`:**  Comprime archivos (crea archivos `.gz`).  `gzip <archivo>`
*   **`gunzip`:**  Descomprime archivos `.gz`.  `gunzip <archivo.gz>`
*   **`bzip2`:**  Comprime archivos (crea archivos `.bz2`).  `bzip2 <archivo>`
*   **`bunzip2`:**  Descomprime archivos `.bz2`.  `bunzip2 <archivo.bz2>`
*   **`xz`**: Compresión con muy alta tasa de compresión.
    * `xz <archivo>`: Comprime un archivo.
    * `unxz <archivo.xz>`: Descomprime.

## 📜 **Expresiones Regulares (Regex)**

Las expresiones regulares son fundamentales en Linux (y en programación en general).  Se usan con comandos como `grep`, `sed`, `awk`, `vim`, etc.

*   **Metacaracteres básicos:**
    *   `.`:  Cualquier carácter (excepto salto de línea, a menos que se use la opción adecuada).
    *   `*`:  Cero o más repeticiones del carácter/grupo anterior.
    *   `+`:  Una o más repeticiones del carácter/grupo anterior.
    *   `?`:  Cero o una repetición del carácter/grupo anterior.
    *   `{n}`:  Exactamente `n` repeticiones.
    *   `{n,}`:  `n` o más repeticiones.
    *   `{n,m}`:  Entre `n` y `m` repeticiones (inclusive).
    *   `[]`:  Cualquier carácter dentro de los corchetes.  Ej: `[abc]` (a, b o c).  `[a-z]` (cualquier letra minúscula).
    *   `[^]`:  Cualquier carácter *que no esté* dentro de los corchetes.  Ej: `[^0-9]` (cualquier cosa que no sea un dígito).
    *   `^`:  Inicio de la línea (si está al principio de la expresión regular).
    *   `$`:  Fin de la línea (si está al final de la expresión regular).
    *   `\`:  Escapa un metacaracter (lo trata como un carácter literal).  Ej: `\.` (busca un punto literal).
    *   `()`:  Agrupa caracteres (para aplicar cuantificadores a todo el grupo).
    *   `|`:  Alternancia (OR).  Ej:  `gato|perro` (busca "gato" o "perro").
*   **Clases de caracteres predefinidas:**
    *   `\d`:  Dígito (equivalente a `[0-9]`).
    *   `\D`:  No dígito (equivalente a `[^0-9]`).
    *   `\w`:  Carácter "de palabra" (letra, dígito o guión bajo; equivalente a `[a-zA-Z0-9_]`).
    *   `\W`:  No carácter "de palabra".
    *   `\s`:  Espacio en blanco (espacio, tabulación, salto de línea, etc.).
    *   `\S`:  No espacio en blanco.

*   **Ejemplos con `grep`:**

    *   `grep "^Hola" archivo.txt`:  Busca líneas que *empiecen* con "Hola".
    *   `grep "mundo$" archivo.txt`:  Busca líneas que *terminen* con "mundo".
    *   `grep "a.b" archivo.txt`:  Busca líneas que contengan "a", seguida de cualquier carácter, seguida de "b".
    *   `grep "ab*" archivo.txt`:  Busca líneas que contengan "a", seguida de cero o más "b".
    *   `grep "ab+" archivo.txt`:  Busca líneas que contengan "a", seguida de una o más "b".
    *   `grep "a.*b" archivo.txt`: Busca líneas que tengan una "a", luego cualquier cantidad de caracteres y luego una "b"
    *   `grep "a[0-9]b" archivo.txt`:  Busca líneas que contengan "a", seguida de un dígito, seguida de "b".
    *   `grep "^[A-Z]" archivo.txt`:  Busca líneas que empiecen con una letra mayúscula.
    *   `grep "gato\|perro" archivo.txt`:  Busca líneas que contengan "gato" o "perro".
    *   `grep -E "(ab){2,}" archivo.txt`:  Busca líneas que contengan "ab" repetido dos o más veces.  `-E` activa las expresiones regulares extendidas (ERE), que son más potentes.

## ⚙️ **`sed` (Stream Editor):** Edición de texto no interactiva (en la línea de comandos).

*   `sed 's/<patron>/<reemplazo>/' <archivo>`:  Sustituye la *primera* ocurrencia de `<patron>` por `<reemplazo>` en cada línea.
    *   `sed 's/<patron>/<reemplazo>/g' <archivo>`:  Sustituye *todas* las ocurrencias de `<patron>` por `<reemplazo>` en cada línea (global).
    *   `sed 's/<patron>/<reemplazo>/2' <archivo>`: Sustituye solo la *segunda* ocurrencia.
    *   `sed '<dirección>s/<patron>/<reemplazo>/' <archivo>`:  Aplica la sustitución solo a las líneas que coincidan con la `<dirección>`.
        *   `sed '10s/a/b/' archivo.txt`:  Sustituye "a" por "b" solo en la línea 10.
        *   `sed '1,5s/a/b/' archivo.txt`:  Sustituye "a" por "b" en las líneas 1 a 5.
        *   `sed '/patron/s/a/b/' archivo.txt`:  Sustituye "a" por "b" en las líneas que contengan "patron".
        *   `sed '$s/a/b/'`: Sustituye en la última línea.
    *   `sed -n 'p' <archivo>`:  Imprime el archivo (equivalente a `cat`).  `-n` suprime la salida por defecto; `p` imprime las líneas.
    *   `sed -n '1,5p' <archivo>`:  Imprime las líneas 1 a 5.
    *   `sed 'd' <archivo>`:  Elimina las líneas (¡no modifica el archivo original por defecto!).
        *   `sed '10d' archivo.txt`:  Elimina la línea 10.
        * `sed '/patron/d'`: Elimina las líneas que contengan "patron".
    *   `sed -i 's/<patron>/<reemplazo>/g' <archivo>`:  Edita el archivo *en su lugar* (¡modifica el archivo original!).  Ten mucho cuidado con `-i`.
    * `sed 'y/abc/xyz/'`: Translitera caracteres (cambia 'a' por 'x', 'b' por 'y', 'c' por 'z').
    * `sed -e 's/foo/bar/g' -e 's/baz/qux/g'`:  Ejecuta varios comandos `sed` en secuencia.
* **Ejemplos:**

    *   `sed 's/Hola/Adiós/' archivo.txt`:  Reemplaza la primera "Hola" por "Adiós" en cada línea.
    *   `sed 's/ /_/g' archivo.txt`:  Reemplaza todos los espacios por guiones bajos.
    *   `sed '/^$/d' archivo.txt`:  Elimina las líneas en blanco.
    *   `sed 's/[0-9]//g' archivo.txt`:  Elimina todos los dígitos.

## ⚙️ **`awk`:**  Lenguaje de programación para procesamiento de texto (muy potente).

*   `awk '{ print $1 }' <archivo>`:  Imprime la primera columna (campo) de cada línea.  Los campos se separan por espacios en blanco por defecto.
*   `awk '{ print $2, $3 }' <archivo>`:  Imprime la segunda y tercera columnas.
*   `awk -F',' '{ print $1 }' <archivo>`:  Usa la coma (`,`) como separador de campos.  `-F` especifica el separador.
*   `awk '{ print NR, $0 }' <archivo>`:  Imprime el número de línea (NR) y la línea completa ($0).
*   `awk '$1 > 10 { print $2 }' <archivo>`:  Si la primera columna es mayor que 10, imprime la segunda columna.
*   `awk '/patron/ { print $0 }' <archivo>`:  Imprime las líneas que coincidan con "patron" (similar a `grep`).
*   `awk 'BEGIN { print "Inicio" } { print $0 } END { print "Fin" }' <archivo>`:  `BEGIN` se ejecuta antes de procesar el archivo, `END` se ejecuta después.
*   `awk '{ suma += $1 } END { print suma }' <archivo>`:  Suma los valores de la primera columna y los imprime al final.
*   `awk '{ if ($1 > max) max = $1 } END { print max }' <archivo>`:  Encuentra el valor máximo en la primera columna.
* `awk '{ a[$1]++ } END { for (i in a) print i, a[i] }'`:  Cuenta la frecuencia de cada valor en la primera columna (crea un histograma).
* **Ejemplos:**

    *   `ps aux | awk '{print $2, $11}'`:  Muestra el PID y el comando de cada proceso.
    *   `df -h | awk '$NF=="/"{printf "Espacio libre en /: %s\n", $(NF-1)}'`  (Obtiene el espacio libre en el directorio raíz).

## 🔗 **Redireccionamiento Avanzado**

*   **Descriptores de archivo:**
    *   `0`:  Entrada estándar (stdin).
    *   `1`:  Salida estándar (stdout).
    *   `2`:  Error estándar (stderr).
*   **`tee`:**  Divide la salida estándar y la envía tanto a la terminal como a un archivo (o varios).  Ej:  `ls -l | tee archivos.txt`
*   **`xargs`:**  Convierte la entrada estándar en argumentos para otro comando.  Muy útil para procesar listas de archivos.
    *   `find . -name "*.txt" | xargs rm`:  Busca archivos .txt y los elimina (¡cuidado!).  Mejor que `find ... -exec rm {} \;` en algunos casos (más eficiente si hay muchos archivos).
    *   `find . -name "*.jpg" | xargs -I {} convert {} {}.png`: Renombra archivos .jpg a .png usando ImageMagick's convert. -I {} define un placeholder.
    *   `cat urls.txt | xargs -P 10 wget`:  Descarga una lista de URLs en paralelo (10 a la vez). `-P` especifica el número de procesos paralelos.
*   **`exec`:**  Reemplaza el proceso actual de la shell por otro comando.
    *  `exec < archivo.txt`: Redirecciona la *entrada estándar de la shell*.  Todos los comandos subsiguientes leerán desde `archivo.txt`.
     * `exec > archivo.txt`: Redirecciona la *salida estándar de la shell*.
      * `exec 3< archivo.txt`:  Abre `archivo.txt` para lectura en el descriptor de archivo 3.
     *   `exec 4> archivo.txt`:  Abre `archivo.txt` para escritura en el descriptor de archivo 4.
      * `read -u 3 linea`:  Lee una línea del descriptor de archivo 3.
      *   `echo "Hola" >&4`:  Escribe "Hola" en el descriptor de archivo 4.
     *   `exec 3<&-`: Cierra el descriptor de archivo 3.
      * `exec 4>&-`:  Cierra el descriptor de archivo 4.

## ➕ **Otros Comandos Útiles**

*   **`watch`:**  Ejecuta un comando repetidamente y muestra su salida.  `watch -n 1 <comando>` (ejecuta cada segundo).
*   **`nc` (netcat):**  "Navaja suiza" de redes.  Puede leer y escribir datos a través de conexiones de red (TCP o UDP).
    *   `nc -l -p <puerto>`:  Escucha en un puerto (servidor simple).
    *   `nc <host> <puerto>`:  Se conecta a un host y puerto (cliente simple).
    *   `nc -z <host> <puerto>`:  Escanea un puerto (verifica si está abierto).
    *  `nc -u`: Usa UDP en vez de TCP.
* `rsync`: Sincronización de archivos (local o remota, muy eficiente).
    * `rsync -avz <origen> <destino>`: Sincroniza directorios (recursivo, verbose, comprime).

## 💡 **Bash Scripting Avanzado**

*   **Arrays:**
    *   `mi_array=(elemento1 elemento2 elemento3)`:  Define un array.
    *   `echo ${mi_array[0]}`:  Accede al primer elemento.
    *   `echo ${mi_array[@]}`:  Accede a *todos* los elementos.
    *   `echo ${#mi_array[@]}`:  Obtiene el número de elementos.
    *   `mi_array+=("nuevo elemento")`:  Añade un elemento al final.

*   **Arrays asociativos (diccionarios):**
    *   `declare -A mi_diccionario`:  Declara un array asociativo.
    *   `mi_diccionario=([clave1]=valor1 [clave2]=valor2)`:  Define un diccionario.
    *   `echo ${mi_diccionario[clave1]}`:  Accede al valor asociado con "clave1".
    *   `echo ${!mi_diccionario[@]}`:  Obtiene todas las claves.
    *   `echo ${mi_diccionario[@]}`:  Obtiene todos los valores.

*   **Funciones:**

    ```bash
    mi_funcion() {
        local variable_local="Valor local"
        echo "Parámetro 1: $1"
        echo "Todos los parámetros: $@"
        return 10  # Valor de retorno (opcional)
    }

    mi_funcion argumento1 argumento2
    echo "Valor de retorno: $?"  # $? contiene el valor de retorno del último comando
    ```

*   **`getopts`:**  Procesa opciones de línea de comandos en scripts.

    ```bash
    while getopts ":a:b:c" opt; do
      case $opt in
        a)
          echo "Opción -a con valor: $OPTARG"
          ;;
        b)
          echo "Opción -b con valor: $OPTARG"
          ;;
        c)
          echo "Opción -c"
          ;;
        \?)
          echo "Opción inválida: -$OPTARG" >&2
          ;;
      esac
    done
    ```
* **`trap`:** Maneja señales.
  ```bash
    trap 'echo "Señal de interrupción recibida"; exit 1' INT #Captura la señal INT (Ctrl+C)
  ```

*   **`read` (con opciones):**

    *   `read -p "Ingrese su nombre: " nombre`:  Muestra un prompt y lee la entrada.
    *   `read -s -p "Ingrese su contraseña: " contrasena`:  Lee una contraseña sin mostrarla en la terminal.
    *   `read -a mi_array`:  Lee la entrada y la guarda en un array.
    * `read -r`: No interpreta backslashes.
    * `read -t 5`: Timeout (espera 5 segundos).
* **`printf`** Formateo de salida
   ```bash
     printf "El valor es: %.2f\n" 12.345 # Imprime "El valor es: 12.35"
   ```

*   **Expansión de llaves:**

    *   `echo {a,b,c}{1,2,3}`:  Genera: a1 a2 a3 b1 b2 b3 c1 c2 c3
    *   `mkdir /tmp/proyecto/{src,bin,docs}`: Crea varios directorios a la vez.

*   **Sustitución de comandos:**

    *   `fecha=$(date)`:  Guarda la salida del comando `date` en la variable `fecha`.  (Forma moderna).
    *   `fecha=`date``:  Forma antigua (backticks).

* **Aritmética en la shell**
 * `resultado=$(( 5 + 3 ))`
 * `(( a++ ))` # Incrementa a
 * `(( a > b )) && echo "a es mayor que b"`

* **Condicionales avanzados**
 *  `[[ ]]`: Versión extendida de `[]` para condicionales.  Permite expresiones regulares y más.

```bash
if [[ "$cadena" =~ ^[0-9]+$ ]]; then  # Comprueba si $cadena es un número entero.
  echo "Es un número"
fi
```
