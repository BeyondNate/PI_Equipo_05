# Informe de Regresión Lineal y Random Forest para la predicción del AQI

## Introducción

El presente informe documenta el análisis realizado en el notebook
`LEONEL_URBANO_CASTILLO.ipynb`, cuyo objetivo es estudiar la relación
entre la concentración de ozono y el Índice de Calidad del Aire (AQI), y
evaluar modelos de aprendizaje supervisado para predecir dicho índice.

El conjunto de datos corresponde al archivo `ad_viz_plotval_data.csv`,
asociado a la estación **Hermiston - Municipal Airport (HMA), Oregon,
EE.UU.**, durante el período **abril--septiembre de 2023**. El dataset
original contiene 21 columnas; para el análisis se seleccionaron cuatro
variables numéricas: `Concentracion_Ozono`, `Obs_Diarias`,
`Pct_Completo` y `AQI`. La variable `AQI` se utiliza como variable
objetivo, mientras que las tres primeras se utilizan como variables
predictoras.

El análisis comprende una exploración descriptiva y gráfica de los
datos, una **Regresión Lineal**, un **Random Forest Regressor** y un
análisis adicional mediante **Mínimos Cuadrados Ordinarios (OLS)**.
También se evalúan los modelos mediante MAE, MSE, RMSE y R², además de
analizar los residuos y la importancia de las variables.

------------------------------------------------------------------------

## Metodología

### 1. Carga y selección de los datos

El archivo CSV se carga mediante `pd.read_csv()` y posteriormente se
seleccionan las cuatro columnas utilizadas en el estudio. Las variables
se renombran para facilitar su manejo:

-   `Concentracion_Ozono`: concentración máxima diaria de ozono.
-   `Obs_Diarias`: número de observaciones diarias.
-   `Pct_Completo`: porcentaje de datos completos.
-   `AQI`: Índice de Calidad del Aire, utilizado como variable objetivo.

La estructura obtenida en el notebook es:

``` text
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 158 entries, 0 to 157
Data columns (total 4 columns):
 #   Column               Non-Null Count  Dtype  
---  ------               --------------  -----  
 0   Concentracion_Ozono  158 non-null   float64
 1   Obs_Diarias          158 non-null   int64  
 2   Pct_Completo         158 non-null   float64
 3   AQI                  158 non-null   int64  
dtypes: float64(2), int64(2)
memory usage: 5.1 KB
```

Por lo tanto, se trabajó con **158 observaciones y 4 variables**, sin
valores nulos en las columnas seleccionadas.

### 2. Estadística descriptiva

Se utilizó `df.describe().round(4)` para obtener los principales
estadísticos descriptivos.

            Concentracion_Ozono   Obs_Diarias   Pct_Completo        AQI
  ------- --------------------- ------------- -------------- ----------
  count                158.0000      158.0000       158.0000   158.0000
  mean                   0.0464       16.9557        99.7342    44.5949
  std                    0.0091        0.3966         2.3794    11.6652
  min                    0.0150       13.0000        76.0000    14.0000
  25%                    0.0400       17.0000       100.0000    37.0000
  50%                    0.0470       17.0000       100.0000    44.0000
  75%                    0.0530       17.0000       100.0000    49.0000
  max                    0.0660       17.0000       100.0000    87.0000

**Interpretación:** la concentración media de ozono es 0.0464, mientras
que el AQI presenta una media de 44.5949 y una desviación estándar de
11.6652. `Obs_Diarias` y `Pct_Completo` presentan poca variabilidad,
debido a que la mayoría de los registros tienen 17 observaciones y 100 %
de completitud.

### 3. Exploración gráfica

Se utilizaron gráficos exploratorios para observar la distribución del
AQI y las relaciones entre las variables.

#### Figura 1. Pairplot de las variables

![Pairplot](imagenes/01_pairplot.png)

**Interpretación:** el pairplot muestra las relaciones bivariadas entre
las variables. Se observa una relación positiva marcada entre
`Concentracion_Ozono` y `AQI`, mientras que `Obs_Diarias` y
`Pct_Completo` presentan variaciones muy limitadas y una relación
prácticamente nula con el AQI. También se observa que `Obs_Diarias` y
`Pct_Completo` se mueven de forma prácticamente idéntica, situación que
posteriormente se confirma mediante la matriz de correlación.

