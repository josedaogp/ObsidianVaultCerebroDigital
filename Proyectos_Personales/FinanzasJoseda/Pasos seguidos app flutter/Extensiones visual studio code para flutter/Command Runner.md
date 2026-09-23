Para lanzar automáticamente la generación del freezed, internacionalización, pub get y todos los comandos que queramos.

La configuración se hace en el fichero settings.json que estará en la carpeta .vscode en la raíz del proyecto. Si no tenemos la carpeta y el fichero, lo creamos.

Ejemplo settings.json:
```
{
    "command-runner.commands": {
        "pub get": "flutter pub get",
        "build_runner": "flutter pub run build_runner build",
        ##"nombre_comando": comando_a_ejecutar
    }
}
```


