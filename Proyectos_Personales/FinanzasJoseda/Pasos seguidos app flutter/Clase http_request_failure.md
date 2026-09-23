Es una clase autogenerada con [[freezed]] para gestionar los posibles errores que devuelvan las peticiones http.

```
import 'package:freezed_annotation/freezed_annotation.dart';

part 'http_request_failure.freezed.dart';

@freezed
class HttpRequestFailure with _$HttpRequestFailure {
  factory HttpRequestFailure.notFound() = HttpRequestFailureNotFound;
  factory HttpRequestFailure.network() = HttpRequestFailureNetwork;
  factory HttpRequestFailure.unauthorized() = HttpRequestFailureUnauthorized;
  factory HttpRequestFailure.unknown() = HttpRequestFailureUnknown;
}
```

Cuelga de lib/app/domain/failures/http_request