#### Figura 2. Distribución del AQI diario

![Distribución del AQI](imagenes/02_histograma_aqi.png)

**Interpretación:** el histograma concentra la mayor cantidad de
observaciones aproximadamente entre 30 y 50 de AQI. También existe una
cola hacia valores superiores, llegando hasta aproximadamente 87. Esto
indica que la distribución no es perfectamente simétrica y presenta
algunos registros con AQI elevado.

#### Figura 3. Función de densidad del AQI

![Densidad del AQI](imagenes/03_densidad_aqi.png)

**Interpretación:** la curva de densidad presenta su mayor concentración
alrededor de los valores medios del AQI, aproximadamente en la zona de
40--50. La cola hacia valores mayores muestra que existen observaciones
con AQI considerablemente superior al grupo principal.

### 4. Matriz de correlación

Se calculó la correlación de Pearson entre las variables numéricas.

  ----------------------------------------------------------------------------------------
                          Concentracion_Ozono    Obs_Diarias   Pct_Completo            AQI
  --------------------- --------------------- -------------- -------------- --------------
  Concentracion_Ozono                  1.0000        -0.0302        -0.0302         0.9503

  Obs_Diarias                         -0.0302         1.0000         1.0000        -0.0080

  Pct_Completo                        -0.0302         1.0000         1.0000        -0.0080

  AQI                                  0.9503        -0.0080        -0.0080         1.0000
  ----------------------------------------------------------------------------------------

#### Figura 4. Mapa de calor de correlaciones

![Heatmap](imagenes/04_heatmap_correlaciones.png)

**Interpretación:** la correlación entre `Concentracion_Ozono` y `AQI`
es **0.9503**, lo que representa una relación lineal positiva muy fuerte
dentro de esta muestra. En contraste, `Obs_Diarias` y `Pct_Completo`
presentan una correlación de **-0.0080** con el AQI. Además, ambas
variables tienen una correlación de **1.0000 entre sí**, lo que
evidencia una fuerte redundancia entre estos dos predictores.

### 5. Definición de variables predictoras y variable objetivo

Se definió:

-   **X:** `Concentracion_Ozono`, `Obs_Diarias` y `Pct_Completo`.
-   **y:** `AQI`.

Las primeras observaciones mostradas por el notebook son:

        Concentracion_Ozono   Obs_Diarias   Pct_Completo
  --- --------------------- ------------- --------------
    0                 0.043            17          100.0
    1                 0.048            17          100.0
    2                 0.047            17          100.0
    3                 0.045            13           76.0
    4                 0.038            17          100.0

Y los primeros valores de `AQI` son:

        AQI
  --- -----
    0    40
    1    44
    2    44
    3    42
    4    35

### 6. División entrenamiento-prueba

Los datos se dividieron mediante `train_test_split()` utilizando
`test_size=0.3` y `random_state=123`. De acuerdo con el tamaño total de
158 observaciones, el notebook trabaja con **110 observaciones de
entrenamiento y 48 de prueba**, como se refleja también en el resultado
de OLS y en el tamaño de `predictions`.

**Dato derivado de la estructura del dataset:**

$$N_{prueba}=158(0.30)=47.4\approx48$$

$$N_{entrenamiento}=158-48=110$$

### 7. Regresión Lineal

Se ajustó un modelo `LinearRegression` utilizando las tres variables
predictoras.

La tabla de coeficientes obtenida en el notebook es:

                          coefficients
  --------------------- --------------
  Concentracion_Ozono      1212.210118
  Obs_Diarias                 0.006555
  Pct_Completo                0.039331

El intercepto obtenido fue:

``` text
-15.623800781887674
```

Por tanto, la ecuación del modelo lineal construido en el notebook es:

$$\widehat{AQI}=-15.6238+1212.2101(Concentracion\_Ozono)+0.006555(Obs\_Diarias)+0.039331(Pct\_Completo)$$

**Interpretación de los coeficientes:** manteniendo constantes las demás
variables, el modelo asigna el mayor coeficiente a la concentración de
ozono. El valor de 1212.2101 indica que, según la escala utilizada en el
dataset, cambios en la concentración de ozono tienen un efecto mucho
mayor sobre el AQI estimado que cambios unitarios en `Obs_Diarias` o
`Pct_Completo`. Esta interpretación debe hacerse considerando las
diferentes unidades y escalas de las variables.

