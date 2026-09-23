## Descripción del Video

En este video se aborda la implementación de lógica y condiciones dentro de flujos de trabajo automatizados. Se presentan diferentes tipos de nodos que permiten comparar datos, aplicar condicionales, crear bucles, manejar errores y temporizar acciones. Se explica cómo utilizar estos nodos a través de ejemplos prácticos, como la comparación de hojas de Google para identificar diferencias y enviar notificaciones. El objetivo es que los usuarios comprendan cómo introducir lógica avanzada en sus flujos de trabajo para mejorar la eficiencia y precisión de las automatizaciones.

## Temas del Video

- Introducción
    - Importancia de la lógica en flujos de trabajo automatizados
- Tipos de nodos lógicos
- Ejemplos prácticos
    - Comparación de dos hojas de Google Sheets
    - Identificación y notificación de diferencias entre conjuntos de datos
    - Uso de bucles para procesar elementos de una lista
    - Envío de mensajes condicionales mediante Telegram
    - Implementación de esperas y temporizadores en flujos de trabajo
- Conclusión y próximo módulo
    - Resumen de la lógica aplicada
    - Avance sobre casos prácticos y específicos en próximos videos

## Nodos lógicos de n8n
- **Compare Data Set node**
    - Comparación de conjuntos de datos y detección de diferencias
    - *Habrá que coger el campo de cada hoja de excel, para su ejemplo.
    - *Hay que indicarle dónde están las diferencias (cuál va a comparar con cuál)**
    - *Tiene cuatro ramas: *
	    - *In A only branch --> Te saca SOLO los elementos que solo están en el primer input*
	    - *In B only branch --> Te saca SOLO los elementos que solo están en el segundo input*
	    - *Same Branch --> Saca SOLO los elementos que son iguales en ambos input*
	    - *Different Branch --> Saca SOLO los elementos que son DISTINTOS en ambos input*
- **If node**
    - Aplicación de condicionales simples
- **Loop Over Items node**
    - Creación de bucles para iterar sobre listas
- **Switch node**
    - Aplicación de múltiples condiciones en paralelo
- **Wait node**
    - Temporización de acciones (espera de segundos a días)
- **Error node**
    - Manejo y personalización de errores en el flujo de trabajo

---

## Recursos

Plantilla usada

[Fechas_en_un_archivo_con_l_gica.json.zip](https://media2-production.mightynetworks.com/asset/2988d994-3cb9-4a65-b1a6-45ebcc00a5b8/Fechas_en_un_archivo_con_l_gica.json.zip "Fechas_en_un_archivo_con_l_gica.json.zip")

---

Transcripción del vídeo

[logica y condiciones.zip](https://media2-production.mightynetworks.com/asset/5a7f2503-ff2e-495d-bec8-2c263c3f3edb/logica_y_condiciones.zip "logica y condiciones.zip")