# REGRESIÓN LINEAL Y MODELOS DE PREDICCIÓN APLICADOS A LA CONCENTRACIÓN DE MONÓXIDO DE CARBONO (CO)

*Caso de estudio: Monterey County, California — Año 2022*

---

## 1. Introducción

El presente informe documenta y analiza en detalle el trabajo de regresión lineal desarrollado en Google Colab que acompaña este documento [1], en el cual se aplican técnicas de regresión lineal y modelos de predicción sobre la concentración máxima diaria de monóxido de carbono (CO) registrada durante el año 2022 en la estación "Salinas 3", condado de Monterey, California.

El monóxido de carbono es un gas incoloro e inodoro que se genera principalmente por la combustión incompleta de combustibles fósiles en vehículos, maquinaria y procesos industriales. Su presencia en el aire ambiental reduce la capacidad de la sangre para transportar oxígeno hacia órganos vitales y constituye un riesgo particular para personas con enfermedades cardiovasculares; por ello, la Agencia de Protección Ambiental de los Estados Unidos (EPA) monitorea de forma continua su concentración en distintas estaciones del país y pone estas mediciones a disposición del público [2], siendo precisamente esa plataforma la fuente de la base de datos utilizada en el cuaderno de trabajo [1].

El trabajo documentado en [1] replica la estructura metodológica de un taller de regresión lineal desarrollado previamente en clase, adaptando el análisis a esta nueva base de datos, e incorpora además, en el presente informe, una interpretación estadística detallada de los resultados obtenidos: tendencias, medidas de tendencia central y dispersión, matrices de correlación, coeficientes del modelo, y métricas de error.

Los objetivos específicos del informe son: (i) describir y explorar estadísticamente la base de datos utilizada; (ii) construir y entrenar un modelo de regresión lineal múltiple para predecir la concentración de CO a partir de variables temporales y de completitud de las observaciones; (iii) evaluar el desempeño del modelo mediante métricas estándar de error (MAE, MSE, RMSE, R²) y mediante el método de mínimos cuadrados ordinarios (OLS); y (iv) discutir críticamente los resultados obtenidos, identificando las limitaciones del enfoque y las causas probables del bajo poder predictivo del modelo.

## 2. Metodología

### 2.1 Descripción de la base de datos

La base de datos fue descargada del portal de datos diarios de calidad del aire de la EPA [2], tal como se documenta en el cuaderno de trabajo [1], y corresponde a la estación de monitoreo "Salinas 3" (Site ID 060531003), condado de Monterey, California, durante el año 2022. El conjunto contiene 365 registros diarios (uno por cada día del año) y 21 columnas originales, entre las que destacan la fecha de medición, la concentración máxima diaria de CO en un periodo de 8 horas (variable objetivo, en partes por millón, ppm), el valor de Índice de Calidad del Aire (AQI) asociado, el número de observaciones horarias utilizadas para calcular el valor diario (Daily Obs Count) y el porcentaje de completitud de dichas observaciones (Percent Complete). La base de datos no presenta valores nulos ni fechas duplicadas, y corresponde a una única estación de monitoreo, por lo que columnas como identificadores de sitio, coordenadas geográficas, código de condado o unidades de medida resultan constantes y fueron descartadas del análisis por no aportar variabilidad explicativa.

### 2.2 Herramientas y librerías utilizadas

El análisis se desarrolló íntegramente en Python 3, dentro del cuaderno (Google Colab) referenciado en [1]. Se emplearon las siguientes librerías: pandas y numpy para la manipulación y el cálculo numérico; matplotlib y seaborn para la visualización de datos; scikit-learn para la construcción, entrenamiento y evaluación de los modelos de regresión lineal y árbol de decisión; y statsmodels para el ajuste del modelo por mínimos cuadrados ordinarios (OLS) y la obtención de su resumen estadístico inferencial.

### 2.3 Fundamentos teóricos de la regresión lineal

