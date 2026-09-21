# Reporte de análisis por variable del dataset de delivery Rappi

## 1. Objetivo del análisis

El objetivo principal es entender cada variable del dataset para saber qué información aporta, cómo se relaciona con el tiempo de entrega y qué tipo de gráfica sería útil para analizarla por separado.

La variable de interés es el tiempo total de entrega, normalmente representado como `Time_taken(min)`. La idea es identificar cuáles factores observables antes de la entrega más influyen en este valor para poder construir un modelo de predicción más sólido y interpretable.

---

## 2. Resumen del dataset

El dataset contiene información de pedidos de comida, con variables tanto operativas como contextuales. En términos generales, las columnas se pueden agrupar en:

- Información del pedido y la zona.
- Condiciones del viaje: distancia, tráfico y señales.
- Condiciones del repartidor y vehículo.
- Variables contextuales: clima y prioridad del pedido.
- Variable objetivo: tiempo real de entrega.

En la práctica, las variables más importantes para predecir el tiempo de entrega suelen ser:

- distancia de recorrido,
- tráfico,
- clima,
- zona de recogida y entrega,
- tiempo de preparación,
- tipo de vehículo,
- experiencia del repartidor,
- velocidad promedio.

---

## 3. Análisis variable por variable

### 3.1. Order_ID

- ¿A qué se refiere?: identificador único del pedido.
- ¿Qué datos contiene?: un código numérico o alfanumérico que distingue cada registro.
- ¿Cómo ayuda a predecir el tiempo de entrega?: prácticamente no. Es una variable de identidad, no tiene relación causal con la demora real.
- Variables más relacionadas: no aporta valor predictivo útil; en una metodología correcta suele excluirse.
- Gráfica recomendada: no se recomienda para análisis predictivo; solo se usa para validaciones de duplicados o control de integridad.

Observación: esta variable se usa más para trazabilidad operativa que para modelado. Si se usa sin transformar, puede provocar sobreajuste o ruido.

---

### 3.2. Weather

- ¿A qué se refiere?: condiciones climáticas durante la entrega.
- ¿Qué datos contiene?: categorías como soleado, nublado, lluvioso, con viento o con condiciones adversas.
- ¿Cómo ayuda a predecir el tiempo de entrega?: afecta la velocidad de conducción, la visibilidad y la probabilidad de retrasos por lluvia, tráfico o zonas más lentas.
- Tres variables más relacionadas:
  1. Traffic_Level
  2. Road_Distance_km
  3. Average_Speed_kmph
- Gráfica recomendada: boxplot o violin plot del tiempo de entrega por clima; además, un bar chart de tiempo promedio por categoría climática.

Interpretación esperada: días con lluvia o clima adverso suelen implicar más tiempo en tránsito y menor velocidad promedio.

---

### 3.3. Pickup_Zone

- ¿A qué se refiere?: zona o sector desde donde se recoge el pedido.
- ¿Qué datos contiene?: nombre de barrio, área urbana o zona logística de origen.
- ¿Cómo ayuda a predecir el tiempo de entrega?: refleja la dificultad operativa del punto de origen, la congestión local y la distancia hacia la zona de destino.
- Tres variables más relacionadas:
  1. Dropoff_Zone
  2. Road_Distance_km
  3. Traffic_Level
- Gráfica recomendada: boxplot de tiempo por zona; mapa de calor de tiempos promedio por zona; análisis por zona versus tráfico.

Interpretación esperada: algunas zonas tienen más congestión, mejores accesos o más distancia efectiva al destino, por lo que se reflejan en tiempos más altos.

---

### 3.4. Dropoff_Zone

- ¿A qué se refiere?: zona o sector donde se entrega el pedido.
- ¿Qué datos contiene?: nombre del barrio, comuna o distrito de destino.
- ¿Cómo ayuda a predecir el tiempo de entrega?: es una de las variables más directas para explicar la complejidad del último tramo del recorrido, especialmente si el destino está en barrios congestionados o de difícil acceso.
- Tres variables más relacionadas:
  1. Pickup_Zone
  2. Road_Distance_km
  3. Traffic_Level
