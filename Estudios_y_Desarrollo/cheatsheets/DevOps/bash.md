#  Bash Cheatsheet: Dominando la Terminal 🚀

## 🌟 **Conceptos Fundamentales**

*   **Bash (Bourne Again Shell):** Un intérprete de comandos (shell) y un lenguaje de scripting para sistemas Unix y Linux.  Es el shell por defecto en la mayoría de las distribuciones de Linux y en macOS (hasta Catalina; zsh es el shell por defecto a partir de macOS Big Sur).
*   **Shell:** Un programa que proporciona una interfaz de línea de comandos para interactuar con el sistema operativo.
*   **Script:** Un archivo de texto que contiene una secuencia de comandos.
*   **Comando (Command):**  Una instrucción que se le da al shell.
*   **Argumento (Argument):**  Información adicional que se le pasa a un comando.
*   **Opción (Option/Flag):**  Un tipo especial de argumento que modifica el comportamiento de un comando.  Generalmente empiezan con `-` (opciones cortas, ej: `-l`) o `--` (opciones largas, ej: `--all`).
*   **Variable:**  Un nombre que representa un valor (texto, número, etc.).
*   **Redirección (Redirection):**  Cambiar la entrada o salida estándar de un comando.
*   **Pipes (Tuberías):**  Conectar la salida estándar de un comando a la entrada estándar de otro comando.
*   **Comodines (Wildcards):**  Caracteres especiales que representan uno o más caracteres (ej: `*`, `?`, `[]`).
*   **Expansión (Expansion):**  El proceso por el cual el shell reemplaza ciertos caracteres o expresiones por sus valores antes de ejecutar un comando.

## 🚀 **Ejecutar Comandos**

*   **Ejecutar un comando:**  Simplemente escribe el nombre del comando y presiona Enter.

    ```bash
    ls -l  # Lista los archivos y directorios en formato largo
    ```

*   **Ejecutar un script:**

    ```bash
    bash mi_script.sh  # Ejecuta el script con bash
    ./mi_script.sh   # Ejecuta el script directamente (si tiene permisos de ejecución)
    ```
    *   Para ejecutar un script directamente, debe tener el permiso de ejecución (`chmod +x mi_script.sh`) y, opcionalmente, una línea *shebang* al principio (`#!/bin/bash`).

## 📝 **Comandos Básicos**

*   **`echo`:**  Imprime texto en la terminal.

    ```bash
    echo "Hola, mundo!"
    echo $VARIABLE  # Imprime el valor de la variable VARIABLE
    ```

*   **`pwd` (Print Working Directory):**  Muestra el directorio actual.

*   **`cd` (Change Directory):**  Cambia de directorio.
    *   `cd <directorio>`
    *   `cd ..` (directorio padre)
    *   `cd ~` (directorio home)
    *   `cd -` (directorio anterior)
    *   `cd` (sin argumentos, va al directorio home)

*   **`ls` (List):**  Lista archivos y directorios.
    *   `ls -l` (formato largo)
    *   `ls -a` (muestra todos los archivos, incluidos los ocultos)
    *   `ls -h` (tamaños "humanos")
    *   `ls -t` (ordena por fecha de modificación)
    *   `ls -r` (invierte el orden)

*   **`mkdir` (Make Directory):**  Crea un directorio.
    *   `mkdir <nombre_directorio>`
    *   `mkdir -p <ruta/con/subdirectorios>` (crea directorios intermedios si no existen)

*   **`rmdir` (Remove Directory):**  Elimina un directorio *vacío*.
*   **`rm` (Remove):**  Elimina archivos y directorios.
    *   `rm <archivo>`
    *   `rm -r <directorio>` (elimina un directorio de forma recursiva, ¡cuidado!)
    *   `rm -f <archivo>` (fuerza la eliminación)
    *   `rm -rf <directorio>` (¡extremadamente peligroso!)

*   **`cp` (Copy):**  Copia archivos y directorios.
    *   `cp <origen> <destino>`
    *   `cp -r <directorio_origen> <directorio_destino>` (copia recursivamente)

*   **`mv` (Move):**  Mueve o renombra archivos y directorios.
    *   `mv <origen> <destino>`

