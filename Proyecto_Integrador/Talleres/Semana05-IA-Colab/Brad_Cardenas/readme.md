# Análisis de Regresión y Machine Learning

## Descripción

Este proyecto desarrolla un análisis de datos para predecir el **Consumo_Energia** utilizando las variables **Temperatura, Horas_Operacion, Carga y Humedad**.

El trabajo se realizó en Python y comprende desde el análisis exploratorio de los datos hasta la construcción y evaluación de modelos de regresión.

## Proceso

El flujo seguido fue:

```text
Carga del dataset
      |
      v
Exploración de los datos
      |
      v
Visualización y correlación
      |
      v
Definición de variables X e y
      |
      v
División entrenamiento / prueba
      |
      v
Regresión lineal
      |
      v
Evaluación y análisis de residuos
      |
      v
Árbol de decisión
      |
      v
Modelo OLS
```

## 1. Carga y exploración de datos

Se cargó el archivo `Data_PI_regresion.csv` utilizando Pandas. El conjunto contiene 5000 registros y las variables utilizadas para el análisis.

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

%matplotlib inline
```

## 2. Visualización de las relaciones entre variables

Se utilizó un `pairplot` para observar la distribución de las variables y sus relaciones.

```python
sns.pairplot(df1)
```

![Ver imagen del Pair Plot](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana05-IA-Colab/Brad_Cardenas/capturas/Relacion_Variables.png)

## 3. Matriz de correlación

Se calculó la correlación entre las variables numéricas y se representó mediante un mapa de calor.

```python
plt.figure(figsize=(10,7))
sns.heatmap(numeric__df1.corr(), annot=True, linewidths=2)
```

![Ver matriz de correlación](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana05-IA-Colab/Brad_Cardenas/capturas/MatrizCorrelacion.png)

## 4. Regresión lineal

Se definieron las variables predictoras y la variable objetivo `Consumo_Energia`. Los datos se dividieron en 70 % para entrenamiento y 30 % para prueba.

```python
X = df1[['Temperatura',
         'Horas_Operacion',
         'Carga',
         'Humedad']]

y = df1['Consumo_Energia']

X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.3,
    random_state=101
)
```

Posteriormente se entrenó el modelo de regresión lineal:

```python
lm = LinearRegression()
lm.fit(X_train, y_train)
```

## 5. Relación entre las variables y el consumo

Se generaron gráficos de dispersión para observar la relación de cada variable predictora con `Consumo_Energia`.

```python
l = list(cdf.index)

from matplotlib import gridspec

fig = plt.figure(figsize=(18, 10))
gs = gridspec.GridSpec(2, 2)

ax0 = plt.subplot(gs[0])
ax0.scatter(df1[l[0]], df1['Consumo_Energia'])
ax0.set_title(l[0] + " vs. Consumo_Energia")

ax1 = plt.subplot(gs[1])
ax1.scatter(df1[l[1]], df1['Consumo_Energia'])
ax1.set_title(l[1] + " vs. Consumo_Energia")

ax2 = plt.subplot(gs[2])
ax2.scatter(df1[l[2]], df1['Consumo_Energia'])
ax2.set_title(l[2] + " vs. Consumo_Energia")

ax3 = plt.subplot(gs[3])
ax3.scatter(df1[l[3]], df1['Consumo_Energia'])
ax3.set_title(l[3] + " vs. Consumo_Energia")
```

![Ver gráficos de las variables predictoras](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana05-IA-Colab/Brad_Cardenas/capturas/VariablesPredictorias.png)

## 6. Valores reales frente a valores predichos

Después de realizar las predicciones, se compararon los valores reales con los obtenidos por el modelo.

```python
plt.figure(figsize=(10,7))
plt.title("Consumo de energía real vs. el predicho")
plt.xlabel("Consumo de energía real")
plt.ylabel("Consumo de energía predicho")
plt.scatter(x=y_test, y=predictions)
```

![Ver gráfico real vs. predicho](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana05-IA-Colab/Brad_Cardenas/capturas/Real_Ficticio.png)

## 7. Análisis de residuos

Se analizaron los residuos para comprobar el comportamiento de los errores del modelo.

```python
plt.figure(figsize=(10,7))
plt.title("Histograma de residuos para verificar la normalidad")
plt.xlabel("Residuos")
plt.ylabel("Densidad del kernel")

sns.distplot([y_test-predictions])
```

![Ver histograma de residuos](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana05-IA-Colab/Brad_Cardenas/capturas/Analisis_Residuos.png)

También se analizaron los residuos frente a los valores predichos para observar posibles patrones.

![Ver gráfico de residuos](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana05-IA-Colab/Brad_Cardenas/capturas/GraficoResiduos.png)

## 8. Árbol de decisión

Como segundo enfoque se utilizó un árbol de decisión para regresión. Se generaron datos de prueba y se entrenó un `DecisionTreeRegressor` con una profundidad máxima de 5.

El modelo fue evaluado mediante el error cuadrático medio (MSE) y se analizaron las importancias de las características.

![Ver gráfico de Árbol de decisión](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana05-IA-Colab/Brad_Cardenas/capturas/ArbolDecision.png)


## 9. Modelo OLS

Finalmente se utilizó `statsmodels` para construir un modelo de Mínimos Cuadrados Ordinarios (OLS). Esto permitió obtener información estadística adicional sobre los coeficientes del modelo, sus errores estándar y las estadísticas `t`.

## Lo aprendido

Durante el desarrollo se aprendió a:

* Cargar y organizar datos con Pandas.
* Realizar un análisis exploratorio.
* Utilizar gráficos para interpretar los datos.
* Analizar correlaciones entre variables.
* Separar variables predictoras y objetivo.
* Dividir datos para entrenamiento y prueba.
* Construir una regresión lineal.
* Interpretar coeficientes y errores estándar.
* Evaluar predicciones mediante gráficos.
* Analizar residuos.
* Utilizar árboles de decisión para regresión.
* Trabajar con modelos OLS mediante Statsmodels.
* Comparar el uso de Scikit-learn y Statsmodels para el análisis estadístico.

## Tecnologías utilizadas

* Python
* NumPy
* Pandas
* Matplotlib
* Seaborn
* Scikit-learn
* Statsmodels
* Google Colab

## Archivo principal

[`Diseño_AI.ipynb`](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana05-IA-Colab/Brad_Cardenas/Dise%C3%B1o_AI.ipynb)

El notebook contiene el código utilizado para realizar todo el proceso de análisis, modelado y evaluación.
