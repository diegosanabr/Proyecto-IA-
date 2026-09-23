# Variables seleccionadas del dataset de delivery Rappi

## Objetivo

Este documento resume el filtrado aplicado en el notebook `proyecto_rappi.ipynb` para conservar únicamente las variables definidas como relevantes para predecir el tiempo de entrega.

El dataset filtrado contiene **20 columnas**: **19 variables predictoras** y la variable objetivo `Time_taken_min` (tiempo de entrega en minutos).

## Variables conservadas

| # | Variable | Motivo de conservación |
|---:|---|---|
| 1 | `Road_Distance_km` | La distancia del recorrido es uno de los factores más directos del tiempo total. |
| 2 | `Average_Speed_kmph` | La velocidad permite explicar cuánto tarda el desplazamiento; su relación con el tiempo es inversa. |
| 3 | `Vehicle_Type` | El vehículo influye en la movilidad, la velocidad y la capacidad de circular en distintas zonas. |
| 4 | `Traffic_Level` | La congestión afecta directamente la velocidad y la duración del recorrido. |
| 5 | `Delivery_Distance_Category` | Resume la distancia en grupos fáciles de interpretar y comparar. |
| 6 | `Weather` | El clima puede afectar la seguridad, la velocidad y las demoras durante el trayecto. |
| 7 | `Preparation_Time_Min` | Representa el tiempo que el restaurante necesita antes de despachar el pedido. |
| 8 | `Cuisine_Type` | Puede relacionarse con diferencias en preparación y operación del restaurante. |
| 9 | `Pickup_Zone` | Representa las condiciones y características de la zona de recogida. |
| 10 | `Order_Hour` | Permite capturar variaciones de tráfico y operación según la hora del pedido. |
| 11 | `Number_of_Signals` | Aproxima las paradas y la fricción vial presentes en la ruta. |
| 12 | `Day_of_Week` | Puede reflejar patrones de demanda y operación durante la semana. |
| 13 | `Is_Weekend` | Distingue el comportamiento operativo de fines de semana y días laborales. |
| 14 | `Order_Items` | La cantidad de artículos puede influir en la preparación y manipulación del pedido. |
| 15 | `Is_Festival` | Permite representar cambios de demanda y operación en días especiales. |
| 16 | `Rider_Experience_Years` | Describe la experiencia del repartidor y su posible efecto en la ruta y la operación. |
| 17 | `Rider_Rating` | Se conserva como señal secundaria de consistencia o desempeño del repartidor. |
| 18 | `Delivery_Priority` | Puede representar reglas operativas, urgencia y prioridad de atención del pedido. |
| 19 | `Restaurant_Load` | Representa la carga de trabajo del restaurante al preparar pedidos. |
| 20 | `Time_taken_min` | Variable objetivo: tiempo total de entrega en minutos. No se utiliza como predictor. |

## Resultado del filtrado

En el notebook se conserva el dataset original en `df` y se crea una copia filtrada llamada `df_filtrado`. Esta copia contiene únicamente las 20 columnas anteriores y mantiene todas las filas originales.

El filtrado aplicado es:

- 19 variables predictoras.
- 1 variable objetivo.
- 20 columnas en total.
- Ninguna fila eliminada en esta etapa.

## Variables removidas y motivo

| Variable removida | Motivo |
|---|---|
| `Order_ID` | Es un identificador único. No describe las condiciones del pedido y puede provocar memorización o sobreajuste. |
| `Order_Date` | La fecha original se elimina como texto. Si más adelante se necesitan tendencias temporales, conviene transformarla en variables como mes, temporada o día del año. |
| `Dropoff_Zone` | En el análisis inicial mostró una relación muy débil con el objetivo. La información geográfica principal se conserva mediante `Pickup_Zone`, distancia, tráfico y categoría de distancia. |
| `Restaurant_Rating` | Es una señal indirecta de calidad del restaurante y mostró una relación débil con el tiempo. Se priorizan variables operativas más cercanas a la entrega, como carga y preparación. |

## Nota metodológica

La selección combina la correlación de las variables numéricas, el comportamiento del tiempo entre categorías y el significado operativo de cada columna. Una variable con relación lineal baja no se elimina automáticamente si puede aportar información categórica útil.

Esta selección es la base para la siguiente etapa del proyecto. Antes de entrenar un modelo, se debe validar si las 19 predictoras mejoran las métricas de regresión y revisar que ninguna contenga información disponible únicamente después de completar la entrega.