*   **`touch`:**  Crea un archivo vacío o actualiza la fecha de modificación de un archivo existente.
*   **`cat`:**  Muestra el contenido de un archivo, concatena archivos.
*   **`less` / `more`:**  Visualiza el contenido de un archivo página por página.
*   **`head`:**  Muestra las primeras líneas de un archivo.
*   **`tail`:**  Muestra las últimas líneas de un archivo.
    *   `tail -f <archivo>` (sigue mostrando las nuevas líneas a medida que se añaden)

*   **`grep`:**  Busca líneas que coincidan con un patrón.
    *   `grep "<patron>" <archivo>`
    *   `grep -i "<patron>" <archivo>` (ignora mayúsculas/minúsculas)
    *   `grep -v "<patron>" <archivo>` (muestra las líneas que *no* coinciden)
    *   `grep -r "<patron>" <directorio>` (busca recursivamente)
    *   `grep -E "<expresion_regular>" <archivo>` (expresiones regulares extendidas)
*   **`wc` (Word Count):**  Cuenta líneas, palabras y caracteres.
*   **`sort`:**  Ordena líneas de texto.
*   **`uniq`:**  Elimina líneas duplicadas consecutivas (generalmente se usa después de `sort`).
*   **`find`:**  Busca archivos y directorios.
*   **`chmod` (Change Mode):**  Cambia los permisos de un archivo o directorio.
*   **`chown` (Change Owner):**  Cambia el propietario de un archivo o directorio.
*   **`chgrp` (Change Group):** Cambia el grupo propietario.
*   **`ps` (Process Status):**  Muestra información sobre los procesos en ejecución.
*   **`top` / `htop`:**  Muestra una vista dinámica de los procesos en ejecución.
*   **`kill`:**  Envía una señal a un proceso (generalmente para terminarlo).
*   **`df` (Disk Free):**  Muestra el espacio libre en disco.
*   **`du` (Disk Usage):**  Muestra el espacio usado por archivos y directorios.
*   **`man`:**  Muestra el manual de un comando.
*  **`history`**: Muestra el historial de comandos.
* **`which`**: Localiza un comando.
* **`whereis`**: Localiza los archivos binarios, de código fuente y de manual de un comando.
* **`whatis`**: Muestra una descripción corta de un comando.

## 💻 **Variables**

*   **Definir una variable:**

    ```bash
    NOMBRE="Juan"
    EDAD=30
    ```
    *   ¡No debe haber espacios alrededor del signo `=`!

*   **Acceder al valor de una variable:**  Usa `$`.

    ```bash
    echo "Hola, me llamo $NOMBRE y tengo $EDAD años."
    echo "Hola, me llamo ${NOMBRE} y tengo ${EDAD} años." # Forma más segura (con llaves)
    ```

*   **Variables de entorno (Environment Variables):**  Variables que afectan al comportamiento del shell y de otros programas.
    *   `$HOME`:  Directorio home del usuario.
    *   `$PATH`:  Lista de directorios donde el shell busca comandos.
    *   `$USER`:  Nombre de usuario actual.
    *   `$PWD`:  Directorio actual.
    *   `$SHELL`:  Ruta al shell actual.
    *   `$TERM`:  Tipo de terminal.
    * `printenv`: Muestra todas las variables de entorno
    *   `export VARIABLE=valor`:  Define una variable de entorno (disponible para los procesos hijos).

*  **Variables locales**: Solo existen dentro del script o función donde se definen.  `local mi_variable=valor`
* **Arrays**:
    ```bash
      mi_array=(elemento1 elemento2 elemento3)
      echo ${mi_array[0]} # Accede al primer elemento
      echo ${mi_array[@]} # Accede a todos los elementos
      echo ${#mi_array[@]} # Obtiene el número de elementos
    ```

## ➡️ **Redirección**

*   **`>`:**  Redirige la salida estándar (stdout) a un archivo (sobrescribiendo el contenido).

    ```bash
    ls -l > lista_archivos.txt
    ```

*   **`>>`:**  Redirige la salida estándar a un archivo (añadiendo al final).

    ```bash
    echo "Nueva línea" >> lista_archivos.txt
    ```

