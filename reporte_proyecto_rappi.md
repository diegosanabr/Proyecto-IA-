# Reporte de análisis del dataset de entrega de Rappi

## 1. Objetivo del estudio

El problema es predecir el tiempo de entrega de un pedido en función de variables observables antes de confirmar la promesa de entrega. En este tipo de dataset, la variable más importante es el tiempo real en minutos, pero el valor útil surge de estudiar qué variables operativas lo explican de forma más clara: distancia, tráfico, clima, zona, tipo de pedido, condiciones del repartidor y horario.

El análisis debe enfocarse en variables que estén disponibles antes del envío y que tengan sentido causal para la demora. En cambio, IDs, nombres, detalles del cliente o columnas sin variación real deben descartarse para evitar ruido y posible sobreajuste.

## 2. Variables más relevantes para el objetivo

Las columnas más importantes normalmente son:

- Distancia estimada del restaurante al destino.
- Densidad o nivel de tráfico.
- Clima o condiciones ambientales.
- Ciudad, zona o barrio.
- Tipo de pedido o modalidad operacional.
- Horario de pedido, recogida o entrega.
- Condición del vehículo y perfil del repartidor.
- Cantidad de entregas asociadas al mismo repartidor.

Con respecto al objetivo, estas variables tienen una relación directa o indirecta con el tiempo. En general:

- La distancia tiende a aumentar el tiempo de entrega.
- El tráfico alto suele prolongar los tiempos de forma significativa.
- El clima adverso o la congestión puede aumentar el tiempo de recorrido y de espera.
- La zona y la hora del día tienen impacto por patrones de tráfico y demanda.
- El perfil del repartidor y la condición del vehículo influyen en la velocidad de operación y la capacidad de respuesta.

## 3. Variables que se deben filtrar o descartar

Antes de entrenar un modelo, conviene descartar:

- IDs del pedido, restaurante, repartidor o cliente.
- Nombres, direcciones o claves internas.
- Columnas con demasiada cardinalidad y poca semántica predictiva.
- Variables que solo se conocen después de la entrega y no sirven para predecirla.

En otras palabras, el modelo no debe aprender a memorizar identificadores ni usar información que no estaba disponible en el momento de planear la entrega.

## 4. Análisis de sesgo y desbalance

### 4.1 Sesgo por segmentación operativa

El dataset puede estar sesgado si:

- una ciudad o zona concentra la mayor parte de los pedidos;
- el tráfico elevado aparece solo en algunos horarios;
- la mayoría de los registros vienen de un tipo de pedido o un perfil de repartidor;
- las entregas complejas o de larga distancia están subrepresentadas.

Este tipo de sesgo no invalida el dataset, pero sí puede empujar al modelo a aprender mejor los casos más frecuentes y peor los casos extremos, que suelen ser los más relevantes para la operación.

### 4.2 Desbalance en problemas de clasificación

Si el problema se transforma en clasificación, por ejemplo:

- entrega rápida / normal / lenta;
- entrega tardía vs no tardía;
- calificación baja vs no baja;

entonces sí puede existir desbalance entre clases. En ese caso, un conjunto con una clase minoritaria muy pequeña puede hacer que el modelo pase por alto situaciones importantes, especialmente las tardías o de riesgo operativo.

Regla práctica:

- Si la clase minoritaria está por debajo de 10%, hay un desbalance fuerte.
- Entre 10% y 20%, existe desbalance moderado.
- Por encima de 20%, generalmente es manejable.

### 4.3 Diferencia clave: regresión vs clasificación

La variable objetivo ideal para este proyecto es el tiempo real en minutos, que es una variable continua. En este caso, no hablamos de “clases” sino de una distribución. El riesgo es más bien:

- concentración de tiempos en un rango muy estrecho;
- pocos casos atípicos con entrega muy lenta;
- sesgo por tráfico, distancia o zona.

Por eso, para regresión, la evaluación debe centrarse en:

- mediana y media del tiempo;
- histogramas por rango;
- errores por segmento (por tráfico, ciudad, distancia);
- MAE, RMSE y R².

## 5. Recomendación técnica

Lo recomendable es usar como objetivo principal la columna de tiempo en minutos y, en una segunda capa, transformar la predicción en una clasificación operativa si el negocio lo requiere.

La estrategia más sólida es:

1. Filtrar columnas no útiles y identificadores.
2. Enriquecer variables con distancia, horario y agregados por zona.
3. Revisar la distribución del tiempo por tráfico, ciudad y clima.
4. Entrenar un modelo de regresión con métricas de error.
5. Si hace falta una decisión operativa, convertir a clase de “entrega lenta” y evaluar con métricas por clase.

## 6. Conclusión

El dataset tiene potencial real para predecir tiempos de entrega, pero ese potencial depende de la calidad de la selección de columnas y de entender el sesgo operativo del problema. El mayor valor no está en todas las columnas, sino en las variables que reflejan la realidad del recorrido: distancia, tráfico, clima, zona y condiciones de operación.

El análisis más útil antes del entrenamiento es revisar la correlación, la distribución del objetivo y el comportamiento por segmentos, porque ahí se ve si el dataset está equilibrado para el caso real o si se está inclinando hacia tipos de pedidos más frecuentes y más simples de entregar.

