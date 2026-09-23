Forma parte de la [[Clase Http]].

```
part of 'http.dart';

dynamic _parseResponseBody(String responseBody) {
  try {
    return jsonDecode(responseBody);
  } catch (_) {
    return responseBody;
  }
}
```