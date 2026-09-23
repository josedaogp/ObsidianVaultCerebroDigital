Una vez creado el repositorio, tendremos que implementarlo.

## Ejemplo:
```
import '../../domain/either/either.dart';
import '../../domain/failures/http_request/http_request_failure.dart';
import '../../domain/models/movie/movie.dart';
import '../../domain/models/peformer/performer.dart';
import '../../domain/repositories/movies_repository.dart';
import '../services/remote/movies_api.dart';

class MoviesRepositoryImpl implements MoviesRepository {
  final MoviesAPI _moviesAPI;

  MoviesRepositoryImpl(this._moviesAPI);

  @override
  Future<Either<HttpRequestFailure, Movie>> getMovieById(
    int id,
  ) {
    return _moviesAPI.getMovieById(id);
  }

  @override
  Future<Either<HttpRequestFailure, List<Performer>>> getCastByMovie(
    int movieId,
  ) {
    return _moviesAPI.getCastByMovie(movieId);
  }
}
```

## Final
Fichero resumen_financiero_repository_impl.dart que cuelga de data/repositories_implementation
```
import '../../domain/either/either.dart';
import '../../domain/failures/http_request/http_request_failure.dart';
import '../../domain/models/bien/bien.dart';
import '../../domain/repositories/resumen_financiero_repository.dart';
import '../services/remote/bienes_api.dart';

class ResumenFinancieroRepositoryImpl implements ResumenFinancieroRepository {
  final BienesAPI _bienesAPI;

  ResumenFinancieroRepositoryImpl(this._bienesAPI);

  @override
  Future<Either<HttpRequestFailure, List<Bien>>> getAllBienes() {
    return _bienesAPI.getAllBienes();
  }
}
```
Necesito la implementación de BienesAPI que contendrá el método getAllBienes y será el próximo paso.

Aquí tendrán que ir todas las llamadas que la pantalla de resumenFinanciero necesite. Voy a empezar por esta por ahora.