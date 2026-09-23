Descargar de pub.dev y más info: https://pub.dev/packages/freezed

## Dependencias
- [[build_runner]] --> para crear los ficheros autogenerados.
- [[json_serializable]] --> para crear la serialización de la clase (opcional si no vamos a querer serializar y solo lo queremos utilizar para el resto de usos).
- freezed_annotations

## Info
Con freezed podremos acortar mucho código, ya que con una simple anotación y el posterior build_runner build podremos generar los constructores con todas las propiedades de una clase, sobreescribir el toString, operador == y el hashCode.
Además implementa el copyWith y junto a jsonSerializable genera la serialización de la clase como objeto, y el método when para poder utilizar programación funcinal.

Lo podemos utilizar para la definición de los estados de la aplicación o para los modelos de datos que recogerán la información de la api.

## Extra
Se puede conseguir el funcionamiento de un Union Type en flutter utilizando freezed.
## Fichero autogenerado
Añade .freezed antes del .dart. Por ejemplo para la clase user.dart, generará user.freezed.dart .

## Uso
- Arriba de la clase: @freezed
- Freezed utiliza mixins así que tendremos que entender la clase de \_$NombreDeLaClase
- Todas las propiedades se definen en el propio constructor de la clase, que deberá ser un factory constructor. 
- Al final del constructor, añadir = \_NombreDeLaClase

## Ejemplo simple freezed con clase de estado de la aplicación:
```
part 'sign_in_state.freezed.dart';

@freezed
class SignInState with _$SignInState {
  const factory SignInState({
    @Default('') String username,
    @Default('') String password,
    @Default(false) bool fetching,
  }) = _SignInState;
}
```
*Nota: La anotación @Default() se utiliza para definir el valor por defecto de la propiedad. **Si no se pone el defautl, habrá que añadir required***
## Ejemplo junto a json_serializable:
```
part 'movie.freezed.dart';
part 'movie.g.dart';

@freezed
class Movie with _$Movie {
  const factory Movie({
    required int id,
    required List<Genre> genres,
    required String overview,
    required int runtime,
    @JsonKey(name: 'poster_path') required String posterPath,
    @JsonKey(name: 'release_date') required DateTime releaseDate,
    @JsonKey(name: 'vote_average') required double voteAverage,
    @JsonKey(readValue: readTitleValue) required String title,
    @JsonKey(readValue: readOriginalTitleValue) required String originalTitle,
    @JsonKey(name: 'backdrop_path') required String? backdropPath,
  }) = _Movie;
  const Movie._();

  factory Movie.fromJson(Json json) => _$MovieFromJson(json);

  Media toMedia() {
    return Media(
      id: id,
      overview: overview,
      title: title,
      originalTitle: originalTitle,
      posterPath: posterPath,
      backdropPath: backdropPath,
      voteAverage: voteAverage,
      type: MediaType.movie,
    );
  }
}
```

## Ejemplo para crear UnionTypes o Sealed Clases con freezed:
```
part 'http_request_failure.freezed.dart';

@freezed
class HttpRequestFailure with _$HttpRequestFailure {
  factory HttpRequestFailure.notFound() = HttpRequestFailureNotFound;
  factory HttpRequestFailure.network() = HttpRequestFailureNetwork;
  factory HttpRequestFailure.unauthorized() = HttpRequestFailureUnauthorized;
  factory HttpRequestFailure.unknown() = HttpRequestFailureUnknown;
}
```
Donde HttpRequestFailureNotFound y el resto, serán clases que autogenere freezed con su propio constructor y demás.

Luego si quiero comprobar de qué tipo es alguna propiedad, podré utilizar:

propiedadX.when() y ahí autocompletará todos los casos.

A los factoryConstructors se les puede pasar algún parámetro (por ejemplo en la clase either). --> Esto se explica en [[Flutter desde cero - Guía completa con arquitectura limpia]] capítulo 326. Freezed - parte 7.