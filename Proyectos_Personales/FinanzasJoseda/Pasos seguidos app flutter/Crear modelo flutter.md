Ahora que tengo el repositorio creado con la función de obtener los bienes, voy a crear el modelo de un Bien.

## Ejemplo
Me voy a basar en este:
```
import 'package:freezed_annotation/freezed_annotation.dart';

import '../../typedefs.dart';

part 'genre.freezed.dart';
part 'genre.g.dart';

@freezed
class Genre with _$Genre {
  factory Genre({
    required int id,
    required String name,
  }) = _Genre;

  factory Genre.fromJson(Json json) => _$GenreFromJson(json);
}

```

En él, utiliza un typedef Json = Map<String,dynamic>; Que lo pone en un fichero llamado typedefs.dart colgando de domain.

## Final
A mi me ha quedado así:
```
// ignore_for_file: invalid_annotation_target

import 'package:freezed_annotation/freezed_annotation.dart';

import '../../typedefs.dart';

part 'bien.freezed.dart';
part 'bien.g.dart';

@freezed
class Bien with _$Bien {
  factory Bien({
    @JsonKey(name: 'nombre_bien') required String nombreBien,
    @JsonKey(name: 'monto_bien') required double montoBien,
    @JsonKey(name: 'id_bien') required int idBien,
  }) = _Bien;

  factory Bien.fromJson(Json json) => _$BienFromJson(json);
}

```

He utilizado nombres distintos para los campos de los que vienen en el json, así que lo he especificado con la anotación @JsonKey . Me daba un warning diciendo que no se podía utilizar ahí así lo he omitido por el momento.