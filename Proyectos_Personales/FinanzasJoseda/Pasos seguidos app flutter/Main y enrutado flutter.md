## Fichero main.dart
Está colgando de lib

```
import 'package:flutter/material.dart';
import 'package:front_finanzasjoseda_reestructurado/app/my_app.dart';

void main() {
  runApp(const MyApp());
}

```

## Fichero my_app.dart
Contiene la definición del primer widget del proyecto, el que generará el materialApp() , las rutas y demás. Cuelga de lib/app

```
import 'package:flutter/material.dart';
import 'package:front_finanzasjoseda_reestructurado/app/presentation/routes/app_routes.dart';
import 'package:front_finanzasjoseda_reestructurado/app/presentation/routes/routes.dart';

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      initialRoute: Routes.resumenFinanciero,
      // onUnknownRoute: (_) => MaterialPageRoute(builder: (_) => Scaffold( body: Center(child: Assets.svgs.error404.svg(),),)),
      routes: appRoutes,
    );
  }
}
```

## Fichero app_routes.dart
Contiene un map instanciando las rutas hacia sus correspondientes widgets.
Cuelga de lib/app/presentation/routes

```
import 'package:flutter/material.dart';
import 'package:front_finanzasjoseda_reestructurado/app/presentation/pages/resumen_financiero/resumen_financiero_page.dart';
import 'package:front_finanzasjoseda_reestructurado/app/presentation/routes/routes.dart';

Map<String, Widget Function(BuildContext)> get appRoutes {

  return {
    Routes.resumenFinanciero: (context) => const ResumenFinancieroPage(),
  };

}
```

## Fichero routes.dart
Donde se definen los nombres de todas las rutas.
Cuelga de lib/app/presentation/routes

```
class Routes {
  Routes._();

  static const resumenFinanciero = '/resumen-financiero';
}
```