- Gráfica recomendada: boxplot por zona de entrega; barplot de tiempos promedio por destino; scatter con distancia por zona.

Interpretación esperada: la demora puede aumentar cuando la entrega se hace en zonas densas, de baja accesibilidad o con alta congestión del tránsito.

---

### 3.5. Vehicle_Type

- ¿A qué se refiere?: tipo de vehículo utilizado por el repartidor.
- ¿Qué datos contiene?: categorías como moto, scooter, bicicleta, auto o vehículo de servicio.
- ¿Cómo ayuda a predecir el tiempo de entrega?: define la capacidad de movilidad, la velocidad promedio y la facilidad para recorrer rutas complejas o zonas densas.
- Tres variables más relacionadas:
  1. Average_Speed_kmph
  2. Road_Distance_km
  3. Traffic_Level
- Gráfica recomendada: boxplot del tiempo de entrega por tipo de vehículo; scatter de velocidad promedio versus tiempo.

Interpretación esperada: un vehículo más rápido o más adaptable puede reducir el tiempo total, aunque el tráfico y la distancia siguen siendo factores dominantes.

---

### 3.6. Rider_Experience_Years

- ¿A qué se refiere?: años de experiencia del repartidor.
- ¿Qué datos contiene?: número continuo de años de experiencia.
- ¿Cómo ayuda a predecir el tiempo de entrega?: influye en la capacidad para elegir rutas rápidas, manejar condiciones complejas y resolver obstáculos en camino.
- Tres variables más relacionadas:
  1. Average_Speed_kmph
  2. Vehicle_Type
  3. Preparation_Time_Min
- Gráfica recomendada: scatter plot de experiencia vs tiempo, y boxplot por rangos de experiencia.

Interpretación esperada: el tiempo puede disminuir con mayor experiencia, pero no siempre de forma lineal; la experiencia suele interactuar con el tráfico, la zona y la velocidad promedio.

---

### 3.7. Rider_Rating

- ¿A qué se refiere?: calificación del repartidor según la experiencia del cliente o el sistema.
- ¿Qué datos contiene?: puntuación, generalmente de 1 a 5 o en escala similar.
- ¿Cómo ayuda a predecir el tiempo de entrega?: puede reflejar desempeño general, pero no es una causa directa del tiempo; más bien es una señal de calidad operativa.
- Tres variables más relacionadas:
  1. Rider_Experience_Years
  2. Vehicle_Type
  3. Average_Speed_kmph
- Gráfica recomendada: boxplot de tiempo por rango de calificación; scatter de rating vs velocidad promedio.

Interpretación esperada: un repartidor mejor calificado suele tener tiempos más consistentes, pero esta relación puede estar muy mezclada con experiencia y condiciones del viaje.

---

### 3.8. Preparation_Time_Min

- ¿A qué se refiere?: tiempo que tarda el restaurante o el pedido en prepararse antes de salir para entrega.
- ¿Qué datos contiene?: minutos de preparación del pedido, típicamente en enteros.
- ¿Cómo ayuda a predecir el tiempo de entrega?: no determina el tiempo de recorrido, pero sí añade un componente de espera que aumenta el tiempo total desde el pedido hasta la entrega.
- Tres variables más relacionadas:
  1. Road_Distance_km
  2. Traffic_Level
  3. Delivery_Priority
- Gráfica recomendada: scatter plot y boxplot por tipo de pedido o zona; histograma del tiempo de preparación.

Interpretación esperada: si el restaurante tarda más en preparar, el pedido sale más tarde, lo que puede agravar el tiempo total percibido.

---

### 3.9. Road_Distance_km

