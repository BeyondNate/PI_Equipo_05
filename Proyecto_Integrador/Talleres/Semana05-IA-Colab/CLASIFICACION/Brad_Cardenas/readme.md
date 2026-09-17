# Predicción de la Concentración Máxima Diaria de CO mediante Regresión Lineal Múltiple: Caso Salt Lake City, UT (2022)

## Introducción

La contaminación del aire es un problema ambiental y de salud pública de gran relevancia, especialmente en zonas urbanas con alta densidad vehicular e industrial. Uno de los contaminantes más monitoreados es el monóxido de carbono (CO), un gas incoloro e inodoro que, en concentraciones elevadas, puede afectar significativamente la salud humana al reducir la capacidad de la sangre para transportar oxígeno.

El presente informe tiene como objetivo desarrollar un modelo de regresión lineal múltiple capaz de predecir la **concentración máxima diaria de CO en un periodo de 8 horas** a partir de otras variables ambientales y de calidad del aire registradas en la estación de monitoreo de Salt Lake City, Utah, durante el año 2022. Se busca, además, identificar cuáles de estas variables tienen mayor incidencia sobre el comportamiento del CO, con el fin de comprender mejor las relaciones entre los distintos contaminantes y condiciones registradas.

Los datos utilizados provienen del sistema público de datos de calidad del aire de la Agencia de Protección Ambiental de los Estados Unidos (EPA) [1].

## Metodología

El desarrollo del proyecto siguió las siguientes etapas:

1. **Recolección de datos:** se utilizó el conjunto de datos históricos de calidad del aire correspondiente a la estación de Salt Lake City, UT, para el año 2022, descargado desde la plataforma AirData de la EPA [1].

2. **Análisis exploratorio de datos (EDA):** se examinó la estructura del dataset (`.info()`, `.describe()`), se generaron gráficos de dispersión por pares (*pairplot*), histogramas y curvas de densidad para observar la distribución de la variable objetivo, así como un mapa de calor de correlaciones entre variables numéricas.

3. **Preparación de los datos:** se excluyeron del conjunto de características (X) las columnas no numéricas o irrelevantes para la regresión (fechas, identificadores de sitio, códigos de método, nombres de condado/estado, etc.), dejando únicamente variables numéricas. La variable objetivo (y) se definió como la concentración máxima diaria de CO en 8 horas.

4. **División de datos:** el conjunto de datos se dividió en entrenamiento (70 %) y prueba (30 %) utilizando `train_test_split` de scikit-learn, con una semilla fija (`random_state = 123`) para garantizar reproducibilidad.

5. **Entrenamiento del modelo:** se implementó un modelo de **Regresión Lineal Múltiple** (`LinearRegression` de scikit-learn) sobre el conjunto de entrenamiento.

6. **Evaluación de significancia:** se calcularon los errores estándar y estadísticos t de cada coeficiente para determinar la importancia relativa de cada variable predictora sobre la variable objetivo.

7. **Evaluación del modelo:** se midió el desempeño del modelo mediante el coeficiente de determinación (R²) tanto en entrenamiento como en prueba, y se calcularon las métricas de error: Error Absoluto Medio (MAE), Error Cuadrático Medio (MSE) y Raíz del Error Cuadrático Medio (RMSE). Adicionalmente, se realizó un análisis de residuos para verificar los supuestos del modelo (normalidad y homocedasticidad).

## Resultados

### Distribución de la variable objetivo

<!-- Insertar aquí: histograma y curva de densidad de "Daily Max 8-hour CO Concentration" -->

**Interpretación:** *(Describir aquí la forma de la distribución: si es simétrica o sesgada, si hay valores atípicos, y qué tan dispersos están los valores de concentración de CO respecto a su media)*

---

### Matriz de correlación

<!-- Insertar aquí: mapa de calor (heatmap) de correlaciones entre variables numéricas -->

**Interpretación:** *(Señalar qué variables muestran mayor correlación positiva o negativa con la concentración de CO, y si existe multicolinealidad relevante entre las variables predictoras)*

---

### Coeficientes del modelo y significancia estadística

<!-- Insertar aquí: tabla `cdf` con Coefficients, Standard Error y t-statistic -->

**Interpretación:** *(Explicar qué variables resultaron más significativas según el estadístico t, y qué dirección de efecto tienen sus coeficientes sobre el CO)*

---

### Variables más influyentes vs. variable objetivo

<!-- Insertar aquí: gráfico de dispersión 2x2 de las 4 variables más importantes vs. CO -->

**Interpretación:** *(Comentar el tipo de relación observada — lineal, no lineal, dispersa — entre cada una de estas variables y la concentración de CO)*

---

### Ajuste del modelo (R² de entrenamiento)

<!-- Insertar aquí: valor impreso de R² de entrenamiento -->

**Interpretación:** *(Indicar qué porcentaje de la variabilidad del CO es explicado por el modelo en el conjunto de entrenamiento)*

---

### Valores reales vs. predichos (conjunto de prueba)

<!-- Insertar aquí: gráfico de dispersión de valores reales vs. predichos con línea de referencia -->

**Interpretación:** *(Evaluar qué tan cerca están los puntos de la línea diagonal ideal, y si el modelo tiende a sobreestimar o subestimar en ciertos rangos)*

---

### Análisis de residuos

<!-- Insertar aquí: histograma de residuos -->

**Interpretación:** *(Indicar si los residuos siguen una distribución aproximadamente normal, lo cual respalda los supuestos del modelo de regresión lineal)*

<!-- Insertar aquí: gráfico de residuos vs. valores predichos -->

**Interpretación:** *(Comentar si los residuos se distribuyen aleatoriamente alrededor de cero (homocedasticidad) o si se observa algún patrón que sugiera un problema en el modelo)*

---

### Métricas finales de desempeño

<!-- Insertar aquí: valores impresos de MAE, MSE, RMSE y R² sobre el conjunto de prueba -->

**Interpretación:** *(Comparar estos valores con la escala de la variable objetivo para juzgar si el error del modelo es aceptable, y comparar el R² de prueba con el de entrenamiento para evaluar sobreajuste)*

## Discusión

*(Sección opcional: aquí puedes reflexionar sobre las limitaciones del modelo — por ejemplo, el uso de un modelo lineal para relaciones que podrían ser no lineales, la posible multicolinealidad entre variables ambientales, el tamaño de la muestra, o la falta de variables externas como tráfico vehicular o condiciones meteorológicas detalladas. También puedes proponer mejoras futuras, como probar modelos no lineales, regularización (Ridge/Lasso) o incluir más estaciones y años de datos.)*

## Referencias

[1] U.S. Environmental Protection Agency, "Download Daily Data," *AirData*. [Online]. Available: https://www.epa.gov/outdoor-air-quality-data/download-daily-data. [Accessed: 17-Sep-2026].
