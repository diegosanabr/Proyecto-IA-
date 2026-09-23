# Proyecto Predicción de Tiempos de Entrega (Rappi)

---

## 1\. Contexto del problema y justificación del uso de inteligencia artificial

### 1.1 El Problema logístico

En la industria de la entrega de alimentos a domicilio como Rappi, la promesa de tiempo de entrega es un factor crítico para la retención de un cliente y la eficiencia. Estimar cuánto va a tardar un pedido antes de que salga del restaurante permite:

* Optimizar la asignación de repartidores y rutas.  
* Gestionar expectativas reales al usuario final.  
* Anticipar demoras por congestión o demoras en cocina.

### 1.2 ¿Por qué una simple fórmula matemática NO es suficiente?

Una aproximación ingenua podría intentar calcular el tiempo mediante una regla basada en física o velocidad constante como que el tiempo estimado  fuese igual  a la distancia sobre la velocidad, sin embargo, esta fórmula falla en entornos reales por algunas de las siguientes razones:

1. **No linealidad del tráfico:** El tráfico no es constante; cambia drásticamente por horas pico, días de la semana, accidentes y diversos eventos. 10 km en una zona de alta congestión toman mucho más tiempo que 20 km en una vía normal.  
2. **Factores ambientales:** El clima adverso como lluvia o tormentas reduce la velocidad de desplazamiento de las motocicletas y bicicletas de forma impredecible mediante reglas fijas.  
3. **Contexto:** El tiempo real es el resultado de combinar la distancia, el nivel de tráfico, las condiciones del vehículo, la experiencia del repartidor y el tiempo de preparación del restaurante.  
4. **Variabilidad espacial:** Las zonas de recogida y entrega pueden traer diversas dinámicas que una ecuación lineal simple ignorará.

**Conclusión:** Se requiere Inteligencia Artificial  porque los modelos basados en datos,pueden aprender patrones complejos, probar múltiples variables simultáneamente y capturar relaciones a partir de datos históricos.

---

## 2\. Estructura y análisis del dataset

El conjunto de datos utilizado consta de 50,000 registros en filas y 24 variables en columnas estructuradas para modelar el comportamiento de las entregas.

### 2.1 Variables clave y su significado operativo

* **`Time_taken_min` (Variable Objetivo):** Representa el tiempo total real que tardó la entrega en minutos. Es una variable continua, lo que define el problema como una tarea de Regresión. Sus estadísticos descriptivos muestran una media aproximada de 83.87 minutos y una desviación estándar de 35.43 minutos con un rango entre 10 y 180 min, y un Rango Intercuartil de 44 min.  
* **`Road_Distance_km`:** Distancia estimada en kilómetros entre el restaurante y el destino. Es uno de los datos con mayor correlación directa con el tiempo de entrega.  
* **`Traffic_Level`:** Nivel de congestión vial  el cual impacta de forma directa la velocidad media del recorrido.  
* **`Weather`:** Condiciones climáticas que afectan la movilidad y los tiempos de tránsito.  
* **`Pickup_Zone` / `Dropoff_Zone`:** Zonas geográficas de origen y destino, las cuales reflejan la complejidad urbana de la ruta.  
* **`Vehicle_Type`:** Tipo de medio de transporte utilizado.  
* **`Preparation_Time_Min`:** Minutos que tardó el restaurante en tener lista la comida antes de ser entregada al repartidor.  
* **`Average_Speed_kmph`:** Velocidad promedio registrada durante el trayecto. Muestra una relación inversa fuerte con el tiempo total.  
* **`Delivery_Priority`:** Nivel de prioridad asignado al pedido.

### 2.2 Variables descartadas por fuga de información y ruido

Para entrenar un modelo robusto y evitar el sobreajuste, se eliminaron deliberadamente aquellas columnas que no aportan valor predictivo o que representan fuga de información:

* **Identificadores únicos:** `Order_ID` sólo sirve para trazabilidad, no generaliza ningún patrón.  
* **Atributos de evaluación:** Columnas de nombres o métricas que solo se conocen *después* de finalizada la entrega y que no están disponibles al momento de prometer el tiempo al cliente.

---

## 3\. Calidad de datos, outliers y sesgos

### 3.1 Tratamiento de valores faltantes y calidad

* El análisis inicial confirma que las variables principales presentan un porcentaje de nulos controlados o inexistentes en su estructura base. Sin embargo, en un entorno de trabajo real los datos pueden llegar incompletos.

### 3.2 Detección y manejo de valores atípicos

