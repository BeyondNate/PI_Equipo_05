# Informe: Análisis mediante Regresión Lineal

## 1. Metodología

El análisis se realizó utilizando un conjunto de datos relacionado con la calidad del aire. El conjunto contiene **355 registros y 21 variables**, entre las que se encuentran la concentración máxima diaria de ozono en 8 horas, el valor diario del índice de calidad del aire (AQI), cantidad de observaciones, porcentaje de datos completos y diferentes datos de ubicación.

Para el procesamiento de los datos se utilizaron principalmente `pandas`, `numpy`, `matplotlib` y `seaborn`. Estas librerías permitieron cargar el archivo, explorar su estructura, obtener estadísticas descriptivas y visualizar las relaciones entre las variables.

### 1.1 Exploración de los datos

Primero se cargó el conjunto de datos mediante `pandas` y se utilizaron funciones básicas como:

* `head()` para observar los primeros registros.
* `info()` para conocer la cantidad de registros, variables y tipos de datos.
* `describe()` para obtener estadísticas descriptivas.
* `pairplot()` para visualizar las relaciones entre las variables.

El conjunto contiene variables numéricas y categóricas. Algunas variables presentan valores constantes debido a que los registros corresponden al mismo sitio de medición.

### 1.2 Análisis de correlación

Se analizó la relación entre las variables numéricas mediante correlaciones y gráficos de dispersión. Esta etapa permitió identificar qué variables presentan una mayor relación lineal con la concentración de ozono.

La correlación se utilizó como una etapa exploratoria previa a la construcción del modelo, ya que permite observar posibles relaciones entre las variables antes de realizar la predicción.

### 1.3 Regresión lineal

Para construir el modelo se separaron las variables de entrada (**X**) y la variable objetivo (**y**). Posteriormente, los datos se dividieron en:

* **70 % para entrenamiento**, utilizado para ajustar el modelo.
* **30 % para prueba**, utilizado para evaluar las predicciones.

La división se realizó utilizando `train_test_split` con `random_state=123`, permitiendo mantener la reproducibilidad del experimento.

El modelo se construyó mediante `LinearRegression()` de `scikit-learn` y se entrenó utilizando los datos de entrenamiento. Se analizaron los coeficientes obtenidos para observar la dirección de la relación entre las variables de entrada y la variable objetivo.

### 1.4 Evaluación del modelo

Una vez entrenado el modelo, se generaron predicciones sobre los datos de prueba. Estas predicciones fueron comparadas con los valores reales mediante gráficos de dispersión.

También se analizaron los **residuos**, definidos como la diferencia entre los valores reales y los valores predichos. Este análisis permite observar posibles patrones en los errores del modelo.

### 1.5 Análisis complementario

Finalmente, se generaron datos artificiales mediante `make_regression` para experimentar con un conjunto controlado de características.

Sobre estos datos se aplicó un **árbol de decisión para regresión**, utilizando `DecisionTreeRegressor`, y se calculó el error cuadrático medio (MSE). También se obtuvo la importancia relativa de las características.

Además, se utilizó `statsmodels` para aplicar el método de mínimos cuadrados mediante `OLS`, obteniendo un resumen estadístico del modelo.

---

## 2. Resultados

La exploración inicial permitió identificar que el conjunto de datos contiene **355 observaciones y 21 columnas**. La variable de concentración máxima diaria de ozono en 8 horas presenta valores entre aproximadamente **0.0 y 0.1 ppm**, mientras que el valor diario del AQI presenta valores entre **8 y 67**.

El análisis exploratorio permitió observar las relaciones entre las variables numéricas y seleccionar las características utilizadas para el modelo de regresión lineal.

La regresión lineal permitió obtener coeficientes para las variables utilizadas como entrada. El signo de cada coeficiente indica la dirección de la relación dentro del modelo: un coeficiente positivo representa un aumento estimado de la variable objetivo cuando aumenta la característica, manteniendo las demás constantes.

La evaluación mediante los datos de prueba permitió generar predicciones y compararlas visualmente con los valores reales. Asimismo, el análisis de residuos permitió observar la distribución de los errores y buscar posibles patrones.

En el análisis complementario, `make_regression` generó **100 muestras, 6 características y 3 características informativas**, incorporando ruido aleatorio para simular datos más realistas. El árbol de decisión permitió obtener una medida de importancia relativa para cada característica y calcular el error cuadrático medio de sus predicciones.

---

## 3. Discusión

El análisis muestra cómo la regresión lineal puede utilizarse para estudiar relaciones entre variables y realizar predicciones sobre una variable objetivo. La exploración inicial es importante porque permite conocer la estructura de los datos y detectar qué variables pueden ser útiles para el modelo.

El análisis de residuos complementa la evaluación de las predicciones, ya que no solo interesa obtener valores predichos, sino también observar el comportamiento de los errores.

El uso de datos artificiales permitió complementar el análisis con un escenario controlado. En este caso, se conocían de antemano las características informativas utilizadas para generar la variable objetivo, lo que permite observar cómo un árbol de decisión identifica la importancia relativa de las características.

---

## 4. Referencias

[1] F. Pedregosa *et al.*, “Scikit-learn: Machine Learning in Python,” *Journal of Machine Learning Research*, vol. 12, pp. 2825–2830, 2011.

[2] W. McKinney, *Python for Data Analysis: Data Wrangling with pandas, NumPy, and Jupyter*, 3rd ed. Sebastopol, CA, USA: O’Reilly Media, 2022.

[3] S. Seabold and J. Perktold, “Statsmodels: Econometric and Statistical Modeling with Python,” in *Proceedings of the 9th Python in Science Conference*, 2010, pp. 92–96.

[4] U.S. Environmental Protection Agency, “Air Quality System (AQS) Data,” U.S. EPA.