La regresión lineal es un método estadístico supervisado que modela la relación entre una variable dependiente continua (variable objetivo) y una o más variables independientes (predictoras), asumiendo que dicha relación puede aproximarse mediante una combinación lineal de los predictores [3]. En su forma múltiple, el modelo se expresa como:

$$y = \beta_0 + \beta_1x_1 + \beta_2x_2 + \dots + \beta_nx_n + \varepsilon$$

donde y es la variable objetivo (en este caso, la concentración de CO), x₁, …, xₙ son las variables predictoras, β₀ es el intercepto (valor esperado de y cuando todos los predictores son cero), β₁, …, βₙ son los coeficientes de regresión (el cambio esperado en y ante un incremento unitario del predictor correspondiente, manteniendo las demás variables constantes) y ε es el término de error aleatorio, que recoge la variabilidad no explicada por el modelo.

Los coeficientes se estiman mediante el método de mínimos cuadrados ordinarios (Ordinary Least Squares, OLS), que consiste en encontrar los valores de β que minimizan la suma de los cuadrados de los residuos (la diferencia entre el valor observado y el valor predicho por el modelo). Este procedimiento tiene solución analítica cerrada y es, además, el estimador insesgado de menor varianza cuando se cumplen los supuestos clásicos de la regresión lineal: (a) linealidad de la relación entre predictores y variable objetivo; (b) independencia de los residuos; (c) homoscedasticidad, es decir, varianza constante de los residuos a lo largo de los valores predichos; y (d) normalidad de los residuos. El incumplimiento de estos supuestos no invalida por completo el modelo, pero sí compromete la validez de las pruebas de significancia estadística e introduce sesgos en la estimación de errores estándar.

Para evaluar qué tan bien se ajusta el modelo, se emplean las siguientes métricas: el Error Absoluto Medio (MAE), que promedia el valor absoluto de los residuos y se interpreta en las mismas unidades que la variable objetivo; el Error Cuadrático Medio (MSE), que penaliza más fuertemente los errores grandes al elevarlos al cuadrado; la Raíz del Error Cuadrático Medio (RMSE), que devuelve el MSE a las unidades originales; y el coeficiente de determinación R², que representa la proporción de la varianza de la variable objetivo que es explicada por el modelo. Un R² cercano a 1 indica un buen ajuste; un R² cercano a 0 indica que el modelo no explica mejor la variabilidad que la simple media de los datos; y un R² negativo, posible únicamente fuera de la muestra de entrenamiento, indica que el modelo predice peor que dicha media.

Adicionalmente, el ajuste por OLS (mínimos cuadrados) mediante la librería statsmodels permite obtener, para cada coeficiente, su error estándar, el estadístico t (cociente entre el coeficiente y su error estándar) y el valor p asociado, que permite contrastar la hipótesis nula de que el coeficiente poblacional es igual a cero. Un valor p mayor a 0.05 (para un nivel de significancia convencional del 5%) indica que no existe evidencia estadística suficiente para afirmar que la variable correspondiente tiene un efecto distinto de cero sobre la variable objetivo.

Un problema frecuente en la regresión múltiple es la multicolinealidad, que ocurre cuando dos o más variables predictoras están altamente correlacionadas entre sí. Esto no sesga las predicciones del modelo en conjunto, pero infla la varianza de los coeficientes individuales, dificulta su interpretación aislada y puede generar coeficientes inestables o de signo contraintuitivo. Un síntoma habitual es un número de condición elevado en el resumen del modelo OLS.

Finalmente, y de manera complementaria, se incluye un modelo de árbol de decisión de regresión (Decision Tree Regressor), un método no paramétrico que particiona el espacio de predictores en regiones sucesivas mediante reglas de decisión binarias, sin asumir una relación lineal entre variables. Este modelo se entrenó sobre un conjunto de datos sintético generado artificialmente, con el único propósito de practicar la técnica; sus resultados no son comparables directamente con el modelo de CO.

