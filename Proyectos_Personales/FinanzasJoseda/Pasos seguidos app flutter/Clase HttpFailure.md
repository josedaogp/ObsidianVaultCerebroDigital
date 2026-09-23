Fichero http_failure.dart que cuelga de /data/http

Símplemente contiene la información que podemos obtener al hacer una petición a una api por http.

```
part of 'http.dart';

class HttpFailure {
  HttpFailure({
    this.statusCode,
    this.exception,
    this.data,
  });

  final int? statusCode;
  final Object? exception;
  final Object? data;
}

class NetworkException {}
```