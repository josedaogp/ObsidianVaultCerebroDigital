Descarga + info: https://pub.dev/packages/json_serializable

## Dependencias
[[build_runner]] --> para generar los archivos autogenerados
## Info
Hace la serialización de las propiedades de una clase para poder mapear por ejemplo una respuesta de una api.

## Fichero autogenerado
Creará un archivo con el mismo nombre que el de origen pero con .g antes del .dart.
Por ejemplo, si tenemos la clase user.dart, creará un user.g.dart.

## Uso
Primero, añadir la anotación @JsonSerializable
Para generar la serialización se pone \_$UserToJson donde User es el nombre de la clase principal, y se le pasa this. Por ejemplo:

```
Map<String, dynamic> toJson() => _$UserToJson(this);
```

## Nota
Esta librería se puede utilizar junto a [[freezed]].

Hace uso de la directiva part y part of de dart para hacer referencia a ficheros distintos como si fueran del mismo fichero.

Es importante meter en el .gitignore todos los archivos de código autogenerado. En este caso tendríamos que añadir