### 2.4 Procedimiento aplicado

- Conversión de la columna Date a formato de fecha y construcción de tres variables temporales derivadas: Month (mes), Day_of_Year (día del año, 1–365) y Day_of_Week (día de la semana, 0=lunes … 6=domingo).
- Definición de la variable objetivo (y): Daily Max 8-hour CO Concentration.
- Selección de características (X): Month, Day_of_Year, Day_of_Week y Percent Complete.
- Exclusión deliberada de Daily AQI Value como predictor, dado que este índice se calcula directamente a partir de la concentración de CO y su uso generaría fuga de información (data leakage).
- Exclusión de Daily Obs Count por estar casi perfectamente correlacionada con Percent Complete (r ≈ 0.9999 en esta base de datos), lo que generaba multicolinealidad severa; se conservó Percent Complete por ser la variable estándar de completitud reportada por la EPA.
- División de los datos en un conjunto de entrenamiento (70 %) y uno de prueba (30 %), utilizando train_test_split con random_state=123 para garantizar reproducibilidad.
- Entrenamiento de un modelo de regresión lineal múltiple (LinearRegression de scikit-learn) sobre el conjunto de entrenamiento.
- Evaluación del modelo sobre el conjunto de prueba mediante MAE, MSE, RMSE y R², y análisis gráfico de residuos.
- Ajuste adicional del mismo conjunto de variables mediante mínimos cuadrados ordinarios (OLS) con statsmodels, para obtener el resumen estadístico inferencial completo.
- Práctica complementaria de un árbol de decisión de regresión sobre un conjunto de datos sintético, generado con make_regression, como ejercicio independiente sobre modelos no lineales.

## 3. Resultados

### 3.1 Análisis exploratorio de la concentración de CO

La Tabla I resume las principales estadísticas descriptivas de la variable objetivo y de las variables relacionadas con la calidad de la medición.

| Estadístico | CO (ppm) | AQI | Obs. Count | % Completo |
|---|---|---|---|---|
| Media | 0.300 | 3.21 | 23.93 | 99.69 |
| Desv. estándar | 0.15 | 1.82 | 0.73 | 3.07 |
| Mínimo | 0.10 | 1.00 | 14.00 | 58.00 |
| Percentil 25 | 0.20 | 2.00 | 24.00 | 100.00 |
| Mediana | 0.30 | 3.00 | 24.00 | 100.00 |
| Percentil 75 | 0.30 | 3.00 | 24.00 | 100.00 |
| Máximo | 1.30 | 15.00 | 24.00 | 100.00 |

*Tabla I. Estadísticas descriptivas de la concentración de CO y variables de completitud (n = 365).*

La concentración media de CO en 2022 fue de 0.30 ppm, con una desviación estándar de 0.145 ppm, un valor mínimo de 0.10 ppm y un máximo de 1.30 ppm. La mediana (0.30 ppm) es prácticamente igual a la media, pero el histograma de la Figura 1 muestra una distribución con marcada asimetría positiva: la mayoría de los días (más del 80 %) se concentran entre 0.2 y 0.4 ppm, mientras que existe una cola derecha de días con valores más altos (0.6 a 1.3 ppm), asociados probablemente a episodios puntuales de mayor tráfico vehicular, condiciones meteorológicas de baja dispersión atmosférica (inversión térmica) o quema de biomasa. La curva de densidad (Figura 2) confirma este patrón, mostrando un pico pronunciado alrededor de 0.25–0.30 ppm y una cola larga hacia valores mayores.

<div align="center">

<img rc="https://github.com/user-attachments/assets/454d2cbf-b5d8-4122-a947-23eecdf66ab1" alt="Figura 1. Histograma de la concentración máxima diaria de CO (8 h), 2022." width="650">

*Figura 1. Histograma de la concentración máxima diaria de CO (8 h), 2022.*

</div>


![Figura 2. Función de densidad estimada de la concentración de CO.](imagenes/figura2_densidad_co.png)

