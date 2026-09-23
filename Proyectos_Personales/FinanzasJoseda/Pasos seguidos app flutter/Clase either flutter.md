El cuarto paso fue preparar todo para hacer la primera llamada a la api. Para ello, empezaremos por la capa de dominio, añadiendo la clase either que necesitaremos para manejar las respuestas ok y los errores.

## Necesitaremos:
### Importar:
- [[build_runner]] --> Para ejecutar comandos de freezed, json serializable y otros
- [[freezed]] --> dev_dependencies
- [[freezed_annotations]]
- equatable (no lo usaremos todavía pero sí en el futuro)
- [[json_serializable]] --> dev_dependencies
- [[json_annotation]]
### Extensiones vscode 
#### [[flutter freezed helpers]]
#### [[Command Runner]]

## either.dart
Cuelga de lib/app/domain/either

```
import 'package:freezed_annotation/freezed_annotation.dart';

part 'either.freezed.dart';

@freezed
class Either<L, R> with _$Either<L, R> {
  factory Either.left(L value) = Left;
  factory Either.right(R value) = Right;
}

```