- ¿A qué se refiere?: distancia real o estimada en kilómetros entre la zona de origen y la zona de destino.
- ¿Qué datos contiene?: valores numéricos continuos en kilómetros.
- ¿Cómo ayuda a predecir el tiempo de entrega?: es probablemente la variable más directa para explicar la duración del viaje. Cuanto mayor es la distancia, más tiempo suele requerir el reparto.
- Tres variables más relacionadas:
  1. Traffic_Level
  2. Average_Speed_kmph
  3. Delivery_Distance_Category
- Gráfica recomendada: scatter plot de distancia vs tiempo de entrega; línea de tendencia; boxplot por categoría de distancia.

Interpretación esperada: la relación es positiva y suele ser una de las más claras. Sin embargo, el tráfico puede elevar el tiempo aún más que la distancia sola.

---

### 3.10. Delivery_Distance_Category

- ¿A qué se refiere?: clasificación de la distancia en rangos, por ejemplo corto, medio o largo.
- ¿Qué datos contiene?: categorías ordinales basadas en la distancia efectiva.
- ¿Cómo ayuda a predecir el tiempo de entrega?: simplifica la interpretación y permite comparar grupos comparables sin depender solo de valores numéricos.
- Tres variables más relacionadas:
  1. Road_Distance_km
  2. Traffic_Level
  3. Average_Speed_kmph
- Gráfica recomendada: boxplot del tiempo por categoría de distancia; barplot de promedio por categoría.

Interpretación esperada: las entregas largas tienen tiempos más altos y mayor variabilidad, especialmente si se combinan con tráfico intenso.

---

### 3.11. Traffic_Level

- ¿A qué se refiere?: intensidad del tráfico en la ruta.
- ¿Qué datos contiene?: escalas categóricas como Low, Medium, High o Very High.
- ¿Cómo ayuda a predecir el tiempo de entrega?: es una de las variables más influyentes sobre la demora real. Aumenta el tiempo por congestión, paradas y menor velocidad media.
- Tres variables más relacionadas:
  1. Average_Speed_kmph
  2. Road_Distance_km
  3. Weather
- Gráfica recomendada: boxplot de tiempo por nivel de tráfico; violin plot; comparación de velocidad media por nivel de congestionamiento.

Interpretación esperada: a mayor tráfico, mayor tiempo de recorrido y menor velocidad promedio.

---

### 3.12. Number_of_Signals

- ¿A qué se refiere?: número de semáforos o cruces controlados en la ruta.
- ¿Qué datos contiene?: valor numérico entero.
- ¿Cómo ayuda a predecir el tiempo de entrega?: representa fricción vial y posibles esperas. Muchas señales implican más demora, especialmente en zonas urbanas densas.
- Tres variables más relacionadas:
  1. Traffic_Level
  2. Road_Distance_km
  3. Average_Speed_kmph
- Gráfica recomendada: scatter plot de señales vs tiempo; boxplot por rangos de señales.

Interpretación esperada: más señales suelen acompañar tiempos más altos y rutas más lentas.

---

### 3.13. Average_Speed_kmph

- ¿A qué se refiere?: velocidad promedio que alcanza el repartidor durante la entrega.
- ¿Qué datos contiene?: valor continuo en kilómetros por hora.
- ¿Cómo ayuda a predecir el tiempo de entrega?: es una de las variables más directas para inferir el tiempo del recorrido. Si la velocidad es baja, el tiempo aumenta.
- Tres variables más relacionadas:
  1. Traffic_Level
  2. Road_Distance_km
  3. Vehicle_Type
- Gráfica recomendada: scatter plot velocidad vs tiempo; boxplot por nivel de tráfico; regresión simple de velocidad contra tiempo.

Interpretación esperada: la velocidad promedio tiene una relación inversa fuerte con el tiempo total del recorrido.

---

### 3.14. Delivery_Priority

- ¿A qué se refiere?: prioridad del pedido, por ejemplo normal, urgente o premium.
- ¿Qué datos contiene?: categorías que indican la urgencia o prioridad operativa.
- ¿Cómo ayuda a predecir el tiempo de entrega?: puede influir en la ruta, la selección del repartidor y la política de atención. También indica si hubo presión para cumplir SLA.
- Tres variables más relacionadas:
  1. Preparation_Time_Min
  2. Traffic_Level
  3. Delivery_Distance_Category