*Figura 2. Función de densidad estimada de la concentración de CO.*

Respecto a la completitud de las observaciones, el 75 % de los días registró un 100 % de observaciones horarias válidas (24 de 24 posibles), con una media de 99.69 % y una desviación estándar muy pequeña (3.07 puntos porcentuales). Esto indica una base de datos de alta calidad, pero también implica que las variables Daily Obs Count y Percent Complete tienen muy poca variabilidad real para explicar los cambios en la concentración de CO, lo que anticipa un aporte predictivo limitado de estas variables.

### 3.2 Matriz de correlación

La Figura 3 muestra la matriz de correlación de Pearson entre la variable objetivo y las principales variables numéricas disponibles. Se observa una correlación prácticamente perfecta entre CO y el Daily AQI Value (r = 0.99), lo cual es consistente con el hecho de que el AQI se calcula directamente a partir de la concentración del contaminante; por ello, y como se explicó en la metodología, esta variable fue excluida del modelo para evitar fuga de información. En contraste, las variables temporales (Month, Day_of_Year, Day_of_Week) muestran correlaciones prácticamente nulas con la concentración de CO (entre -0.03 y 0.00), y las variables de completitud (Percent Complete, Daily Obs Count) muestran una correlación débil y negativa (-0.06). Finalmente, se confirma la correlación casi perfecta entre Daily Obs Count y Percent Complete (r ≈ 1.00), que motivó la exclusión de la primera del modelo final.

![Figura 3. Matriz de correlación entre la variable objetivo y las principales variables numéricas.](imagenes/figura3_matriz_correlacion.png)

*Figura 3. Matriz de correlación entre la variable objetivo y las principales variables numéricas.*

![Figura 4. Diagramas de dispersión de cada característica utilizada frente a la concentración de CO.](imagenes/figura4_dispersion_variables.png)

*Figura 4. Diagramas de dispersión de cada característica utilizada frente a la concentración de CO.*

Los diagramas de dispersión de la Figura 4 son coherentes con las correlaciones reportadas: no se observa ningún patrón lineal claro entre las variables temporales o de completitud y la concentración de CO; los puntos se distribuyen de forma prácticamente horizontal (sin pendiente aparente) para Month, Day_of_Year y Day_of_Week, y de forma muy concentrada en el extremo derecho para Percent Complete, reflejando la baja variabilidad de esta última variable.

### 3.3 Modelo de regresión lineal múltiple

El modelo entrenado sobre el conjunto de entrenamiento (70 % de los datos, n = 255) produjo un intercepto β₀ = 0.6373 y los coeficientes reportados en la Tabla II.

| Variable | Coeficiente (β) |
|---|---|
| Month | -0.018675 |
| Day_of_Year | 0.000651 |
| Day_of_Week | -0.000038 |
| Percent Complete | -0.003389 |

*Tabla II. Coeficientes estimados del modelo de regresión lineal múltiple.*

La interpretación formal de estos coeficientes sería: por cada mes adicional del año, la concentración de CO disminuye en promedio 0.0187 ppm, manteniendo las demás variables constantes; por cada día adicional del año, aumenta en promedio 0.00065 ppm; el día de la semana prácticamente no influye (coeficiente cercano a cero); y por cada punto porcentual adicional de completitud de las observaciones, la concentración de CO disminuye en promedio 0.0034 ppm. Sin embargo, como se muestra en los resultados de OLS, ninguno de estos coeficientes es estadísticamente significativo, por lo que estas magnitudes no deben interpretarse como efectos reales y consistentes, sino como el resultado del ruido muestral en ausencia de una relación lineal genuina entre estas variables y el CO.

### 3.4 Evaluación del modelo sobre el conjunto de prueba

| Métrica | Valor |
|---|---|
| MAE (Error Absoluto Medio) | 0.0799 ppm |
| MSE (Error Cuadrático Medio) | 0.0199 ppm² |
| RMSE (Raíz del Error Cuadrático Medio) | 0.1411 ppm |
| R² (conjunto de prueba) | -0.0133 |