### 8. Error estándar y estadística t

El notebook calcula el error estándar y la estadística t de los
coeficientes:

                          coefficients   Standard Error   t-statistic
  --------------------- -------------- ---------------- -------------
  Concentracion_Ozono      1212.210118        39.193945     30.928504
  Obs_Diarias                 0.006555         0.958513      0.006839
  Pct_Completo                0.039331         0.159752      0.246198

**Interpretación:** `Concentracion_Ozono` presenta una estadística t
elevada en valor absoluto, mientras que `Obs_Diarias` y `Pct_Completo`
presentan valores cercanos a cero. Esto es consistente con la matriz de
correlación y con la poca variabilidad de estas últimas variables. No
obstante, debido a la correlación perfecta entre `Obs_Diarias` y
`Pct_Completo`, la interpretación individual de sus coeficientes debe
hacerse con cautela.

La ecuación utilizada para la estadística t es:

$$t_i=\frac{\beta_i}{SE(\beta_i)}$$

### 9. Gráficos de dispersión de los predictores

#### Figura 5. Predictores frente al AQI

![Predictores vs AQI](imagenes/05_predictores_vs_aqi.png)

**Interpretación:**

-   **Concentracion_Ozono vs. AQI:** se observa una relación positiva
    clara. A medida que aumenta la concentración de ozono, también
    aumenta el AQI. La forma de la nube de puntos es coherente con la
    alta correlación de 0.9503.
-   **Obs_Diarias vs. AQI:** los puntos se concentran principalmente en
    `Obs_Diarias = 17`, con pocos valores diferentes. No se aprecia una
    tendencia lineal clara.
-   **Pct_Completo vs. AQI:** ocurre algo similar. La mayoría de
    observaciones están en 100 %, por lo que existe poca variabilidad
    para explicar cambios en el AQI.

### 10. Predicciones de la Regresión Lineal

El modelo generó **48 predicciones** para el conjunto de prueba.

#### Figura 6. AQI real vs. AQI predicho

![AQI real vs predicho](imagenes/06_aqi_real_vs_predicho.png)

**Interpretación:** la línea roja discontinua representa la predicción
perfecta, es decir, `AQI Predicho = AQI Real`. Los puntos se encuentran
relativamente próximos a esta línea, especialmente en el rango central.
Sin embargo, en valores altos de AQI se observan desviaciones más
visibles, lo que indica que la regresión lineal tiene mayor dificultad
para reproducir algunos valores extremos.

### 11. Análisis de residuos

#### Figura 7. Histograma de residuos

![Histograma de residuos](imagenes/07_histograma_residuos.png)

**Interpretación:** el histograma permite observar la distribución de
los errores definidos como:

$$e_i=y_i-\widehat{y}_i$$

La distribución se concentra alrededor de cero, pero no presenta una
forma perfectamente simétrica. Se observan desviaciones y colas, por lo
que la normalidad de los residuos no debe considerarse perfecta
únicamente a partir de esta figura.

#### Figura 8. Residuos vs. AQI predicho

![Residuos vs predicho](imagenes/08_residuos_vs_predicho.png)

**Interpretación:** el gráfico permite verificar si los residuos se
distribuyen alrededor de cero y si la dispersión es aproximadamente
constante. La presencia de puntos por encima y por debajo de la línea
cero indica errores de ambos signos. Las desviaciones más grandes en
determinados niveles de predicción muestran que la variabilidad del
error no es completamente uniforme.

### 12. Métricas de evaluación de la Regresión Lineal

La tabla de resultados del notebook es:

  Métrica                                      Valor
  ---------------------------------------- ---------
  MAE (Error Absoluto Medio)                  2.4384
  MSE (Error Cuadrático Medio)               10.5707
  RMSE (Raíz del Error Cuadrático Medio)      3.2513
  R² (Coeficiente de determinación)           0.9130

Las ecuaciones correspondientes son:

$$MAE=\frac{1}{n}\sum_{i=1}^{n}|y_i-\widehat{y}_i|$$

$$MSE=\frac{1}{n}\sum_{i=1}^{n}(y_i-\widehat{y}_i)^2$$

