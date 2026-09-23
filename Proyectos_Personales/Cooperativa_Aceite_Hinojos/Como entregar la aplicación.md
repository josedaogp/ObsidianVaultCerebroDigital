1. Cambiar en el main.py el nombre del contenedor de docker en el check_docker_running y en el start_docker_container
2. Generar el .exe con pyinstaller --onefile --windowed main.py LEER ABAJO
	1. --windowed puede dar problemas. Usar este comando: pyinstaller --onefile --add-data "fuente:fuente" main.py .Importante el --add-data para añadir las fuentes
SERÍA ASÍ:
```git
pyinstaller --onefile --add-data "fuente:fuente" main.py
```

4. Sacar los requerimientos con pip freeze > requirements.txt
5. Instalar pgadmin4


### **Paso 1: Verificar que Docker está instalado**

6. **Abrir PowerShell o CMD (símbolo del sistema)** en la máquina del cliente.
7. Ejecutar el siguiente comando para verificar si Docker está instalado:
    
    `docker --version`
    
    Si Docker no está instalado, sigue los siguientes pasos para instalarlo.

#### Instalación de Docker:

8. **Descargar Docker Desktop para Windows**:
    
    - Ve al sitio oficial de Docker Docker Desktop para Windows.
    - Haz clic en **"Get Docker"** para descargar el instalador.
9. **Instalar Docker**:
    
    - Ejecuta el archivo descargado y sigue las instrucciones del instalador.
    - Asegúrate de que Docker Desktop se inicie correctamente después de la instalación.
10. **Verificar la instalación de Docker**:
    
    - Una vez que Docker esté instalado, abre PowerShell o CMD nuevamente.
    - Ejecuta:
        
        bash
        
        Copiar código
        
        `docker --version`
        
    - También puedes verificar si Docker está corriendo correctamente con:
        
        bash
        
        Copiar código
        
        `docker info`
        

---

### **Paso 2: Verificar si Python está instalado**

11. **Verificar la instalación de Python**:
    - Abre PowerShell o CMD y ejecuta el siguiente comando para verificar si Python está instalado:
        
        bash
        
        Copiar código
        
        `python --version`
        
        Si Python no está instalado, sigue los pasos para instalarlo.

#### Instalación de Python:

12. **Descargar Python**:
    
    - Dirígete al sitio oficial de Python [python.org](https://www.python.org/downloads/).
    - Descarga la última versión de Python para Windows.
13. **Instalar Python**:
    
    - Durante la instalación, **marca la opción "Add Python to PATH"** para asegurarte de que Python se pueda ejecutar desde cualquier terminal.
    - Selecciona **"Install Now"** para una instalación estándar.
    - Verifica que Python se haya instalado correctamente ejecutando:
        
        bash
        
        Copiar código
        
        `python --version`
        
14. **Instalar `pip` (si no se instala automáticamente)**:
    
    - Si por alguna razón no se instala `pip` junto con Python, puedes instalarlo manualmente:
        - Descarga [get-pip.py](https://bootstrap.pypa.io/get-pip.py).
        - Ejecuta el siguiente comando en la terminal:
            
            bash
            
            Copiar código
            
            `python get-pip.py`
            

---

### **Paso 3: Instalar las dependencias de Python**

15. **Instalar dependencias necesarias para tu aplicación**:
    - Si ya tienes un archivo `requirements.txt` que contiene las dependencias necesarias para tu aplicación, ejecuta el siguiente comando:
        
        bash
        
        Copiar código
        
        `pip install -r requirements.txt`
        
    - Si no tienes este archivo, puedes instalar manualmente las dependencias, por ejemplo:
        
        bash
        
        Copiar código
        
        `pip install pyside6 pip install psycopg2  # si usas psycopg2 para la conexión con PostgreSQL`
        

---

### **Paso 4: Verificar Docker y Base de Datos (si no lo has hecho con el script)**

16. **Verificar si el contenedor de PostgreSQL está corriendo**:
    
    - Abre PowerShell o CMD y ejecuta:
        
        bash
        
        Copiar código
        
        `docker ps`
        
    - Esto te mostrará los contenedores activos. Si no ves el contenedor de PostgreSQL, puedes iniciarlo con el siguiente comando:
        
        bash
        
        Copiar código
        
        `docker start <nombre_del_contenedor>`
        
        O, si no está creado, puedes ejecutarlo con:
        
        bash
        
        Copiar código
        
 ```

docker run --name cooperativa_aceite_postgres_test_db \
  -e POSTGRES_USER=cooperativaAHtest \
  -e POSTGRES_PASSWORD=cooperativaAHtestpass \
  -e POSTGRES_DB=cooperativaHinojosTestBBDD \
  -p 5433:5432 \
  -v postgres_test_data:/var/lib/postgresql/data \
  --restart always \
  -d postgres:15

```

        
17. **Verificar que la base de datos está accesible y copiar la estructura de datos**:
    
    - Abre el cliente de tu aplicación o una herramienta como **pgAdmin** o **DBeaver** para verificar la conexión a la base de datos PostgreSQL en `localhost:5432`.

---

### **Paso 5: Instalar la Aplicación Generada**

18. **Ejecutar el instalador del ejecutable** (si tienes un instalador que genera el `.exe`):
    
    - Si has utilizado PyInstaller para crear un `.exe` de tu aplicación, simplemente ejecuta el instalador y sigue los pasos del proceso.
19. **Ejecutar la aplicación manualmente** (si no usas un instalador):
    
    - Si no tienes un instalador, solo ejecuta el archivo `.exe` generado por PyInstaller en la carpeta `dist`.

---

### **Paso 6: Verificar que todo funciona**

20. **Iniciar la aplicación**:
    - Ejecuta la aplicación generada con el archivo `.exe` o desde el instalador.
    - Asegúrate de que la aplicación pueda conectarse correctamente a la base de datos.
    - Verifica que no haya errores y que la base de datos esté levantada si no estaba activa antes.

---

### **Resumen de la instalación paso a paso**:

21. **Verificar si Docker está instalado**. Si no está, instalar Docker Desktop.
22. **Verificar si Python está instalado**. Si no está, instalar Python y asegurarte de que esté en el `PATH`.
23. **Instalar las dependencias de Python** si no se hizo automáticamente.
24. **Verificar que Docker y la base de datos estén en funcionamiento**.
25. **Instalar y ejecutar el `.exe` de la aplicación**.