*Tabla III. Métricas de error del modelo de regresión lineal sobre el conjunto de prueba (n = 110).*

El RMSE del modelo (0.141 ppm) es prácticamente idéntico a la desviación estándar del propio conjunto de prueba (0.1409 ppm), lo que confirma numéricamente que el modelo no logra explicar la variabilidad de la concentración de CO: en la práctica, predecir simplemente el valor promedio habría producido un error similar. Esto se traduce en un R² negativo (-0.0133), lo que significa que, sobre los datos de prueba, el modelo lineal ajustado predice ligeramente peor que una predicción constante igual a la media del conjunto de entrenamiento.

![Figura 5. Concentración de CO real vs. predicha (conjunto de prueba).](imagenes/figura5_real_vs_predicho.png)

*Figura 5. Concentración de CO real vs. predicha (conjunto de prueba). La línea discontinua roja representa la predicción perfecta (y = x).*

La Figura 5 ilustra claramente esta limitación: las predicciones del modelo (eje vertical) se concentran en una banda muy estrecha, entre 0.28 y 0.38 ppm, independientemente del valor real observado (eje horizontal), que varía entre 0.1 y 1.2 ppm. En otras palabras, el modelo se comporta de forma muy similar a una predicción constante cercana a la media histórica, y es incapaz de anticipar los días con concentraciones elevadas (por ejemplo, el punto con CO real = 1.2 ppm fue predicho en apenas 0.29 ppm).

### 3.5 Análisis de residuos

![Figura 6. Histograma de los residuos del modelo (conjunto de prueba).](imagenes/figura6_histograma_residuos.png)

*Figura 6. Histograma de los residuos del modelo (conjunto de prueba).*

![Figura 7. Residuos frente a los valores predichos (evaluación visual de homoscedasticidad).](imagenes/figura7_residuos_vs_predichos.png)

*Figura 7. Residuos frente a los valores predichos (evaluación visual de homoscedasticidad).*

El histograma de residuos (Figura 6) muestra una distribución con asimetría positiva y una cola larga hacia la derecha, con residuos promedio de 0.0123 ppm y desviación estándar de 0.1413 ppm: la mayoría de los residuos se agrupan cerca de cero o son ligeramente negativos, pero existen varios residuos grandes y positivos (hasta 0.9 ppm), correspondientes precisamente a los días con concentraciones de CO reales elevadas que el modelo no logró anticipar. El diagrama de residuos frente a valores predichos (Figura 7) refuerza esta lectura: en lugar de una nube de puntos dispersa aleatoriamente alrededor de cero (lo que indicaría homoscedasticidad, un supuesto clave de la regresión lineal), se observa una franja vertical muy estrecha de valores predichos (0.28–0.38 ppm) con residuos que se abren en abanico hacia arriba, evidenciando que el modelo no está capturando en absoluto la señal asociada a los picos de concentración.

### 3.6 Resultados del ajuste por mínimos cuadrados ordinarios (OLS)

El ajuste por OLS sobre el mismo conjunto de entrenamiento confirma los hallazgos anteriores desde una perspectiva de inferencia estadística (Tabla IV).

| Variable | Coef. | Error est. | t | P>\|t\| |
|---|---|---|---|---|
| const | 0.6373 | 0.279 | 2.288 | 0.023 |
| Month | -0.0187 | 0.032 | -0.582 | 0.561 |
| Day_of_Year | 0.0007 | 0.001 | 0.621 | 0.535 |
| Day_of_Week | -3.761e-05 | 0.005 | -0.008 | 0.994 |
| Percent Complete | -0.0034 | 0.003 | -1.211 | 0.227 |

*Tabla IV. Resumen del modelo OLS: coeficientes, errores estándar, estadístico t y valor p (n = 255).*

