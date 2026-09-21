# Selección de variables para el dataset Rappi

## Objetivo

A partir del análisis de candidatas y de la relación con `Time_taken(min)`, se propone conservar **19 variables predictoras más la variable objetivo**. Esta propuesta sirve para la documentación del proyecto y **todavía no modifica ni elimina columnas del dataset**.

En el archivo descargado, la columna objetivo aparece como `Time_taken_min`; corresponde al mismo tiempo de entrega en minutos descrito como `Time_taken(min)`.

## Variables definitivas a conservar

| # | Variable | Motivo principal |
|---:|---|---|
| 1 | `Road_Distance_km` | Es la variable numérica más relacionada con el tiempo (`r = 0.703`). |
| 2 | `Average_Speed_kmph` | Tiene una relación inversa fuerte (`r = -0.596`): a menor velocidad, mayor tiempo. |
| 3 | `Vehicle_Type` | Presenta diferencias importantes entre categorías (`eta = 0.489`). |
| 4 | `Traffic_Level` | Representa la congestión y muestra una relación relevante por categoría (`eta = 0.333`). |
| 5 | `Delivery_Distance_Category` | Resume rangos de distancia y aporta una comparación operativa (`eta = 0.307`). |
| 6 | `Weather` | Las condiciones climáticas separan los tiempos de entrega (`eta = 0.299`). |
| 7 | `Preparation_Time_Min` | Añade el tiempo de preparación al proceso (`r = 0.228`). |
| 8 | `Cuisine_Type` | Puede representar diferencias en preparación y operación del restaurante (`eta = 0.200`). |
| 9 | `Pickup_Zone` | Conserva información geográfica y operativa del punto de recogida (`eta = 0.158`). |
| 10 | `Order_Hour` | Captura variaciones del tráfico y de la operación según la hora del pedido. |
| 11 | `Number_of_Signals` | Representa posibles paradas y fricción vial (`r = 0.127`). |
| 12 | `Day_of_Week` | Puede capturar patrones de demanda y operación semanal (`eta = 0.103`). |
| 13 | `Is_Weekend` | Complementa el efecto del calendario (`r = 0.102`). |
| 14 | `Order_Items` | Puede afectar el tiempo de preparación (`r = 0.082`). |
| 15 | `Is_Festival` | Permite representar cambios operativos en días especiales (`r = 0.073`). |
| 16 | `Rider_Experience_Years` | Aporta información sobre el perfil del repartidor, aunque su relación lineal es débil (`r = -0.057`). |
| 17 | `Rider_Rating` | Puede reflejar consistencia operativa; su relación observada es débil (`r = -0.036`). |
| 18 | `Delivery_Priority` | Puede representar reglas operativas y prioridades de atención; se conserva para validar su aporte en el modelo. |
| 19 | `Restaurant_Load` | Representa la carga operativa del restaurante y se conserva para validarla junto con preparación y pedidos. |

## Variable objetivo

- `Time_taken_min` o `Time_taken(min)`: tiempo total de entrega en minutos. No es una variable predictora; es el valor que el modelo debe estimar.

## Variables definitivas candidatas a filtrar

| Variable | Por qué se propone filtrar |
|---|---|
| `Order_ID` | Es un identificador único. No describe las condiciones del pedido y podría causar memorización o sobreajuste. Aunque aparece asociado al objetivo por su unicidad, esa relación no es útil ni generalizable. |
| `Order_Date` | La fecha cruda no debe entrar directamente al modelo como texto. Se propone filtrarla después de extraer variables como mes, día o temporada. |
| `Dropoff_Zone` | En este dataset mostró una relación muy débil por categoría (`eta = 0.006`). Se deja fuera de la primera selección, aunque podría reincorporarse si un análisis posterior demuestra valor al combinarla con distancia y tráfico. |
| `Restaurant_Rating` | Es una señal indirecta de calidad del restaurante y presentó una relación muy débil con el objetivo (`r = 0.019`). Se deja fuera frente a variables operativas más directamente relacionadas con la entrega. |

## Nota sobre la selección

La matriz de correlación ordena principalmente variables numéricas. Para las variables categóricas se revisó la diferencia de los tiempos entre categorías mediante una medida de efecto (`eta`). Por eso, una variable categórica no se eliminó únicamente por no aparecer en la matriz numérica.

La selección es una **lista inicial de 20 columnas**: 19 predictoras y el objetivo. Antes de entrenar, se debe comprobar el resultado con validación del modelo, revisar la fecha transformada y confirmar que ninguna variable contiene información disponible únicamente después de la entrega.

## Resumen final

- Columnas totales propuestas: **20**.
- Variables predictoras: **19**.
- Variable objetivo: **1**.
- Columnas candidatas a filtrar: **4**.
- Estado: **selección analítica; el filtrado aún no se ha ejecutado**.