$$RMSE=\sqrt{MSE}$$

$$R^2=1-\frac{\sum(y_i-\widehat{y}_i)^2}{\sum(y_i-\bar{y})^2}$$

**Interpretación:** el MAE de 2.4384 indica el error absoluto medio de
las predicciones en unidades de AQI. El RMSE de 3.2513 penaliza más los
errores grandes. El R² de 0.9130 indica que el modelo explica
aproximadamente el 91.30 % de la variabilidad observada del AQI en el
conjunto de prueba.

**Dato derivado de la métrica:**

$$R^2\times100=0.9130\times100=91.30\%$$

Por tanto, el 91.30 % corresponde a la proporción de variabilidad del
AQI explicada por el modelo en el conjunto de prueba, según esta
métrica.

------------------------------------------------------------------------

## Resultados

### 1. Random Forest Regressor

Se entrenó un `RandomForestRegressor` con:

-   `n_estimators = 100`
-   `random_state = 123`

El modelo se entrenó utilizando las mismas particiones de entrenamiento
y prueba empleadas para la regresión lineal.

Los resultados obtenidos fueron:

  Métrica      Valor
  --------- --------
  MAE         0.3481
  MSE         0.7163
  RMSE        0.8464
  R²          0.9941

Las métricas se calculan mediante las mismas ecuaciones anteriores.

**Interpretación:** en el conjunto de prueba, el Random Forest presenta
un MAE y un RMSE menores que la Regresión Lineal, además de un R² mayor.
Esto significa que, dentro de esta evaluación concreta, sus predicciones
se encuentran más próximas a los valores observados y explican una mayor
proporción de la variabilidad del AQI.

### 2. Importancia de variables

#### Figura 9. Importancia de variables -- Random Forest

![Importancia de variables](imagenes/09_importancia_variables.png)

**Interpretación:** el gráfico muestra una importancia relativa
prácticamente concentrada en `Concentracion_Ozono`, mientras que
`Obs_Diarias` y `Pct_Completo` presentan importancia cercana a cero.
Este resultado es coherente con la matriz de correlación y con los
gráficos de dispersión: la concentración de ozono contiene la mayor
parte de la información útil para explicar el AQI dentro de este
conjunto de datos.

### 3. Comparación de modelos

La tabla comparativa generada directamente en el notebook es:

  Modelo                  MAE     RMSE       R²
  ------------------ -------- -------- --------
  Regresión Lineal     2.4384   3.2513   0.9130
  Random Forest        0.3481   0.8464   0.9941

#### Figura 10. Comparación de AQI real vs. predicho

![Comparación de modelos](imagenes/10_comparacion_modelos.png)

**Interpretación:** en el gráfico de la izquierda, correspondiente a la
Regresión Lineal, existe una dispersión mayor respecto de la línea de
predicción perfecta. En el gráfico de la derecha, correspondiente al
Random Forest, los puntos aparecen mucho más próximos a dicha línea.
Esta diferencia visual coincide con los valores de R² y RMSE obtenidos
para ambos modelos.

**Datos derivados de la tabla comparativa:**

Para calcular la reducción relativa del RMSE del Random Forest respecto
a la Regresión Lineal:

$$Reduccion\ RMSE=\frac{RMSE_{RL}-RMSE_{RF}}{RMSE_{RL}}\times100$$

$$=\frac{3.2513-0.8464}{3.2513}\times100\approx73.97\%$$

Para el MAE:

$$Reduccion\ MAE=\frac{MAE_{RL}-MAE_{RF}}{MAE_{RL}}\times100$$

$$=\frac{2.4384-0.3481}{2.4384}\times100\approx85.73\%$$

Estos porcentajes son cálculos derivados de la tabla del notebook y no
valores que hayan sido impresos directamente por el código.

### 4. Análisis OLS

Finalmente, se utilizó `statsmodels` para ajustar un modelo de Mínimos
Cuadrados Ordinarios utilizando `x_train` y `y_train`.

El resumen generado por el notebook fue:

``` text
                            OLS Regression Results                            
==============================================================================
Dep. Variable:                    AQI   R-squared:                       0.899
Model:                            OLS   Adj. R-squared:                  0.898
Method:                 Least Squares   F-statistic:                     478.4
Date:                Thu, 17 Sep 2026   Prob (F-statistic):           4.31e-54
Time:                        21:22:41   Log-Likelihood:                -301.89
No. Observations:                 110   AIC:                             609.8
Df Residuals:                     107   BIC:                             617.9
Df Model:                           2                                         
Covariance Type:            nonrobust                                         
=======================================================================================
                          coef    std err          t      P>|t|      [0.025      0.975]
---------------------------------------------------------------------------------------
const                 -14.0995     14.472     -0.974      0.332    -42.788      14.589
Concentracion_Ozono  1212.2101     39.196     30.927      0.000    1134.508    1289.912
Obs_Diarias            -4.5663      4.668     -0.978      0.330     -13.820       4.687
Pct_Completo            0.8015      0.937     0.856      0.394      -1.056       2.659
==============================================================================
Omnibus:                       64.719   Durbin-Watson:                   2.245
Prob(Omnibus):                  0.000   Jarque-Bera (JB):              253.059
Skew:                           2.092   Prob(JB):                     1.12e-55
Kurtosis:                       9.140   Cond. No.                     3.38e+15
==============================================================================

Notes:
[1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
[2] The smallest eigenvalue is 9.87e-26. This might indicate that there are
strong multicollinearity problems or that the design matrix is singular.
```

El notebook también genera la advertencia:

``` text
SingularMatrixWarning: The design matrix is rank-deficient.
The model parameters are not uniquely determined.
```

**Interpretación:** el OLS confirma nuevamente que `Concentracion_Ozono`
es la variable asociada al mayor efecto estadístico sobre el AQI. Sin
embargo, `Obs_Diarias` y `Pct_Completo` presentan problemas de
colinealidad porque su correlación es exactamente 1.0000. El número de
condición extremadamente elevado (`3.38e+15`) y la advertencia de matriz
singular son señales de este problema. Por ello, los coeficientes
individuales de estas dos variables deben interpretarse con precaución.

------------------------------------------------------------------------

## Discusión

Los resultados muestran una relación muy fuerte entre la concentración
de ozono y el AQI en los 158 registros analizados. La correlación de
Pearson entre ambas variables es 0.9503 y los gráficos de dispersión
muestran una tendencia ascendente clara. Esto explica por qué la
concentración de ozono adquiere el principal peso dentro de los modelos.

La Regresión Lineal obtuvo un R² de 0.9130 y un RMSE de 3.2513 en el
conjunto de prueba. El modelo representa adecuadamente la tendencia
general de los datos, aunque presenta desviaciones mayores en algunos
valores altos de AQI. El análisis gráfico de los residuos también
muestra que los errores no presentan una distribución perfectamente
ideal.

El Random Forest obtuvo un R² de 0.9941 y un RMSE de 0.8464 en el mismo
conjunto de prueba. En esta evaluación, sus predicciones se encuentran
más próximas a los valores reales, hecho que también se observa en el
gráfico comparativo.

Sin embargo, estos resultados deben analizarse considerando las
características del dataset. `Obs_Diarias` y `Pct_Completo` tienen una
correlación perfecta de 1.0000 y poca variabilidad. Esto genera
redundancia entre predictores y problemas de colinealidad en el análisis
OLS. Además, la importancia de variables del Random Forest se concentra
prácticamente por completo en `Concentracion_Ozono`, lo que indica que
las otras dos variables aportan poca información predictiva en esta
muestra.

Por ello, el análisis muestra que la concentración de ozono es el
principal predictor del AQI dentro del conjunto estudiado. La
comparación entre modelos también evidencia que una relación
predominantemente asociada al ozono puede ser capturada por una
regresión lineal, mientras que el Random Forest reproduce con menor
error los valores del conjunto de prueba utilizado.

Es importante señalar que los resultados corresponden específicamente a
la muestra de 158 observaciones, a la estación Hermiston - Municipal
Airport y al período abril--septiembre de 2023. Por tanto, no deben
generalizarse automáticamente a otras estaciones, períodos o condiciones
ambientales sin realizar una validación adicional.

------------------------------------------------------------------------

## Explicación de 7 funciones utilizadas en el código

### 1. `pd.read_csv()`

**Uso en el notebook:**