El R² dentro de la muestra de entrenamiento es de apenas 0.008 (R² ajustado = -0.008), y el estadístico F conjunto del modelo (F = 0.512, p = 0.727) no permite rechazar la hipótesis nula de que todos los coeficientes son simultáneamente iguales a cero; es decir, el conjunto de predictores utilizado no explica de forma estadísticamente significativa la variabilidad de la concentración de CO. A nivel individual, todos los valores p son muy superiores a 0.05 (el más bajo corresponde a Percent Complete, con p = 0.227), por lo que ninguna variable predictora resulta significativa. Además, las pruebas de normalidad de los residuos (Jarque-Bera) muestran una asimetría (skewness) de 3.24 y una curtosis de 19.99, muy alejadas de los valores esperados bajo normalidad (0 y 3, respectivamente), lo que indica que el supuesto de normalidad de los residuos tampoco se cumple. El número de condición del modelo (6.88 × 10³) es menor que el obtenido antes de excluir Daily Obs Count, lo que confirma que la corrección aplicada en la metodología redujo la multicolinealidad, aunque no mejoró la capacidad explicativa del modelo, ya que el problema de fondo es la ausencia de relación lineal entre los predictores disponibles y la variable objetivo, y no la colinealidad entre ellos.

### 3.7 Árbol de decisión de regresión (práctica complementaria, datos sintéticos)

Como práctica adicional se generó un conjunto de datos sintético de 100 observaciones y 6 características (make_regression, de las cuales 3 son realmente informativas), sobre el cual se entrenó un árbol de decisión de regresión con profundidad máxima de 5 niveles. El modelo obtuvo un error cuadrático medio (MSE) de 7 931.57 sobre el conjunto de prueba (valores en la escala arbitraria del generador sintético) y la Figura 8 muestra la importancia relativa que el árbol asignó a cada una de las seis características.

![Figura 8. Importancia relativa de las características en el árbol de decisión (datos sintéticos).](imagenes/figura8_importancia_arbol.png)

*Figura 8. Importancia relativa de las características en el árbol de decisión (datos sintéticos).*

Como es de esperar en un árbol entrenado sobre datos generados sintéticamente con variables informativas conocidas, el modelo concentra la mayor parte de la importancia en un subconjunto reducido de características, mientras que las variables no informativas reciben una importancia cercana a cero. Se reitera que estos resultados corresponden a un conjunto de datos artificial, generado únicamente con fines de práctica sobre árboles de decisión, y no son comparables con el desempeño del modelo de regresión lineal aplicado a la concentración real de CO.

## 4. Discusión

Los resultados obtenidos muestran, de manera consistente en todas las métricas calculadas (R² de prueba, R² de OLS, prueba F conjunta, prueba t individual y comparación visual de residuos), que el modelo de regresión lineal múltiple construido a partir de variables temporales (mes, día del año, día de la semana) y de completitud de las observaciones (porcentaje de datos válidos) no logra explicar de forma significativa la variabilidad de la concentración máxima diaria de CO en la estación Salinas 3 durante 2022. El RMSE del modelo es esencialmente igual a la desviación estándar de los datos de prueba, y el R² fuera de muestra es negativo, lo que equivale a decir que una predicción ingenua basada únicamente en la media histórica habría tenido un desempeño comparable o incluso ligeramente mejor.

Esta falta de poder explicativo tiene una explicación razonable desde el punto de vista de la química atmosférica: la concentración de CO en aire ambiental depende principalmente de fuentes de combustión (tráfico vehicular, quema de biomasa, actividad industrial) y de condiciones meteorológicas que favorecen o dificultan la dispersión de contaminantes (velocidad y dirección del viento, temperatura, presión atmosférica, inversiones térmicas), ninguna de las cuales está representada en las variables utilizadas. Las variables temporales (mes, día del año, día de la semana) son, en el mejor de los casos, proxies muy indirectos de patrones estacionales o de actividad humana, y su correlación prácticamente nula con la variable objetivo (entre -0.03 y 0.00, según la Figura 3) así lo confirma. De manera similar, las variables de completitud de las observaciones (Percent Complete, Daily Obs Count) son indicadores de la calidad del proceso de medición, no de la magnitud del fenómeno medido, por lo que resulta razonable que aporten muy poco valor predictivo; a esto se suma que, en esta base de datos, dichas variables presentan muy poca variabilidad real (el 75 % de los días alcanzó 100 % de completitud), lo que limita aún más su capacidad de discriminar entre días con distintos niveles de CO.

