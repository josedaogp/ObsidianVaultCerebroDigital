## Aprendizaje batch
Los sistemas basados en aprendizaje batch **no aprenden de manera incremental**, se entrenan utilizando **todos** los datos disponibles.
### Características del aprendizaje batch
- Si se desesa que el sistema se adapte a un nuevo tipo de dato, se debe entrenar de nuevo con todos los datos disponibles
- Solución sencillla --> Ideal cuando los outputs no suelen cambiar.
- Funciona bien para sistemas que no requieren un conjunto de datos muy grande ni adaptarse a nuevos datos de manera muy rápida. Ya que, si hay que volver a entrenar el modelo, sería muy costoso de nuevo.
- Muy restringido para dispositivos con una capacidad limitada de recursos, como un smartphone.

## Aprendizaje online
Los sistemas basados en aprendizaje online se entrenan incrementalmente, mediante consumo incremental de datos, ya sea indivuales o en pequeños grupos (mini-batches).

Primero, ajustará un poco la función hipótesis con los primeros datos que le pasamos para el entrenamiento. Se irán metiendo más datos al entrenamiento si la salida no es la esperada. Así podemos ir mejorando el modelo conforme vayamos teniendo nuevos datos. Los datos para el entrenamiento también se pueden proporcionar en tiempo real, pero hay que asegurar que esos datos son de calidad y no tienen ruido, ya que podrían degradar el rendimiento de la función hipótesis y del sistema en sí.
### Características del aprendizaje online
- Solución ideal para sistemas que reciben datos continuamente y requieren adaptarse a ellos de manera rápida
- Es capaz de lidiar con grandes conjuntos de datos que puede que no entren en una sola máquina
- Aparecen algunas variables importantes que hay que determinar, como el ratio de aprendizaje
- Puede ser muy inestables si por alguna razón se consumen datos de baja calidad.