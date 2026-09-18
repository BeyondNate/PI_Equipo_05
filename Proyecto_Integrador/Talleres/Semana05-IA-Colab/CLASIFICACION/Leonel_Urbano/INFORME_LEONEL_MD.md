# INFORME DE REGRESIÓN LINEL Y RANDOM FOREST PARA LA PREDICCION DEL AQI

## INTRODUCCIÓN

La calidad del aire constituye un aspecto importante para evaluar las condiciones ambientales y sus posibles efectos sobre la población. Entre los indicadores utilizados para representar estas condiciones se encuentra el Índice de Calidad del Aire (AQI), cuyo comportamiento puede relacionarse con diferentes variables asociadas a la medición de contaminantes atmosféricos. En particular, la concentración de ozono puede presentar una relación importante con las variaciones observadas en el AQI.

En este estudio se analiza un conjunto de 158 observaciones correspondientes a mediciones realizadas entre abril y septiembre de 2023. El análisis considera como variable dependiente el AQI, mientras que como variables predictoras se emplean la Concentración de Ozono, el número de Observaciones Diarias y el Porcentaje de Datos Completos. Inicialmente, se realiza un análisis exploratorio mediante estadística descriptiva, gráficos de distribución, diagramas de dispersión y una matriz de correlaciones, con el propósito de identificar patrones y relaciones entre las variables.

Posteriormente, se desarrolla un modelo de Regresión Lineal para estimar el AQI a partir de las variables predictoras. Su desempeño se evalúa mediante métricas como el Error Absoluto Medio (MAE), Error Cuadrático Medio (MSE), Raíz del Error Cuadrático Medio (RMSE) y coeficiente de determinación (R²). Asimismo, se analizan los residuos para observar el comportamiento de los errores de predicción.

Como complemento, se emplea un modelo de Random Forest Regressor, con el propósito de comparar su capacidad predictiva con la obtenida mediante la regresión lineal. También se analiza la importancia relativa de las variables predictoras y se utiliza un modelo de Mínimos Cuadrados Ordinarios (OLS) para profundizar en la interpretación estadística de los coeficientes.

Finalmente, los resultados permiten identificar las variables que presentan mayor relación con el AQI, evaluar el comportamiento de los modelos de predicción y analizar las diferencias entre un enfoque lineal y uno basado en árboles de decisión. De esta manera, el estudio busca determinar qué tan adecuadamente pueden utilizarse las variables ambientales disponibles para explicar y predecir las variaciones del Índice de Calidad del Aire.


## METODOLOGíA

### 1. Carga y selección de los datos

El archivo CSV se carga mediante `pd.read_csv()` y posteriormente se
seleccionan las cuatro columnas utilizadas en el estudio. Las variables
se renombran para facilitar su manejo:

-   `Concentracion_Ozono`: concentración máxima diaria de ozono.
-   `Obs_Diarias`: número de observaciones diarias.
-   `Pct_Completo`: porcentaje de datos completos.
-   `AQI`: Índice de Calidad del Aire, utilizado como variable objetivo.

La estructura obtenida es:

<img width="482" height="243" alt="image" src="https://github.com/user-attachments/assets/48c3c04e-6dba-4f55-884f-7100ada7946a" />

Por lo tanto, se trabajó con **158 observaciones y 4 variables**, sin
valores nulos en las columnas seleccionadas.

### 2. Estadística descriptiva

Se utilizó `df.describe().round(4)` para obtener los principales
estadísticos descriptivos.

<img width="632" height="343" alt="image" src="https://github.com/user-attachments/assets/c2ce607e-218e-41bd-91f5-a6b59d9e5cd9" />

**Interpretación:** La concentración media de ozono es 0.0464, mientras
que el AQI presenta una media de 44.5949 y una desviación estándar de
11.6652. `Obs_Diarias` y `Pct_Completo` presentan poca variabilidad,
debido a que la mayoría de los registros tienen 17 observaciones y 100 %
de completitud.

