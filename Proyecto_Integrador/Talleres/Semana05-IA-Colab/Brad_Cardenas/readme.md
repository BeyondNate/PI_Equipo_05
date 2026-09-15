# Análisis de Regresión y Machine Learning

## Descripción

Este proyecto desarrolla un análisis de datos orientado a la predicción del **Consumo_Energia** a partir de cuatro variables: **Temperatura, Horas_Operacion, Carga y Humedad**.

El trabajo utiliza Python para explorar los datos, analizar las relaciones entre variables, construir modelos de regresión y evaluar sus resultados.

## Proceso realizado

El flujo de trabajo fue el siguiente:

1. **Carga de datos**
   Se cargó el archivo `Data_PI_regresion.csv` utilizando Pandas. El conjunto contiene 5000 registros y cinco variables numéricas.

2. **Análisis exploratorio**
   Se revisó la estructura del DataFrame, los tipos de datos y las estadísticas descriptivas. También se utilizaron gráficos de distribución, `pairplot` y gráficos de dispersión para observar las relaciones entre las variables.

3. **Análisis de correlación**
   Se calculó la matriz de correlación de las variables numéricas y se representó mediante un mapa de calor para facilitar su interpretación.

4. **Preparación del modelo**
   Se definieron las cuatro variables de entrada como predictoras (`X`) y `Consumo_Energia` como variable objetivo (`y`).

5. **División de datos**
   Los datos se dividieron en un 70 % para entrenamiento y un 30 % para prueba, utilizando `random_state=101` para mantener la reproducibilidad.

6. **Regresión lineal**
   Se entrenó un modelo `LinearRegression` de Scikit-learn con los datos de entrenamiento. Se obtuvieron el intercepto y los coeficientes de cada variable.

7. **Análisis estadístico**
   Se calcularon los errores estándar y las estadísticas `t` de los coeficientes para analizar la relación entre cada variable y el modelo.

8. **Evaluación**
   Se realizaron predicciones sobre los datos de prueba y se compararon los valores reales con los predichos. También se analizaron los residuos mediante histogramas y gráficos de residuos frente a valores predichos.

9. **Árbol de decisión**
   Se generaron datos artificiales mediante `make_regression` y se entrenó un `DecisionTreeRegressor` con una profundidad máxima de 5. Posteriormente se calculó el MSE y la importancia relativa de las características.

10. **OLS con Statsmodels**
    Finalmente, se utilizó `statsmodels` para construir un modelo de Mínimos Cuadrados Ordinarios agregando una constante a las variables predictoras y obteniendo el resumen estadístico completo.

## Flujo del proyecto

```text
Dataset CSV
    |
    v
Carga y revisión de datos
    |
    v
Análisis exploratorio
    |
    v
Correlación y visualización
    |
    v
Definición de X e y
    |
    v
División 70% / 30%
    |
    v
Regresión lineal
    |
    v
Coeficientes y errores estándar
    |
    v
Predicciones
    |
    v
Análisis de residuos
    |
    v
Evaluación del modelo
    |
    +----------------------+
    |                      |
    v                      v
Árbol de decisión       Modelo OLS
    |                      |
    v                      v
MSE e importancia       Resumen estadístico
de características
```

## Lo aprendido

Durante el desarrollo se trabajó con:

* Carga y manipulación de datos utilizando Pandas.
* Análisis exploratorio y estadística descriptiva.
* Visualización de datos con Matplotlib y Seaborn.
* Interpretación de matrices de correlación.
* Separación de variables predictoras y variable objetivo.
* División de datos en entrenamiento y prueba.
* Entrenamiento de modelos de regresión lineal.
* Interpretación de coeficientes, errores estándar y estadísticas `t`.
* Evaluación mediante predicciones y análisis de residuos.
* Uso básico de árboles de decisión para regresión.
* Comparación de diferentes herramientas para regresión, principalmente Scikit-learn y Statsmodels.

## Tecnologías utilizadas

* Python
* NumPy
* Pandas
* Matplotlib
* Seaborn
* Scikit-learn
* Statsmodels
* Google Colab / Jupyter Notebook

## Archivo principal

`Diseño_AI.ipynb`

El notebook contiene todo el proceso de análisis, entrenamiento, evaluación y modelado desarrollado en el proyecto.

