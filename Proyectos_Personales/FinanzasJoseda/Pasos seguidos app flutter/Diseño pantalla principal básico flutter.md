El tercer paso que realicé fue diseñar la pantalla básica para ver qué iba a ir necesitando de la api. Este código aún no está refactorizado ni utiliza datos reales de la api.

## resumen_financiero_page.dart
En concreto es el fichero resumen_financiero_page.dart que cuelga de lib/app/presentation/pages/resumen_financiero

```
import 'package:flutter/material.dart';

class ResumenFinancieroPage extends StatelessWidget {
  const ResumenFinancieroPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        toolbarHeight: 35,
        backgroundColor: Colors.red,
      ),
      body: _ResumenFinancieroBody(),
    );
  }
}

class _ResumenFinancieroBody extends StatefulWidget {
  const _ResumenFinancieroBody({
    super.key,
  });

  @override
  State<_ResumenFinancieroBody> createState() => _ResumenFinancieroBodyState();
}

class _ResumenFinancieroBodyState extends State<_ResumenFinancieroBody> {
  bool _expanded = false;
  @override
  Widget build(BuildContext context) {
    final List<String> listaprueba = [
      "Agosto",
      "Agosto",
      "Agosto",
      "Agosto",
      "Agosto",
      "Agosto",
      "Agosto",
      "Agosto",
      "Agosto",
      "Agosto1",
    ];

    final List<String> categorias = [
      "Agosto",
      "Agosto",
      "Agosto",
      "Agosto",
      "Agosto",
    ];

    return ListView(
      children: [
        _FilaListaMeses(
            //Este lo quiero fijo aunque haga scroll
            listaCategorias: categorias,
            listaprueba: listaprueba),
        // ListView.builder(
        //     physics: AlwaysScrollableScrollPhysics(),
        //     shrinkWrap: true,
        //     itemCount: 30,
        //     itemBuilder:
        // (BuildContext context, int index) {
        for (int i = 0; i < categorias.length; i++)
          Padding(
            //ESTE WIDGET ES EL QUE QUIERO MOSTRAR DINÁMICAMENTE
            padding: const EdgeInsets.all(8.0),
            child: Center(
              child: GestureDetector(
                onTap: () {
                  setState(() {
                    _expanded = !_expanded; // Cambiar el estado al hacer clic
                  });
                },
                child: AnimatedContainer(
                  decoration: BoxDecoration(
                    color: Colors.red[100],
                    borderRadius: BorderRadius.circular(10),
                  ),
                  duration: const Duration(
                      milliseconds: 100), // Duración de la animación
                  width: MediaQuery.of(context).size.width,
                  height: _expanded ? 200 : 100, // Altura condicional
                  child: Row(
                    children: [
                      Container(
                        height: 100,
                        decoration: BoxDecoration(
                          color: Colors.red,
                          borderRadius: BorderRadius.circular(10),
                        ),
                        child: const Center(
                          child: Padding(
                            padding: const EdgeInsets.all(8.0),
                            child: Text(
                              // '${categorias[i].nombreCategoria}',
                              'Hola',
                              style: const TextStyle(color: Colors.black),
                            ),
                          ),
                        ),
                      ),
                      Expanded(
                        child: Column(
                          children: [
                            Padding(
                              padding: const EdgeInsets.all(8.0),
                              child: Container(
                                decoration: BoxDecoration(
                                  color: Colors.blue,
                                  borderRadius: BorderRadius.circular(10),
                                ),
                                height: 34,
                                child: const Center(
                                  child: Text(
                                    'Presupuesto: 50€',
                                    style: TextStyle(color: Colors.black),
                                  ),
                                ),
                              ),
                            ),
                            Padding(
                              padding: const EdgeInsets.all(8.0),
                              child: Container(
                                height: 34,
                                decoration: BoxDecoration(
                                  color: Colors.blue,
                                  borderRadius: BorderRadius.circular(10),
                                ),
                                child: const Center(
                                  child: Text(
                                    'Gasto: 30€',
                                    style: TextStyle(color: Colors.black),
                                  ),
                                ),
                              ),
                            ),
                            if (_expanded)
                              Padding(
                                padding: const EdgeInsets.all(8.0),
                                child: Container(
                                  height: 84,
                                  decoration: BoxDecoration(
                                    color: Colors.green,
                                    borderRadius: BorderRadius.circular(10),
                                  ),
                                  child: const Center(
                                    child: Text(
                                      'Introducir gasto:',
                                      style: TextStyle(color: Colors.black),
                                    ),
                                  ),
                                ),
                              ),
                          ],
                        ),
                      ),
                    ],
                  ),
                ),
              ),
            ),
          )
        // }
        // ),
      ],
    );
  }
}

class _FilaListaMeses extends StatelessWidget {
  const _FilaListaMeses({
    super.key,
    required this.listaCategorias,
    required this.listaprueba,
  });

  final List<String> listaCategorias;
  final List<String> listaprueba;

  @override
  Widget build(BuildContext context) {
    return SingleChildScrollView(
      scrollDirection: Axis.horizontal,
      reverse: true,
      child: Row(
        children: [
          Row(
            children: List.generate(listaCategorias.length, (index) {
              if (index == listaprueba.length - 1) {
                return Padding(
                  padding: const EdgeInsets.all(4.0),
                  child: ElevatedButton(
                    style: ButtonStyle(
                      backgroundColor:
                          MaterialStatePropertyAll(Colors.indigo[200]),
                    ),
                    onPressed: () {},
                    child: const Text('+'),
                  ),
                );
              } else {
                return Padding(
                  padding: const EdgeInsets.all(4.0),
                  child: ElevatedButton(
                    style: ButtonStyle(
                      backgroundColor:
                          MaterialStatePropertyAll(Colors.indigo[100]),
                    ),
                    onPressed: () {},
                    child: const Text('Agosto 23'),
                  ),
                );
              }
            }),
          ),
        ],
      ),
    );
  }
}

```

El resultado sería algo así:
![[Pasted image 20240515122904.png]]