### 3. Exploración gráfica

Se utilizaron gráficos exploratorios para observar la distribución del
AQI y las relaciones entre las variables.

#### Figura 1. Pairplot de las variables

<img width="986" height="986" alt="image" src="https://github.com/user-attachments/assets/affbb864-8197-45b4-926f-4f00ab40936e" />

**Interpretación:** El pairplot muestra las relaciones bivariadas entre
las variables. Se observa una relación positiva marcada entre
`Concentracion_Ozono` y `AQI`, mientras que `Obs_Diarias` y
`Pct_Completo` presentan variaciones muy limitadas y una relación
prácticamente nula con el AQI. También se observa que `Obs_Diarias` y
`Pct_Completo` se mueven de forma prácticamente idéntica, situación que
posteriormente se confirma mediante la matriz de correlación.

#### Figura 2. Distribución del AQI diario

<img width="694" height="399" alt="image" src="https://github.com/user-attachments/assets/0f318a81-2e87-4ddf-8d69-ac365462e458" />

**Interpretación:** El histograma concentra la mayor cantidad de
observaciones aproximadamente entre 30 y 50 de AQI. También existe una
cola hacia valores superiores, llegando hasta aproximadamente 87. Esto
indica que la distribución no es perfectamente simétrica y presenta
algunos registros con AQI elevado.

#### Figura 3. Función de densidad del AQI

<img width="584" height="461" alt="image" src="https://github.com/user-attachments/assets/1fe03832-8caa-4813-8755-d2f9f0880045" />


**Interpretación:** La curva de densidad presenta su mayor concentración
alrededor de los valores medios del AQI, aproximadamente en la zona de
40--50. La cola hacia valores mayores muestra que existen observaciones
con AQI considerablemente superior al grupo principal.

### 4. Matriz de correlación

Se calculó la correlación de Pearson entre las variables numéricas.

<img width="681" height="187" alt="image" src="https://github.com/user-attachments/assets/9eaaa826-b6be-467d-a090-da026048273d" />

#### Figura 4. Mapa de calor de correlaciones

<img width="763" height="530" alt="image" src="https://github.com/user-attachments/assets/b1a132ab-9180-4d08-9b8a-7f5248c9ccbc" />

**Interpretación:** La correlación entre `Concentracion_Ozono` y `AQI`
es **0.9503**, lo que representa una relación lineal positiva muy fuerte
dentro de esta muestra. En contraste, `Obs_Diarias` y `Pct_Completo`
presentan una correlación de **-0.0080** con el AQI. Además, ambas
variables tienen una correlación de **1.0000 entre sí**, lo que
evidencia una fuerte redundancia entre estos dos predictores.

### 5. Definición de variables predictoras y variable objetivo - Regresión lineal

Se definió:

-   **X:** `Concentracion_Ozono`, `Obs_Diarias` y `Pct_Completo`.
-   **y:** `AQI`.

Las primeras observaciones mostradas son:

<img width="456" height="212" alt="image" src="https://github.com/user-attachments/assets/8e4cc336-48a2-42ff-8367-fc21765537e6" />

Y los primeros valores de `AQI` son:

<img width="72" height="221" alt="image" src="https://github.com/user-attachments/assets/840bff30-1caf-49ab-881e-f24a15355f61" />

### 6. División entrenamiento-prueba

Los datos se dividieron mediante `train_test_split()` utilizando
`test_size=0.3` y `random_state=123`. De acuerdo con el tamaño total de
158 observaciones, el notebook trabaja con **110 observaciones de
entrenamiento y 48 de prueba**, como se refleja también en el resultado
de OLS y en el tamaño de `predictions`.


### 7. Regresión Lineal

Se ajustó un modelo `LinearRegression` utilizando las tres variables
predictoras.

La tabla de coeficientes obtenida en el notebook es:

<img width="297" height="162" alt="image" src="https://github.com/user-attachments/assets/8b939f7d-89eb-4550-b943-be8d6131c5e1" />