- Gráfica recomendada: boxplot de tiempo por prioridad; conteo por prioridad; análisis de tiempos por prioridad y tráfico.

Interpretación esperada: las entregas con prioridad alta suelen tener tiempos más bajos o más controlados, pero también pueden implicar rutas más intensas.

---

### 3.15. Time_taken(min)

- ¿A qué se refiere?: tiempo real de entrega en minutos desde el inicio hasta la finalización del pedido.
- ¿Qué datos contiene?: variable continua, generalmente en minutos, con valores enteros o decimales.
- ¿Cómo ayuda a predecir el tiempo de entrega?: es la variable objetivo que se desea predecir con un modelo.
- Tres variables más relacionadas:
  1. Road_Distance_km
  2. Traffic_Level
  3. Average_Speed_kmph
- Gráfica recomendada: histograma, boxplot, densidad KDE y scatter por factores clave.

Interpretación esperada: la variable objetivo suele distribuirse con asimetría por algunos casos de entrega muy lenta. El análisis de histogramas, boxplots por tráfico y scatter con distancia ayuda a detectar patrones y valores atípicos.

---

## 4. Variables que más aportan para predecir la entrega

En una primera etapa de modelado, las variables que más claramente aportan valor para explicar el tiempo de entrega son:

1. Road_Distance_km
2. Traffic_Level
3. Average_Speed_kmph
4. Weather
5. Pickup_Zone / Dropoff_Zone
6. Preparation_Time_Min
7. Number_of_Signals
8. Vehicle_Type
9. Rider_Experience_Years
10. Delivery_Priority

Estas variables son las que mejor conectan la operación real con la duración del servicio. En cambio, variables de identidad como `Order_ID` o calificaciones sin una relación causal clara suelen descartarse en la etapa de modelado.

---

## 5. Recomendación visual por aspecto de análisis

A continuación, un mapa práctico para decidir qué gráfica usar por cada tema:

- Distancia vs tiempo: scatter plot con tendencia.
- Tráfico vs tiempo: boxplot o violin plot.
- Clima vs tiempo: boxplot por categoría.
- Zona vs tiempo: barplot de tiempo promedio por zona.
- Vehículo vs tiempo: boxplot por tipo de vehículo.
- Experiencia vs tiempo: scatter plot o línea de tendencia.
- Señales vs tiempo: scatter plot y análisis de distribución.
- Prioridad vs tiempo: boxplot por prioridad.
- Tiempo objetivo: histograma, KDE y boxplot.

Esto ayuda a separar el análisis por aspecto y no mezclar variables que responden a problemas distintos.

---

## 6. Conclusión

El dataset está bien estructurado para un problema de predicción de tiempos de entrega, porque combina variables de distancia, flujo vehicular, clima, zona y condiciones operativas. La clave para el análisis no es solo mirar cada variable por separado, sino estudiar cómo se relacionan entre sí y cómo esas combinaciones explican la variabilidad del tiempo total.

Si se quiere construir un modelo útil, las variables más relevantes para empezar son la distancia, el tráfico, la velocidad promedio, el clima y las zonas de pickup/dropoff. A partir de ahí, se pueden añadir indicadores de preparación, experiencia del repartidor y prioridad del pedido para mejorar la capacidad predictiva.

---

## 7. Sugerencia final de trabajo

Para continuar con la etapa analítica, lo ideal es:

1. limpiar variables no útiles (IDs, identificadores y datos de trazabilidad),
2. analizar correlación y distribución de las variables clave,
3. revisar relaciones por segmento (zona, clima, tráfico, vehículo),
4. crear gráficas de dispersión y cajas por cada factor,
5. preparar la base para un modelo de regresión con la variable objetivo `Time_taken(min)`.

Con este enfoque, el proyecto puede pasar de un análisis descriptivo a un proceso de modelado más robusto y con mejor capacidad explicativa.
