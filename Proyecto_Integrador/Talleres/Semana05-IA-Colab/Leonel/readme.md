REGRESIÓN LINEAL Y ANÁLISIS DE DATOS

Descripción del proyecto:

En este proyecto realizo un análisis de datos orientado a estudiar el consumo de energía a partir de diferentes variables de operación. El conjunto de datos contiene 5000 registros y cinco variables principales:

- Temperatura

- Horas_Operacion

- Carga

- Humedad

- Consumo_Energia

Mi objetivo principal es explorar los datos, identificar relaciones entre las variables y construir un modelo de regresión lineal que permita estimar el consumo de energía a partir de las variables disponibles.

Además de la regresión lineal, utilizo un árbol de decisión para regresión y comparo el comportamiento de las variables mediante diferentes herramientas estadísticas y gráficas.

1. Relación entre las variables:

<img width="1235" height="1231" alt="image" src="https://github.com/user-attachments/assets/ff155d9b-1565-48fb-aced-42219e1105fc" />

En esta gráfica utilizo un pairplot para observar las relaciones entre las variables numéricas del conjunto de datos.

Me permite analizar visualmente cómo se comportan variables como Temperatura, Horas_Operacion, Carga y Humedad frente a las demás variables. También puedo observar la relación de estas variables con Consumo_Energia.

Esta visualización sirve como una primera aproximación para identificar posibles relaciones lineales, tendencias y dispersiones en los datos.

2. Distribución del consumo de energía:

<img width="695" height="351" alt="image" src="https://github.com/user-attachments/assets/dab36335-1b52-4e1c-9b4f-72d7a141eb55" />


En este histograma analizo específicamente la variable Consumo_Energia.

Utilizo plot.hist() con 25 intervalos (bins=25) para observar cómo se distribuyen los valores del consumo dentro de los 5000 registros.

Esta gráfica me permite conocer la concentración de los datos y observar si los valores del consumo se encuentran distribuidos de manera uniforme o si existe una mayor concentración en determinados rangos.

3. Correlación entre las variables:

<img width="761" height="588" alt="image" src="https://github.com/user-attachments/assets/758e1320-78fd-4360-b857-e5c2a2ebbdbb" />

En esta sección calculo la matriz de correlación utilizando únicamente las variables numéricas.

La correlación permite analizar qué tan relacionadas están dos variables entre sí. Para facilitar su interpretación utilizo un mapa de calor (heatmap), donde cada celda muestra el valor de correlación entre un par de variables.

Esta parte es especialmente importante porque permite identificar qué variables presentan una relación más marcada con Consumo_Energia antes de construir el modelo de regresión.

4. Relación de cada variable predictora con el consumo de energía:

<img width="1442" height="844" alt="image" src="https://github.com/user-attachments/assets/931165b7-ebc9-4abd-847e-21837029bbb8" />

En esta sección genero cuatro diagramas de dispersión para analizar individualmente la relación entre cada variable predictora y Consumo_Energia.

Las variables predictoras utilizadas son Temperatura, Horas_Operacion, Carga y Humedad. Para organizar los gráficos utilizo una cuadrícula de 2 × 2, lo que permite visualizar las cuatro relaciones dentro de una misma figura.

Cada punto representa una observación del conjunto de datos. Esta visualización permite identificar de forma gráfica si existe alguna tendencia entre las variables independientes y el consumo de energía.

5. Consumo de energía real vs. consumo predicho:

<img width="852" height="650" alt="image" src="https://github.com/user-attachments/assets/dca75c23-4aa0-400a-884f-74a5154da7c4" />

En este gráfico comparo los valores reales de Consumo_Energia del conjunto de prueba (y_test) con los valores estimados por el modelo de regresión lineal (predictions).

El eje horizontal representa el consumo de energía real, mientras que el eje vertical representa el consumo de energía predicho por el modelo.

El objetivo de esta comparación es observar qué tan cercanas se encuentran las predicciones respecto a los valores reales. Idealmente, los puntos deberían aproximarse a una línea diagonal de 45°, ya que esto indicaría que los valores predichos se encuentran cerca de los valores observados.

6. Distribución de los residuos:

<img width="944" height="647" alt="image" src="https://github.com/user-attachments/assets/a0fb9225-c107-45e3-8e60-ee05237c3cc2" />

En esta sección analizo los residuos del modelo, calculados como la diferencia entre los valores reales y los valores predichos:

y_test - predictions

Utilizo un histograma junto con una estimación de densidad (KDE) para observar cómo se distribuyen estos errores.

El eje horizontal representa los residuos, mientras que el eje vertical representa su densidad. Esta gráfica permite revisar visualmente la distribución de los errores y comprobar si presentan una concentración alrededor de un valor central o algún patrón particular.

En el código se utiliza sns.distplot() para realizar esta visualización; además, se deja indicado que esta función se eliminará en versiones futuras de Seaborn.