*   **`<`:**  Redirige la entrada estándar (stdin) desde un archivo.

    ```bash
    sort < palabras.txt
    ```

*   **`2>`:**  Redirige la salida de error estándar (stderr) a un archivo.

    ```bash
    comando_con_error 2> errores.txt
    ```
*   **`&>`:**  Redirige tanto stdout como stderr a un archivo.

    ```bash
    comando_con_error &> salida.txt
    ```
*  **`2>&1`**: Redirige stderr a stdout.
* **`|` (Pipe):**  Conecta la salida estándar de un comando a la entrada estándar de otro comando.

    ```bash
    ls -l | grep ".txt"  # Lista los archivos y luego filtra los que terminan en ".txt"
    cat archivo.txt | sort | uniq #Ejemplo de uso de pipes
    ```

## 💬 **Comentarios**

```bash
# Esto es un comentario (línea completa)

comando  # Esto es un comentario (después de un comando)
```

## 🔠 **Expansión**

*   **Expansión de variables:**  `$VARIABLE` o `${VARIABLE}`.
*   **Expansión aritmética:**  `$(( expresión ))`.

    ```bash
    echo $(( 5 + 3 * 2 ))  # Imprime 11
    ```

*   **Expansión de comandos:**  `$(comando)` o `` `comando` `` (backticks).  Reemplaza la expresión por la salida del comando.

    ```bash
    fecha=$(date)
    echo "Hoy es $(date)"
    ```

*   **Expansión de llaves (Brace Expansion):**

    ```bash
    echo {a,b,c}{1,2,3}  # Genera: a1 a2 a3 b1 b2 b3 c1 c2 c3
    mkdir proyecto/{src,bin,docs} #Crea varios directorios
    cp archivo.{txt,bak} #Equivalente a cp archivo.txt archivo.bak
    ```

*   **Expansión de tilde (`~`):**  `~` se expande al directorio home del usuario actual, `~/ruta` a una ruta dentro del home.
* **Expansión de parámetros**: Manipula variables. Ejemplos:
  * `${var:-valor}`: Si var no existe o está vacío, usa "valor"
  * `${var:=valor}`: Si var no existe o está vacío, le asigna "valor".
  * `${var:?mensaje}`: Si var no existe o está vacío, muestra "mensaje" y termina el script.
  * `${var:+valor}`: Si var existe y no está vacía, usa "valor"
  *`${#var}`: Longitud de la variable.
  *`${var:offset}`: Extrae una subcadena.
  * `${var:offset:length}`: Extrae una subcadena
  * `${var#patrón}`: Elimina el *prefijo* más corto que coincida.
  * `${var##patrón}`: Elimina el *prefijo* más largo que coincida.
  * `${var%patrón}`: Elimina el *sufijo* más corto que coincida.
  * `${var%%patrón}`: Elimina el *sufijo* más largo que coincida.
  * `${var/patrón/reemplazo}`: Reemplaza la primera coincidencia.
  * `${var//patrón/reemplazo}`: Reemplaza *todas* las coincidencias.
  * `${var^}`: Convierte el primer carácter a mayúscula
  * `${var^^}`: Convierte toda la cadena a mayúscula.
  * `${var,}`: Convierte el primer carácter a minúscula.
  * `${var,,}`: Convierte toda la cadena a minúscula.

## ❓ **Comodines (Wildcards)**

*   **`*`:**  Representa cero o más caracteres.

    ```bash
    ls *.txt  # Lista todos los archivos que terminan en .txt
    rm  *.log #Elimina todos los que terminan en .log
    ```

*   **`?`:**  Representa un único carácter.

    ```bash
    ls documento?.txt  # Lista documento1.txt, documento2.txt, etc.
    ```

*   **`[]`:**  Representa un carácter de un conjunto o rango.

    ```bash
    ls archivo[123].txt  # Lista archivo1.txt, archivo2.txt, archivo3.txt
    ls [a-z]* # Lista todos los archivos cuyo nombre empiece por una letra minúscula.
    ls *[!0-9]* # Lista todos los archivos cuyo nombre *no* contenga ningún dígito
    ```

