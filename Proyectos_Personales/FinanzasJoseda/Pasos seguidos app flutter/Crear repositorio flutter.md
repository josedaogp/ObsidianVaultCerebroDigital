En principio voy a crear un único repositorio que proveerá la información correspondiente a la pantalla resumen_financiero y que se llame resumen_financiero_repository.dart
En un futuro pensaré en refactorizarlo.

Me he basado en este ejemplo:
```
import '../either/either.dart';
import '../failures/http_request/http_request_failure.dart';
import '../models/movie/movie.dart';
import '../models/peformer/performer.dart';

abstract class MoviesRepository {
  Future<Either<HttpRequestFailure, Movie>> getMovieById(int id);
  Future<Either<HttpRequestFailure, List<Performer>>> getCastByMovie(
    int movieId,
  );
}
```

Me ha quedado de momento así:
```
import '../either/either.dart';
import '../failures/http_request/http_request_failure.dart';
import '../models/bien/bien.dart';

abstract class ResumenFinancieroRepository {
  Future<Either<HttpRequestFailure, List<Bien>>> getAllBienes();
}
```
Por lo pronto voy a probar a traerme un Bien. El próximo paso será crear el modelo de Bien.