``` python
df_raw = pd.read_csv('ad_viz_plotval_data.csv')
```

**Función:** carga un archivo CSV y lo convierte en un `DataFrame` de
pandas.

**En este proyecto:** se utiliza para importar los registros de calidad
del aire desde `ad_viz_plotval_data.csv`.

### 2. `df.describe()`

**Uso en el notebook:**

``` python
df.describe().round(4)
```

**Función:** genera estadísticas descriptivas de las columnas numéricas,
como conteo, media, desviación estándar, mínimos, cuartiles y máximo.

**En este proyecto:** permitió conocer la distribución numérica de la
concentración de ozono, las observaciones diarias, el porcentaje de
completitud y el AQI.

### 3. `sns.pairplot()`

**Uso en el notebook:**

``` python
sns.pairplot(df)
```

**Función:** genera una matriz de gráficos que permite visualizar las
relaciones entre pares de variables y sus distribuciones individuales.

**En este proyecto:** se utilizó para realizar una exploración visual
inicial de las relaciones entre las tres variables predictoras y el AQI.

### 4. `train_test_split()`

**Uso en el notebook:**

``` python
x_train, x_test, y_train, y_test = train_test_split(
    x, y, test_size=0.3, random_state=123
)
```

**Función:** divide los datos en subconjuntos de entrenamiento y prueba.

**En este proyecto:** se utilizó el 70 % de los datos para entrenamiento
y el 30 % para prueba, manteniendo `random_state=123` para obtener una
división reproducible.

### 5. `LinearRegression().fit()`

**Uso en el notebook:**

``` python
lm = LinearRegression()
lm.fit(x_train, y_train)
```

**Función:** ajusta los parámetros de un modelo de regresión lineal a
partir de los datos de entrenamiento.

**En este proyecto:** estima el intercepto y los coeficientes que
relacionan las variables predictoras con el AQI.

### 6. `metrics.mean_absolute_error()`

**Uso en el notebook:**

``` python
mae = metrics.mean_absolute_error(y_test, predictions)
```

**Función:** calcula el promedio de los valores absolutos de las
diferencias entre los valores reales y los predichos.

**Ecuación:**

$$MAE=\frac{1}{n}\sum_{i=1}^{n}|y_i-\widehat{y}_i|$$

**En este proyecto:** permitió cuantificar el error promedio de las
predicciones de la Regresión Lineal y del Random Forest.

### 7. `metrics.r2_score()`

**Uso en el notebook:**

``` python
r2 = metrics.r2_score(y_test, predictions)
```

**Función:** calcula el coeficiente de determinación R², que indica qué
proporción de la variabilidad de la variable objetivo es explicada por
el modelo respecto a una referencia basada en la media.

**Ecuación:**

$$R^2=1-\frac{\sum(y_i-\widehat{y}_i)^2}{\sum(y_i-\bar{y})^2}$$

**En este proyecto:** permitió comparar la capacidad explicativa de la
Regresión Lineal y del Random Forest.

------------------------------------------------------------------------

## Referencias

\[1\] U.S. Environmental Protection Agency, *Air Data: Air Quality Data
Collected at Outdoor Monitors Across the United States*, U.S. EPA, 2023.

\[2\] W. McKinney, *Python for Data Analysis: Data Wrangling with
pandas, NumPy, and Jupyter*, 3rd ed. Sebastopol, CA, USA: O'Reilly
Media, 2022.

\[3\] C. R. Harris et al., "Array programming with NumPy," *Nature*,
vol. 585, pp. 357--362, 2020.

\[4\] J. D. Hunter, "Matplotlib: A 2D graphics environment," *Computing
in Science & Engineering*, vol. 9, no. 3, pp. 90--95, 2007.

\[5\] M. Waskom, "seaborn: statistical data visualization," *Journal of
Open Source Software*, vol. 6, no. 60, p. 3021, 2021.

\[6\] F. Pedregosa et al., "Scikit-learn: Machine Learning in Python,"
*Journal of Machine Learning Research*, vol. 12, pp. 2825--2830, 2011.

\[7\] S. Seabold and J. Perktold, "Statsmodels: Econometric and
Statistical Modeling with Python," in *Proc. 9th Python in Science Conf.
(SciPy 2010)*, 2010.
