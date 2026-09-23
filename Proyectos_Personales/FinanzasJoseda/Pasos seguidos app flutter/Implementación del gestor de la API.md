Aquí haré la comunicación con la API y la gestión del resultado, así como el mapeado/serializado del objeto json en un objeto válido.

## Dependencias
- Modelo (en este caso el de Bien)
- [[Función handleHttpFailure]]
- [[Clase http_request_failure]]
- [[Librería http]]
- [[Clase either flutter]]
## Ejemplo base:
Este ejemplo tiene también internacionalización que yo no la voy a aplicar.
```
import '../../../domain/either/either.dart';
import '../../../domain/failures/http_request/http_request_failure.dart';
import '../../../domain/models/movie/movie.dart';
import '../../../domain/models/peformer/performer.dart';
import '../../http/http.dart';
import '../local/language_service.dart';
import '../utils/handle_failure.dart';

class MoviesAPI {
  final Http _http;

  MoviesAPI(
    this._http,
    this._languageService,
  );
  final LanguageService _languageService;

  Future<Either<HttpRequestFailure, Movie>> getMovieById(int id) async {
    final result = await _http.request(
      '/movie/$id',
      languageCode: _languageService.languageCode,
      onSuccess: (json) {
        return Movie.fromJson(json);
      },
    );
    return result.when(
      left: handleHttpFailure,
      right: (movie) => Either.right(movie),
    );
  }
}
```

## Final
fichero bienes_api.dart que cuelga de data/services/remote
```
import '../../../domain/either/either.dart';
import '../../../domain/failures/http_request/http_request_failure.dart';
import '../../../domain/models/bien/bien.dart';
import '../../http/http.dart';
import '../utils/handle_failure.dart';

class BienesAPI {
  final Http _http;

  BienesAPI(
    this._http,
  );

  Future<Either<HttpRequestFailure, List<Bien>>> getAllBienes() async {
    final result = await _http.request<List<Bien>>(
      '/get_bienes',
      onSuccess: (json) {
        //Como el json me viene con corchetes, será una lista.
        final List<dynamic> listResp = json.decode(json.body);
        // Formateo la lista a objetos de tipo Bien.
        return listResp.map((e) => Bien.fromJson(e)).toList();
      },
    );
    return result.when(
      left: handleHttpFailure,
      right: (bienes) => Either.right(bienes),
    );
  }
}

```
La única diferencia con el ejemplo es que yo quiero devolver una Lista de Bienes, por lo que le hago un map para pasarlo a lista.