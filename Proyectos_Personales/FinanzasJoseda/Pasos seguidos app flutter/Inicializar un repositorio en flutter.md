Para hacerlo correctamente y con clean architecture, habrá que inyectarlo como dependencia al inicio de la aplicación.

Para ello, en el main.dart, el runApp correrá un MultiProvider (suponemos que se van a utilizar más de un provider, si no valdría con el provider normal) y ahí se mete el provider con hijo MyApp:

Todas las dependencias que necesite nuestro repositorio, se instanciarán en la función Main().
## Dependencias:
- [[Librería provider]]
## Código main.dart ANTES:
```
import 'package:flutter/material.dart';
import 'package:front_finanzasjoseda_reestructurado/app/my_app.dart';

void main() {
  runApp(const MyApp());
}

```
## Código main.dart NUEVO:
```
import 'package:flutter/material.dart';
import 'package:http/http.dart';
import 'package:provider/provider.dart';
import 'app/data/http/http.dart';
import 'app/data/repositories_implementation/resumen_financiero_repository_impl.dart';
import 'app/data/services/remote/bienes_api.dart';
import 'app/domain/repositories/resumen_financiero_repository.dart';
import 'app/my_app.dart';

void main() {
  //Dependencia cliente http
  final http = Http(
    client: Client(),
    baseUrl: '10.0.2.2:8000',
  );

  runApp(
    MultiProvider(
      providers: [
        Provider<ResumenFinancieroRepository>(
            create: (_) => ResumenFinancieroRepositoryImpl(
                  BienesAPI(http),
                ))
      ],
      child: const MyApp(),
    ),
  );
}

```