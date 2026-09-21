# Planteamiento del proyecto: predicción del tiempo de entrega en Rappi

## 1. Problema a resolver

El problema central consiste en predecir el tiempo total de entrega de un pedido en minutos, a partir de variables observables antes de que la entrega se concrete.

En un contexto real de delivery, el tiempo de entrega no depende solo de la distancia entre el restaurante y el cliente. También intervienen factores como:

- nivel de tráfico,
- zona de recogida y entrega,
- condiciones climáticas,
- tipo de vehículo,
- experiencia del repartidor,
- tiempo de preparación del pedido,
- prioridad del servicio,
- velocidad promedio del recorrido,
- entre otras condiciones operativas.

El objetivo es estimar, con la mayor precisión posible, cuánto tiempo tardará en llegar un pedido, para poder:

- mejorar la experiencia del cliente,
- gestionar mejor la operación logística,
- optimizar tiempos de promesa de entrega,
- reducir retrasos,
- anticipar cuellos de botella operativos,
- apoyar decisiones de asignación de pedidos y repartidores.

---

## 2. Variable objetivo

La variable objetivo es el tiempo de entrega, generalmente representado como:

- Time_taken(min)
- o una versión equivalente del tiempo total en minutos.

Esta variable es la que queremos predecir y se construye como el tiempo real que requiere completar la entrega desde que el pedido se registra hasta que llega al cliente.

Por lo tanto, el problema es de tipo supervisado y, en este caso, de regresión, porque la variable objetivo es continua y numérica.

---

## 3. ¿Por qué es necesario entrenar un modelo de inteligencia artificial?

### 3.1. Porque el problema no es una simple regla matemática

Una regla matemática directa podría funcionar solo cuando existen relaciones muy simples y deterministas, por ejemplo:

- tiempo estimado = distancia / velocidad promedio,
- o una fórmula fija por zona.

Sin embargo, en la práctica el tiempo de entrega depende de múltiples factores simultáneos y altamente variables:

- la distancia no siempre explica exactamente la demora,
- el tráfico puede cambiar mucho en minutos,
- el clima puede reducir la velocidad de conducción,
- la zona de entrega puede afectar la accesibilidad,
- la prioridad del pedido puede cambiar la estrategia de operación,
- la experiencia del repartidor puede influir en la ruta y en la velocidad real.

Esto hace que la relación entre las variables y el tiempo de entrega no pueda representarse de manera rígida con una sola ecuación fija.

El comportamiento real del sistema es complejo, no lineal y con interacciones entre variables. Por ejemplo:

- 10 km en una zona congestionada pueden tardar más que 20 km en una zona libre,
- una zona con buen tráfico y un repartidor experimentado puede ser mucho más eficiente que otra con igual distancia pero peor circulación,
- una entrega con prioridad alta puede requerir rutas y tiempos distintos de una entrega normal.

Por eso, un modelo de machine learning o inteligencia artificial es necesario para capturar estas relaciones no lineales y combinadas.

### 3.2. Porque el problema tiene patrones complejos

El comportamiento del tiempo de entrega se basa en la intersección de varios fenómenos, como:

- operación logística,
- comportamiento del tráfico,
- condiciones ambientales,
- decisiones del repartidor,
- características del pedido,
- variabilidad por zona u horario.

Estos patrones suelen ser difíciles de modelar con reglas manuales, porque no siempre siguen una fórmula exacta ni una lógica lineal simple.

Un modelo de IA puede aprender automáticamente patrones de datos históricos y generalizar a nuevas entregas.

### 3.3. Porque el valor real está en la predicción

El objetivo no es solo describir el problema, sino anticipar resultados. En un sistema de delivery, una buena predicción del tiempo de entrega permite:

- decidir si el pedido puede cumplirse en tiempo,
- estimar la promesa de entrega de forma más realista,
- priorizar pedidos urgentes,
- alertar sobre entregas en riesgo,
- reducir frustración del cliente,
- mejorar la eficiencia operativa.

