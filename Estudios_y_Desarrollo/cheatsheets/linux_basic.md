# 🐧 Linux Command Cheat Sheet

Una guía rápida y visual de comandos esenciales de Linux, organizados por categoría.

---

## 📁 Archivos y Directorios

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `ls` | Lista archivos y carpetas | `ls -la` |
| `cd` | Cambia de directorio | `cd /var/log` |
| `pwd` | Muestra el directorio actual | `pwd` |
| `mkdir` | Crea una carpeta | `mkdir nueva_carpeta` |
| `rm` | Elimina archivos o carpetas | `rm archivo.txt`, `rm -rf carpeta/` |
| `cp` | Copia archivos o carpetas | `cp file.txt backup/` |
| `mv` | Mueve o renombra archivos | `mv viejo.txt nuevo.txt` |
| `touch` | Crea archivos vacíos | `touch archivo.txt` |
| `find` | Busca archivos en el sistema | `find / -name "*.log"` |
| `stat` | Muestra detalles de un archivo | `stat archivo.txt` |

---

## 🔍 Búsqueda y Procesamiento de Texto

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `cat` | Muestra el contenido de un archivo | `cat archivo.txt` |
| `less` / `more` | Visualiza archivos de forma paginada | `less archivo.txt` |
| `head` / `tail` | Muestra primeras/últimas líneas | `head -n 10 archivo.log` |
| `grep` | Busca texto en archivos | `grep "error" archivo.log` |
| `cut` | Extrae columnas de texto | `cut -d ":" -f1 archivo.txt` |
| `awk` | Procesa columnas y campos | `awk -F ":" '{print $2}' archivo.txt` |
| `sed` | Reemplaza o modifica texto | `sed 's/viejo/nuevo/' archivo.txt` |
| `tr` | Sustituye caracteres | `tr 'a-z' 'A-Z'` |
| `sort` | Ordena líneas alfabéticamente | `sort archivo.txt` |
| `uniq` | Elimina líneas duplicadas | `sort archivo.txt | uniq` |
| `wc` | Cuenta líneas, palabras, caracteres | `wc -l archivo.txt` |
| `diff` | Muestra diferencias entre archivos | `diff archivo1 archivo2` |
| `xargs` | Ejecuta comandos sobre la salida de otro | `find . -name "*.log" | xargs rm` |
| `tee` | Muestra salida y guarda en archivo | `echo "hola" | tee hola.txt` |

---

## 🔧 Procesos y Rendimiento

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `ps` | Lista procesos activos | `ps aux` |
| `top` / `htop` | Monitor de procesos en tiempo real | `top` |
| `kill` | Termina un proceso por PID | `kill 1234` |
| `killall` | Mata todos los procesos con un nombre | `killall firefox` |
| `jobs` | Muestra procesos en segundo plano | `jobs` |
| `bg` / `fg` | Mueve procesos entre segundo y primer plano | `fg %1` |
| `nice` / `renice` | Cambia la prioridad de procesos | `nice -n 10 comando` |

---

## 💾 Disco y Archivos

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `df` | Muestra uso de disco | `df -h` |
| `du` | Muestra tamaño de carpetas/archivos | `du -sh *` |
| `lsblk` | Lista discos y particiones | `lsblk` |
| `mount` / `umount` | Monta y desmonta dispositivos | `mount /dev/sdb1 /mnt` |
| `blkid` | Muestra UUIDs de dispositivos | `blkid` |
| `file` | Detecta el tipo de archivo | `file imagen.png` |

---

## 🔐 Permisos y Usuarios

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `whoami` | Muestra el usuario actual | `whoami` |
| `id` | Muestra UID/GID y grupos | `id` |
| `chmod` | Cambia permisos de archivos | `chmod 755 archivo` |
| `chown` | Cambia propietario de archivos | `chown user:group archivo` |
| `adduser` / `useradd` | Crea un usuario nuevo | `adduser nuevo` |
| `passwd` | Cambia la contraseña de un usuario | `passwd user` |
| `groups` | Muestra grupos del usuario | `groups user` |

---

## 🔌 Redes

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `ip` | Muestra info de red | `ip a` |
| `ping` | Verifica conectividad | `ping google.com` |
| `traceroute` | Muestra la ruta a un host | `traceroute google.com` |
| `nslookup` / `dig` | Resuelve DNS | `dig racks.academy` |
| `netstat` / `ss` | Muestra conexiones y puertos | `ss -tuln` |
| `curl` / `wget` | Descarga o prueba endpoints HTTP | `curl https://example.com` |
| `telnet` | Prueba puertos abiertos | `telnet 192.168.1.10 80` |
| `tcpdump` | Captura paquetes de red | `tcpdump -i eth0` |
| `nmap` | Escanea puertos de red | `nmap 192.168.1.1` |
| `ethtool` | Muestra estado de interfaces | `ethtool eth0` |

---

## 📦 Gestión de Paquetes

### Debian/Ubuntu (APT)

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `apt update` | Actualiza lista de paquetes | `sudo apt update` |
| `apt upgrade` | Actualiza todos los paquetes | `sudo apt upgrade` |
| `apt install` | Instala un paquete | `sudo apt install nginx` |
| `apt remove` | Elimina un paquete | `sudo apt remove nginx` |
| `dpkg -i` | Instala desde archivo `.deb` | `sudo dpkg -i paquete.deb` |

---

## 🔄 Compresión y Archivos

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `tar` | Empaqueta y comprime archivos | `tar -czvf backup.tar.gz carpeta/` |
| `gzip` / `gunzip` | Comprime/descomprime `.gz` | `gzip archivo.txt` |
| `zip` / `unzip` | Comprime/descomprime `.zip` | `unzip archivo.zip` |

---

## 🌐 SSH y Transferencia de Archivos

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `ssh` | Conecta a un servidor remoto | `ssh usuario@ip` |
| `scp` | Copia archivos entre máquinas | `scp archivo.txt user@ip:/ruta/` |
| `rsync` | Sincroniza archivos/directorios | `rsync -av carpeta/ user@ip:/destino/` |
| `ssh-keygen` | Genera claves SSH | `ssh-keygen -t rsa -b 4096` |

---

## ⚙️ Sistema y Configuración

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `uname` | Muestra info del kernel | `uname -a` |
| `uptime` | Tiempo encendido del sistema | `uptime` |
| `hostname` | Muestra o cambia el hostname | `hostnamectl set-hostname nuevo-host` |
| `dmesg` | Logs del kernel | `dmesg | tail` |
| `journalctl` | Logs de systemd | `journalctl -xe` |
| `systemctl` | Control de servicios | `systemctl restart nginx` |
| `shutdown` | Apaga el sistema | `shutdown now` |
| `reboot` | Reinicia el sistema | `reboot` |

---

## 📎 Otros útiles

| Comando | Descripción | Ejemplo |
|--------|-------------|---------|
| `echo` | Imprime texto | `echo "Hola"` |
| `date` | Muestra la fecha y hora | `date "+%d/%m/%Y %H:%M:%S"` |
| `alias` | Crea atajos de comandos | `alias ll='ls -la'` |
| `history` | Muestra historial de comandos | `history` |
| `crontab` | Programa tareas | `crontab -e` |
| `man` | Muestra el manual de un comando | `man ls` |

---

> ✅ **TIP:** Usa `man <comando>` o `--help` para ver detalles y opciones de cada comando.