El término de intersección del modelo lineal: -15.623800781887674

Por tanto, la ecuación del modelo lineal construido en el GoogleColab es: Coeficientes del modelo lineal: [1.21221012e+03 6.55510761e-03 3.93306457e-02]

**Interpretación de los coeficientes:** Manteniendo constantes las demás
variables, el modelo asigna el mayor coeficiente a la concentración de
ozono. El valor de 1212.2101 indica que, según la escala utilizada en el
dataset, cambios en la concentración de ozono tienen un efecto mucho
mayor sobre el AQI estimado que cambios unitarios en `Obs_Diarias` o
`Pct_Completo`. Esta interpretación debe hacerse considerando las
diferentes unidades y escalas de las variables.

### 8. Error estándar y estadística t

Se calcula el error estándar y la estadística t de los
coeficientes:

<img width="566" height="143" alt="image" src="https://github.com/user-attachments/assets/254d7df5-da3f-44c8-9b69-13abeb2d4d4e" />

**Interpretación:** `Concentracion_Ozono` presenta una estadística t
elevada en valor absoluto, mientras que `Obs_Diarias` y `Pct_Completo`
presentan valores cercanos a cero. Esto es consistente con la matriz de
correlación y con la poca variabilidad de estas últimas variables. No
obstante, debido a la correlación perfecta entre `Obs_Diarias` y
`Pct_Completo`, la interpretación individual de sus coeficientes debe
hacerse con cautela.


### 9. Gráficos de dispersión de los predictores

#### Figura 5. Predictores frente al AQI

<img width="1789" height="490" alt="image" src="https://github.com/user-attachments/assets/ba6a28b5-412b-4fc7-a07e-60005967135a" />

**Interpretación:**

-   **Concentracion_Ozono vs. AQI:** Se observa una relación positiva
    clara. A medida que aumenta la concentración de ozono, también
    aumenta el AQI. La forma de la nube de puntos es coherente con la
    alta correlación de 0.9503.
-   **Obs_Diarias vs. AQI:** Los puntos se concentran principalmente en
    `Obs_Diarias = 17`, con pocos valores diferentes. No se aprecia una
    tendencia lineal clara.
-   **Pct_Completo vs. AQI:** Ocurre algo similar. La mayoría de
    observaciones están en 100 %, por lo que existe poca variabilidad
    para explicar cambios en el AQI.

### 10. Predicciones de la Regresión Lineal

El modelo generó **48 predicciones** para el conjunto de prueba.

#### Figura 6. AQI real vs. AQI predicho

<img width="851" height="647" alt="image" src="https://github.com/user-attachments/assets/7447535c-47cb-4892-9faf-e98906911f0d" />

**Interpretación:** La línea roja discontinua representa la predicción
perfecta, es decir, `AQI Predicho = AQI Real`. Los puntos se encuentran
relativamente próximos a esta línea, especialmente en el rango central.
Sin embargo, en valores altos de AQI se observan desviaciones más
visibles, lo que indica que la regresión lineal tiene mayor dificultad
para reproducir algunos valores extremos.

### 11. Análisis de residuos

#### Figura 7. Histograma de residuos

<img width="852" height="687" alt="image" src="https://github.com/user-attachments/assets/367988eb-daf5-4b21-b052-4565af1a12f7" />

**Interpretación:** La distribución se concentra alrededor de cero, pero no presenta una
forma perfectamente simétrica. Se observan desviaciones y colas, por lo
que la normalidad de los residuos no debe considerarse perfecta
únicamente a partir de esta figura.

#### Figura 8. Residuos vs. AQI predicho

<img width="854" height="687" alt="image" src="https://github.com/user-attachments/assets/2daa19f0-fc1a-4fbc-a836-51f6cb81be0b" />

**Interpretación:** El gráfico permite verificar si los residuos se
distribuyen alrededor de cero y si la dispersión es aproximadamente
constante. La presencia de puntos por encima y por debajo de la línea
cero indica errores de ambos signos. Las desviaciones más grandes en
determinados niveles de predicción muestran que la variabilidad del
error no es completamente uniforme.