Esto no se resuelve con una regla estática, sino con un modelo que aprenda de datos reales y que pueda adaptarse a distintos escenarios.

---

## 4. ¿Por qué no basta con una fórmula matemática simple?

Una regla matemática podría usarse como una primera aproximación, por ejemplo:

- tiempo estimado = distancia / velocidad base,
- o tiempo estimado = distancia × factor de zona.

Pero esta estrategia presenta varias limitaciones:

1. No considera el tráfico real.
2. No captura diferencias entre zonas urbanas o suburbanas.
3. No incorpora clima, prioridad ni experiencia del repartidor.
4. No toma en cuenta interacciones complejas entre variables.
5. No se ajusta bien cuando el entorno cambia con el tiempo.
6. No generaliza bien a situaciones no observadas anteriormente.

En otras palabras, una regla fija parte de supuestos simplificados, mientras que el problema real es dinámico, multidimensional y cambiante.

El aprendizaje automático aporta capacidad para aprender relaciones de la forma:

- distancia + tráfico + clima + zona + velocidad + prioridad,

y producir una estimación más realista y útil para la operación.

---

## 5. Origen de los datos

Los datos provienen de un dataset de predicción de tiempo de entrega de comida, orientado a escenarios de operación logística urbana. El conjunto incluye información de pedidos, rutas, variables de tránsito, condiciones del entorno y rendimiento del proceso de entrega.

El dataset se usa como base para modelar el tiempo real de entrega y relacionarlo con factores operativos visibles antes de la entrega.

### Características generales del dataset

El dataset incluye información de tipo:

- operativo: distancia, tiempo de preparación, velocidad promedio,
- geográfico: zona de recogida y entrega,
- contextual: clima y presión del tráfico,
- del repartidor: experiencia y tipo de vehículo,
- del pedido: prioridad, categoría de distancia, complejidad operativa,
- objetivo: tiempo real de entrega en minutos.

Esto lo convierte en un conjunto apropiado para un problema de regresión con variables predictoras heterogéneas, tanto numéricas como categóricas.

---

## 6. Características importantes del dataset

Algunas de las características más relevantes son:

- la variable objetivo es continua,
- existen variables numéricas y categóricas,
- hay factores que tienen relación directa con el tiempo de entrega,
- algunas variables pueden estar más correlacionadas que otras,
- es posible detectar patrones por zona, tráfico, clima y distancia,
- la variable objetivo puede presentar sesgo o valores extremos,
- los valores atípicos pueden representar entregas complejas y no necesariamente errores.

Estas características hacen que el conjunto sea interesante para evaluar modelos predictivos, comparar variables relevantes y entender mejor el comportamiento del servicio.

---

## 7. Qué se espera del proyecto

Se espera construir un flujo de análisis que permita:

1. explorar y entender el dataset,
2. identificar las variables más relevantes,
3. analizar la distribución de la variable objetivo,
4. detectar outliers y observar su impacto,
5. evaluar la relación entre el tiempo de entrega y factores como distancia, tráfico y velocidad,
6. entrenar un modelo de IA para predecir el tiempo de entrega,
7. validar la calidad de la predicción con métricas adecuadas,
8. y obtener una herramienta útil para apoyar decisiones de operación.

---

## 8. Conclusión

El problema de predecir el tiempo de entrega en Rappi no puede resolverse solo con una fórmula matemática fija porque la operación real depende de múltiples variables interrelacionadas y dinámicas. El tiempo de entrega es resultado de condiciones logísticas, ambientales y de operación que cambian por caso, por zona y por momento.

Por eso, entrenar un modelo de inteligencia artificial es necesario para aprender patrones históricos, detectar relaciones complejas y producir predicciones útiles para la operación. El dataset tiene la estructura adecuada para este tipo de análisis y la variable objetivo es clara: estimar el tiempo real de entrega en minutos con base en variables observables antes de la entrega.

Este proyecto, por tanto, no solo busca predecir un número, sino entender el comportamiento operativo del servicio y usar esa información para mejorar la experiencia del cliente y la eficiencia de la entrega.