## 🚦 **Control de Flujo (Condicionales y Bucles)**

*   **`if`-`then`-`elif`-`else`-`fi`:**

    ```bash
    if [ "$EDAD" -gt 18 ]; then # -gt: greater than (mayor que)
      echo "Eres mayor de edad."
    elif [ "$EDAD" -eq 18 ]; then # -eq: equal (igual)
      echo "Tienes 18 años."
    else
      echo "Eres menor de edad."
    fi

    # También se puede usar [[ ]] (más potente):
    if [[ "$NOMBRE" == "Juan" ]]; then
      echo "Hola, Juan!"
    fi
    ```

    *   **Operadores de comparación numérica:**  `-eq` (igual), `-ne` (no igual), `-gt` (mayor que), `-ge` (mayor o igual que), `-lt` (menor que), `-le` (menor o igual que).
    *   **Operadores de comparación de cadenas:**  `=` o `==` (igual), `!=` (no igual), `<` (menor que, según orden lexicográfico), `>` (mayor que).
    *   **Operadores de archivos:**  `-e` (existe), `-f` (es un archivo regular), `-d` (es un directorio), `-r` (se puede leer), `-w` (se puede escribir), `-x` (se puede ejecutar), etc.
    * **Operadores lógicos**: `-a` (and), `-o` (or), `!` (not).  Dentro de `[[ ]]`, se pueden usar `&&`, `||` y `!`.

*   **`case`:**

    ```bash
    case "$OPCION" in
      a)
        echo "Opción A seleccionada"
        ;;
      b)
        echo "Opción B seleccionada"
        ;;
      *)  # Caso por defecto
        echo "Opción no válida"
        ;;
    esac
    ```

*   **`for` (bucle):**

    ```bash
    for fruta in manzana plátano naranja; do
      echo "Me gusta la $fruta"
    done

    # Iterar sobre los archivos de un directorio:
    for archivo in *.txt; do
      echo "Procesando: $archivo"
    done

    # Bucle estilo C:
      for (( i=0; i<10; i++ )); do
        echo $i
    done
    ```

*   **`while` (bucle):**

    ```bash
    contador=0
    while [ "$contador" -lt 5 ]; do  # Mientras contador sea menor que 5
      echo "Contador: $contador"
      ((contador++)) # Incrementa el contador
    done
    ```

*   **`until` (bucle):**  Similar a `while`, pero se ejecuta *hasta que* la condición sea verdadera.

*   **`break`:**  Sale de un bucle.
*   **`continue`:**  Salta a la siguiente iteración de un bucle.

## 📝 **Funciones**

```bash
mi_funcion() {
  local variable_local="Valor local"
  echo "Parámetro 1: $1"
  echo "Todos los parámetros: $@"
  return 10 #Valor de retorno (opcional)
}
#Llamar a la función
mi_funcion "argumento1" "argumento2"
echo "Valor de retorno: $?" # $? contiene el valor de retorno del último comando
```

## 💡 **Trucos y Consejos**

*   **`!!`:**  Ejecuta el último comando.
*   **`!<número>`:**  Ejecuta el comando número `<número>` del historial.
*   **`!cadena`:**  Ejecuta el último comando que *comienza* con `cadena`.
*   **`CTRL + R`:**  Búsqueda *inversa* en el historial.
*   **Tab completion:**  Presiona la tecla `Tab` para autocompletar nombres de archivos, directorios y comandos.
*   **`alias`:**  Crea atajos para comandos.
*  **`set -e`**:  Termina el script inmediatamente si un comando falla.
* **`set -u`**:  Termina el script si se usa una variable no definida.
*  **`set -x`**:  Muestra los comandos que se ejecutan (para debugging).
*  **`trap`**:  Captura señales (ej: para limpiar archivos temporales antes de salir).
* **`$RANDOM`**: Retorna un número aleatorio.
*  **`${#variable}`**:  Obtiene la longitud de una cadena.
* **`read`**: Lee la entrada del usuario.
* **Here documents (`<<`)**: Redirige un bloque de texto a la entrada estándar de un comando.
*  **Here strings (`<<<`)**: Redirige una cadena a la entrada estándar de un comando.