Un hallazgo relevante del proceso metodológico fue la detección de multicolinealidad casi perfecta entre Daily Obs Count y Percent Complete (r ≈ 0.9999), la cual fue corregida excluyendo la primera variable del conjunto de predictores. Esta corrección redujo el número de condición del modelo OLS de 9.12 × 10⁴ a 6.88 × 10³, mejorando la estabilidad numérica de la estimación; sin embargo, el R² de prueba solo mejoró marginalmente (de -0.0316 a -0.0133), lo que confirma que la multicolinealidad no era la causa principal del bajo desempeño del modelo, sino la ausencia de una relación lineal genuina entre los predictores disponibles y la variable objetivo. Este resultado ilustra un punto metodológico importante: corregir problemas de diseño del modelo (como la multicolinealidad) es necesario para una interpretación correcta de los coeficientes, pero no garantiza, por sí solo, una mejora sustancial en la capacidad predictiva cuando el conjunto de variables carece de señal explicativa relevante.

Asimismo, se verificó que el valor de Daily AQI Value mantiene una correlación de 0.99 con la concentración de CO, lo cual confirma la decisión metodológica (tomada siguiendo el taller original) de excluir esta variable como predictor: al tratarse de un índice calculado directamente a partir de la propia concentración del contaminante, su inclusión habría generado fuga de información (data leakage) y un modelo con métricas de error artificialmente buenas, pero sin ningún valor predictivo real ni capacidad de generalización.

Como limitaciones del presente análisis cabe señalar: (i) el estudio se restringe a una sola estación de monitoreo y a un único año, lo que impide evaluar la robustez del modelo ante distintos contextos geográficos o climáticos; (ii) no se dispone de variables meteorológicas ni de tráfico vehicular, que de acuerdo con la literatura sobre calidad del aire suelen ser las variables con mayor poder explicativo sobre la concentración de contaminantes como el CO; y (iii) el reducido tamaño de la muestra de prueba (110 observaciones) limita la potencia estadística de las pruebas de significancia. Como líneas de trabajo futuro, se recomienda incorporar variables exógenas de tipo meteorológico (temperatura, velocidad del viento, humedad relativa) y de actividad antropogénica (conteo de tráfico, día festivo/laborable), así como explorar modelos no lineales (árboles de decisión o ensambles, aplicados directamente sobre los datos reales de CO y no solo sobre datos sintéticos) que puedan capturar relaciones más complejas entre estas variables y la concentración del contaminante.

## 5. Referencias

[1] I. C. Parhuay Meza, "Taller de Regresión Lineal y Modelos de Predicción — Base de datos de CO, Monterey County, 2022," cuaderno de Google Colab, Universidad Peruana Cayetano Heredia, 2026, trabajo no publicado.

[2] U.S. Environmental Protection Agency, "Download Daily Data," Outdoor Air Quality Data, y "Basic Information about Carbon Monoxide (CO) Outdoor Air Pollution." [En línea]. Disponible: https://www.epa.gov/outdoor-air-quality-data/download-daily-data ; https://www.epa.gov/co-pollution/basic-information-about-carbon-monoxide-co-outdoor-air-pollution. [Accedido: sep. 2026].

[3] G. James, D. Witten, T. Hastie, and R. Tibshirani, *An Introduction to Statistical Learning*, 1st ed. New York, NY, USA: Springer, 2013.
