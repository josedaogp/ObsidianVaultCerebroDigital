https://pub.dev/packages/build_runner

Esta librería la uso para poder autogenerar archivos autogenerados, como los de [[freezed]] o los de la internacionalización de la aplicación. Está mantenida oficialmente por el equipo de flutter.

Para ejecutarlo:
```
dart run build_runner build
```

Si tenemos conflictos por temas de caché, podemos utilizar la opción --delete-conflicting-outputs:
```
dart run build_runner build --delete-conflicting-outputs
```
## A tener en cuenta
La ejecución de este comando puede tardar bastante, así que deberíamos incluir en un fichero las carpetas que debe analizar, que será aquellas donde estén las clases que deban autogenerar algún fichero.
## Extra
Para no tener que ejecutarlo constantemente, podemos utilizar una extensión de visual studio llamada [[Command Runner]].

## Optimizar uso
Crear un fichero llamado build.yaml en la raíz del proyecto.

```
targets:
  $default:
    builders:
      json_serializable:
        generate_for:
          include:
            - lib/app/domain/models/**/*
            - lib/app/domain/models/*
      freezed:
        generate_for:
          include:
            - lib/app/presentation/**/**_state.dart
            - lib/app/domain/models/**/*
            - lib/app/domain/failures/**/*
            - lib/app/domain/either/either.dart
```

Incluir las carpetas donde debería ir a buscar los ficheros que tienen que ser autogenerados.