### 12. Métricas de evaluación de la Regresión Lineal

La tabla de resultados es:

<img width="431" height="92" alt="image" src="https://github.com/user-attachments/assets/899351ca-484b-4205-a81d-ee9be69e439c" />

**Interpretación:** El MAE de 2.4384 indica el error absoluto medio de
las predicciones en unidades de AQI. El RMSE de 3.2513 penaliza más los
errores grandes. El R² de 0.9130 indica que el modelo explica
aproximadamente el 91.30 % de la variabilidad observada del AQI en el
conjunto de prueba.

## RESULTADOS

### 1. Random Forest Regressor

Se entrenó un `RandomForestRegressor` con:

-   `n_estimators = 100`
-   `random_state = 123`

El modelo se entrenó utilizando las mismas particiones de entrenamiento
y prueba empleadas para la regresión lineal.

Los resultados obtenidos fueron:

<img width="200" height="111" alt="image" src="https://github.com/user-attachments/assets/79c17a3c-8e3e-4dc8-a1d6-4eb7ce82b1f2" />

**Interpretación:** En el conjunto de prueba, el Random forest presenta
un MAE y un RMSE menores que la Regresión lineal, además de un R² mayor.
Esto significa que, dentro de esta evaluación concreta, sus predicciones
se encuentran más próximas a los valores observados y explican una mayor
proporción de la variabilidad del AQI.

### 2. Importancia de variables

#### Figura 9. Importancia de variables -- Random Forest

<img width="790" height="390" alt="image" src="https://github.com/user-attachments/assets/6a123729-4c76-47f0-a236-8436c059b0d2" />

**Interpretación:** El gráfico muestra una importancia relativa
prácticamente concentrada en `Concentracion_Ozono`, mientras que
`Obs_Diarias` y `Pct_Completo` presentan importancia cercana a cero.
Este resultado es coherente con la matriz de correlación y con los
gráficos de dispersión: la concentración de ozono contiene la mayor
parte de la información útil para explicar el AQI dentro de este
conjunto de datos.

### 3. Comparación de modelos

La tabla comparativa es:

<img width="347" height="97" alt="image" src="https://github.com/user-attachments/assets/51c01a09-cabf-452a-be13-7d6dfaeead39" />

#### Figura 10. Comparación de AQI real vs. predicho

<img width="1589" height="617" alt="image" src="https://github.com/user-attachments/assets/8febbcd2-1a27-4112-81ff-5860d54d11ed" />

**Interpretación:** En el gráfico de la izquierda, correspondiente a la
Regresión Lineal, existe una dispersión mayor respecto de la línea de
predicción perfecta. En el gráfico de la derecha, correspondiente al
Random Forest, los puntos aparecen mucho más próximos a dicha línea.
Esta diferencia visual coincide con los valores de R² y RMSE obtenidos
para ambos modelos.

### 4. Análisis OLS

Finalmente, se utilizó `statsmodels` para ajustar un modelo de Mínimos
Cuadrados Ordinarios utilizando `x_train` y `y_train`.

El resumen generado por el notebook fue:

<img width="807" height="552" alt="image" src="https://github.com/user-attachments/assets/910eb05f-e844-48ce-a459-3ecbd812ad3e" />

**Interpretación:** el OLS confirma nuevamente que `Concentracion_Ozono`
es la variable asociada al mayor efecto estadístico sobre el AQI. Sin
embargo, `Obs_Diarias` y `Pct_Completo` presentan problemas de
colinealidad porque su correlación es exactamente 1.0000. El número de
condición extremadamente elevado (`3.38e+15`) y la advertencia de matriz
singular son señales de este problema. Por ello, los coeficientes
individuales de estas dos variables deben interpretarse con precaución.

## DISCUSÍON

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

**En este proyecto:** permitió comparar la capacidad explicativa de la
Regresión Lineal y del Random Forest.


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