* Al analizar la variable objetivo `Time_taken_min`, se identificaron valores extremos mediante el método del Rango Intercuartil .  
* Específicamente, se detectaron más de 2,000 observaciones en los límites superiores los cuales eran valores cercanos al tope de los 180 minutos.  
* Estos outliers no deben eliminarse automáticamente a menos que sean errores de digitación, ya que representan entregas legítimamente complejas con recorridos largos bajo lluvia y tráfico denso. Ignorarlos dejaría al modelo ciego ante los peores escenarios posibles del servicio.

### 3.3 Sesgo operativo y desbalance

* **Sesgo por segmentación:** Si la mayoría de los datos provienen de una zona específica o de horarios de tráfico moderado, el modelo tenderá a predecir muy bien los casos frecuentes y a fallar en los extremos o que simplemente se salgan de sus parametros.  
* **Enfoque de Regresión vs la Clasificación:** Al tratarse de una variable continua `Time_taken_min`, el concepto de desbalance se evalúa analizando la asimetría y el sesgo de la distribución de tiempos la cual nos muestra una concentración en rangos medios. Si el problema se transformara en una tarea de clasificación, se observaría un desbalance de clases, requiriendo métricas orientadas hacia aquello.

---

## 4\. Patrones Analíticos e Insights del EDA

A partir de las visualizaciones y matrices de correlación desarrolladas en el proyecto, se pueden observar los siguientes hallazgos analíticos:

1. **Jerarquía de Correlación con el Tiempo:**  
   * La distancia vial  de la variable `Road_Distance_km` y el nivel de tráfico de `Traffic_Level` dominan la varianza del tiempo de entrega.  
   * A mayor densidad de tráfico y mayor distancia, el tiempo crece de forma no lineal debido a las paradas y desaceleraciones que se van acumulando.  
2. **Impacto del Clima y la Velocidad:**  
   * Las categorías de clima desplazan la distribución del tiempo hacia la derecha aumentando la mediana de duración y reduciendo simultáneamente la velocidad promedio de los vehículos.  
3. **El Tiempo de Preparación:**  
   * El `Preparation_Time_Min` actúa como una variable al inicio de la entrega. Un restaurante con alta carga de pedidos incrementa el tiempo total acumulado, independientemente de la agilidad del repartidor.

---

## 5\. Plan de Trabajo y Siguientes Fases&nbsp;

Para la evolución del proyecto hacia el modelado predicción, se planea la siguiente ruta:

&nbsp;

---

## 6*.* Gráficas

![ Gráficos 1: análisis visual de la variable objetivo y relaciones clave](https://github.com/17273747/imagenes_proyecto_IA1/blob/main/distribucion_tiempo_de_entrega.png?raw=true)

* **X (Tiempo en minutos):** Muestra el rango de duración de las entregas, desde un aproximado 10 minutos hasta un límite máximo de 180 minutos.&nbsp;  
* **Y (Frecuencia):** Muestra la cantidad de pedidos que caen dentro de cada intervalo de tiempo.&nbsp;  
* Esta gráfica permite ver que la distribución de los tiempos de entrega, presenta una asimetría hacia la derecha con un grupo de entregas atípicas que llegan hasta el límite de los 180 minutos, los cuales representan los casos más complejos de congestión o clima adverso que el modelo deberá aprender a predecir.&nbsp;  
  &nbsp;

&nbsp;

![ Gráficos 2:Correlación de las 25 variables más relacionadas con la variable objetivo](https://github.com/17273747/imagenes_proyecto_IA1/blob/main/variables_con_mayor_relaci%C3%B3n_al_tiempo_de_entrega.png?raw=true)

![Gráfico 3: Distribución + boxplot de time_taken](https://github.com/17273747/imagenes_proyecto_IA1/blob/main/distribucion_boxplot_de_time_taken.png?raw=true)

![Gráfico 4: Tiempo de entrega por nivel de trafico](https://github.com/17273747/imagenes_proyecto_IA1/blob/main/Tiempo_de_entrega_por_nivel_de_trafico.png?raw=true)

![Gráfico 5: Time_taken vs velocidad promedio](https://github.com/17273747/imagenes_proyecto_IA1/blob/main/Time_taken_vs_velocidad_promedio2.png?raw=true)

![Gráfico 6: Distribución con cuartiles y tiempo de entrega por cuartil ](https://github.com/17273747/imagenes_proyecto_IA1/blob/main/distribucion_tiempo_de_entrega_con_cuartiles.png?raw=true)

![ Gráfico 7:  # 1. Histograma + KDE, # 2. Boxplot, # 3. Q-Q plot, # 4. Boxplot por tráfico](https://github.com/17273747/imagenes_proyecto_IA1/blob/main/cuatro_graficas.png?raw=true)




