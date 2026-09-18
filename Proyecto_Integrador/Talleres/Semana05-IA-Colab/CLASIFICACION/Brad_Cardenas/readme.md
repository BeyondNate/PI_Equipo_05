# Informe: Análisis mediante Regresión Lineal — Concentración de CO (Salt Lake City, 2022)

## 1. Metodología

### 1.1. Exploración del conjunto de datos

Se utilizó un conjunto de datos de calidad del aire correspondiente a la estación **Copper View** (Salt Lake City, Utah), que contiene **365 registros y 21 variables**, uno por cada día del año 2022. Entre las variables disponibles se encuentran la **concentración máxima diaria de CO en 8 horas** (variable objetivo), el **valor diario del AQI**, la **cantidad de observaciones diarias**, el **porcentaje de datos completos** y distintos campos identificativos de la estación (ubicación, códigos de método y de parámetro, coordenadas), que son constantes para todo el dataset al provenir de un único sitio de monitoreo.

Para conocer la estructura de los datos se utilizaron funciones básicas de `pandas`, principalmente `head()`, `info()` y `describe()`.

```python
df1 = pd.read_csv(file)
df1.head()
df1.info(verbose=True)
df1.describe().round(1)
```

La función `head()` permitió observar los primeros registros (por ejemplo, el 01/01/2022 se registró una concentración de 0.4 ppm de CO con un AQI de 5). La función `info()` confirmó que las 365 filas no tienen valores nulos y que las variables numéricas relevantes son de tipo `float64` e `int64`. Finalmente, `describe()` mostró que la concentración de CO oscila entre **0.0 y 1.0 ppm**, con una media de **0.284 ppm**, mientras que el AQI diario varía entre **0 y 11**, con una media de **3.15**.

**Imagen 1 – Exploración inicial del conjunto de datos**

![Exploración inicial del dataset](Capturas/exploracion_dataset.png)

*Figura 1. Primeros registros del conjunto de datos.*

---

### 1.2. Análisis exploratorio y correlación

Se realizó un análisis exploratorio para observar visualmente las relaciones entre las variables. Para ello se utilizó `pairplot()` de la biblioteca `seaborn`, aplicado sobre las cuatro variables numéricas que sí varían en el conjunto de datos (`Daily Max 8-hour CO Concentration`, `Daily AQI Value`, `Daily Obs Count` y `Percent Complete`; el resto de columnas numéricas —coordenadas, códigos de sitio y de parámetro— son constantes porque los datos provienen de una única estación).

```python
sns.pairplot(df1)
```

**Imagen 2 – Relaciones entre variables**

![Pairplot](Capturas/pairplot.png)

*Figura 2. Relaciones entre las variables del conjunto de datos.*

**Interpretación:** el panel más relevante es el que relaciona `Daily Max 8-hour CO Concentration` con `Daily AQI Value`: los puntos se alinean casi perfectamente sobre una recta creciente, lo que anticipa una correlación lineal casi perfecta entre ambas variables (el AQI de CO se calcula directamente a partir de la concentración de CO, por lo que esta relación es, en la práctica, una transformación matemática y no una asociación empírica). En cambio, `Daily Obs Count` y `Percent Complete` se concentran mayoritariamente en un único valor (24 observaciones y 100 % de datos completos), con un grupo reducido de días con menos observaciones (14–20) y menor porcentaje de completitud (58 %–92 %); estos puntos corresponden a días con fallas o interrupciones en el equipo de medición y no muestran una relación clara con la concentración de CO.

### Distribución de la variable objetivo

```python
df1['Daily Max 8-hour CO Concentration'].plot.hist(bins=25, figsize=(8,4))
df1['Daily Max 8-hour CO Concentration'].plot.density()
```

**Imagen 3 – Histograma de la variable objetivo**

![Histograma](Capturas/histograma_objetivo.png)

*Figura 3. Distribución de frecuencias de la concentración máxima diaria de CO.*

**Interpretación:** la distribución está claramente **sesgada a la derecha (asimetría positiva)**. La mayoría de los días del año presentan concentraciones bajas de CO, concentradas entre 0.1 y 0.3 ppm (más de 180 de los 365 días), mientras que un número reducido de días alcanza valores altos, de hasta 1.0 ppm. Esto es coherente con el comportamiento típico de contaminantes atmosféricos: la concentración se mantiene baja en condiciones normales y solo se eleva en episodios puntuales (por ejemplo, días fríos con inversión térmica, típicos del invierno en el valle de Salt Lake City, donde el CO tiende a acumularse cerca del suelo).

---

### 1.3. Matriz de correlación

Se realizó un análisis de correlación con el objetivo de identificar relaciones lineales entre las variables y determinar cuáles podrían ser utilizadas para el modelo de regresión.

```python
numeric_df = df1.select_dtypes(include=[np.number])
numeric_df.corr()
sns.heatmap(numeric_df.corr(), annot=True, linewidths=2)
```

**Imagen 4 – Matriz de correlación**

![Matriz de correlación](Capturas/matriz_correlacion.png)

*Figura 4. Matriz de correlación de las variables analizadas.*

**Interpretación:** la matriz confirma lo observado en el pairplot: `Daily Max 8-hour CO Concentration` y `Daily AQI Value` presentan una correlación de **1.00**, es decir, prácticamente perfecta (de nuevo, por construcción, ya que el AQI de CO es una función directa de su concentración). Por su parte, `Daily Obs Count` y `Percent Complete` están correlacionadas entre sí de forma perfecta (**1.00**, ya que ambas miden completitud de datos desde ángulos distintos) y mantienen una correlación **débil y negativa (-0.13)** con la concentración de CO, lo que sugiere que, en los días con menos observaciones registradas, la concentración medida tiende a ser ligeramente más baja (posiblemente por sesgo de muestreo incompleto), aunque el efecto es marginal.

---

### 1.4. Preparación de los datos

Para construir el modelo se separaron las variables independientes, representadas por **X** (`Daily AQI Value`, `Daily Obs Count`, `Percent Complete`), de la variable objetivo, representada por **y** (`Daily Max 8-hour CO Concentration`). Se excluyeron del conjunto de predictores las columnas de texto, identificadores y códigos que no aportan a la regresión (`Date`, `Source`, `Site ID`, `POC`, `Units`, `Local Site Name`, códigos AQS/CBSA/FIPS, `State`, `County`, coordenadas, etc.).

```python
variable_objetivo = 'Daily Max 8-hour CO Concentration'
columnas_a_excluir = ['Date', 'Source', 'Site ID', 'POC', 'Units', ...]
X = df1.drop(columns=columnas_a_excluir)
y = df1[variable_objetivo]
```

Posteriormente, los datos fueron divididos en:

* **70 % para entrenamiento** (255 registros), utilizado para ajustar el modelo.
* **30 % para prueba** (110 registros), utilizado para evaluar las predicciones.

La división se realizó mediante `train_test_split()` utilizando `random_state=123` para mantener la reproducibilidad de los resultados.

```python
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=123)
```

---

### 1.5. Regresión lineal

Se utilizó `LinearRegression()` para construir el modelo de regresión lineal múltiple.

```python
lm = LinearRegression()
lm.fit(X_train, y_train)
```

El modelo permite estimar una relación lineal entre las variables de entrada y la variable objetivo. Después del entrenamiento se obtuvieron la intersección y los coeficientes:

```python
print(lm.intercept_)
print(lm.coef_)
```

**Resultados obtenidos:**

| Variable | Coeficiente | t-estadístico |
|---|---|---|
| Daily AQI Value | 0.0852 | 189.82 |
| Daily Obs Count | 0.2573 | 114.71 |
| Percent Complete | -0.0611 | -114.23 |
| Intercepto | -0.0510 | — |

**Interpretación de los coeficientes:** `Daily AQI Value` tiene el t-estadístico más alto en valor absoluto, lo que confirma que es, por lejos, la variable más determinante del modelo (relación directamente proporcional: a mayor AQI, mayor concentración de CO, consistente con la correlación de 1.00 observada antes). `Daily Obs Count` presenta un coeficiente positivo (0.257): a más observaciones registradas en el día, el modelo predice una concentración levemente mayor. `Percent Complete` tiene coeficiente negativo (-0.061): días con mayor porcentaje de completitud tienden a asociarse, dentro del modelo, con concentraciones ligeramente menores, en línea con la correlación negativa detectada previamente.

### Significancia estadística de los coeficientes (t-statistic)

```python
n = X_train.shape[0]; k = X_train.shape[1]; dfN = n - k
train_error = np.square(train_pred - y_train)
...
```

Ordenando las variables por su t-estadístico (en valor absoluto), la jerarquía de importancia resultante es:

```
Daily AQI Value
Daily Obs Count
Percent Complete
```

### Relación de las variables más importantes con la variable objetivo

```python
fig = plt.figure(figsize=(18, 10))
gs = gridspec.GridSpec(2, 2)
ax0.scatter(df1[l[0]], df1[variable_objetivo]); ...
```

**Imagen 5 – Variables más importantes vs. variable objetivo**

![Variables vs target](Capturas/variables_vs_target.png)

*Figura 5. Relación de las tres variables predictoras con la concentración de CO.*

**Interpretación:** el panel de `Daily AQI Value` muestra la relación lineal casi perfecta ya descrita. Los paneles de `Daily Obs Count` y `Percent Complete` muestran una nube de puntos concentrada en los valores máximos (24 observaciones / 100 % completitud) que cubre prácticamente todo el rango de concentración de CO, y un pequeño grupo de puntos con valores más bajos de estas dos variables asociados a concentraciones diversas —evidencia visual de que su aporte predictivo real es marginal frente al de `Daily AQI Value`.

### R cuadrado del ajuste del modelo (entrenamiento)

```python
metrics.r2_score(y_train, train_pred)
```

**Resultado:** R² de entrenamiento = **0.993**, es decir, el modelo explica el 99.3 % de la variabilidad de la concentración de CO en el conjunto de entrenamiento.

---

### 1.6. Evaluación del modelo con datos de prueba

```python
predictions = lm.predict(X_test)
```

### Valores reales vs. predichos

```python
plt.scatter(x=y_test, y=predictions, alpha=0.7)
plt.plot([y_test.min(), y_test.max()], [y_test.min(), y_test.max()], ...)
```

**Imagen 6 – Valores reales vs. valores predichos**

![Valores reales y predichos](Capturas/real_vs_predicho.png)

*Figura 6. Comparación entre los valores reales y los valores predichos por el modelo.*

**Interpretación:** los puntos se ajustan de forma muy cercana a la línea diagonal de predicción perfecta (línea roja), en todo el rango de valores (desde 0.0 hasta 0.9 ppm). Existen leves desviaciones puntuales, por ejemplo, cerca de 0.3–0.5 ppm, donde el modelo subestima o sobreestima levemente algunos valores individuales, pero en general el ajuste es excelente y consistente con el R² obtenido.

### Análisis de residuos

```python
residuos = y_test - predictions
sns.histplot(residuos, kde=True, color='blue', bins=30)
```

```python
plt.scatter(x=predictions, y=residuos, alpha=0.7)
plt.axhline(y=0, color='red', linestyle='--', linewidth=2)
```

**Imagen 7 – Análisis de residuos**

![Análisis de residuos](Capturas/residuos.png)

*Figura 7. Distribución de los residuos e histograma de residuos vs. predichos.*

**Interpretación:** el histograma de residuos muestra una forma aproximadamente centrada en cero (media de los residuos ≈ -0.003), con una leve asimetría negativa, y con la mayor parte de los errores concentrados en un rango muy estrecho, entre -0.05 y +0.06 ppm. Esto es coherente con los indicadores de error obtenidos (ver sección 1.8) y respalda el supuesto de normalidad de los residuos, aunque con algunas colas discretas. En el gráfico de residuos vs. predichos no se observa un patrón sistemático (curvatura o embudo) alrededor de la línea de referencia en cero, lo que sugiere una **homoscedasticidad razonable**: la varianza del error se mantiene relativamente estable a lo largo del rango de valores predichos, sin evidencia fuerte de que el modelo funcione peor para concentraciones altas que para concentraciones bajas.

---

### 1.7. Análisis complementario con datos artificiales

Como complemento del análisis, se generó un conjunto de datos artificiales utilizando `make_regression()`, con el fin de probar otros algoritmos de forma controlada (con relaciones conocidas de antemano).

Se utilizaron **100 muestras, 6 características y 3 características informativas**, además de un nivel de ruido de 20 y una semilla aleatoria (`random_state=20`) para mantener la reproducibilidad.

```python
x, y, coef = make_regression(
    n_samples=100, n_features=6, n_informative=3,
    random_state=20, shuffle=False, noise=20, coef=True
)
```

---

### 1.8. Árbol de decisión

Sobre los datos artificiales se entrenó un `DecisionTreeRegressor` con una profundidad máxima de 5.

```python
tree_model = tree.DecisionTreeRegressor(max_depth=5, random_state=10)
```

El modelo realizó predicciones sobre el conjunto de prueba, obteniéndose un **MSE de 7,931.6** (un valor elevado en términos absolutos, pero esperable dado que los datos artificiales se generaron con `noise=20` y valores de la variable objetivo en una escala mucho más amplia que la del CO real).

**Imagen 8 – Real vs. predicho (árbol de decisión)**

![Árbol real vs predicho](Capturas/arbol_real_vs_pred.png)

*Figura 8. Comparación entre valores reales y predichos del árbol de decisión sobre datos artificiales.*

Además, se obtuvo la importancia relativa de cada característica utilizada por el árbol.

**Imagen 9 – Importancia de las características**

![Importancia de características](Capturas/importancia_caracteristicas.png)

*Figura 9. Importancia relativa de las características en el árbol de decisión.*

**Interpretación:** el árbol identifica correctamente que las variables **x1, x2 y x3** son las más relevantes (con importancias de 0.269, 0.537 y 0.111 respectivamente, sumando ≈ 92 % de la importancia total), mientras que **x4, x5 y x6** —que no fueron marcadas como informativas al generar los datos— reciben una importancia mucho menor (todas por debajo de 0.04). Esto valida que el modelo de árbol es capaz de distinguir automáticamente las variables realmente predictivas de las que solo aportan ruido, tal como se diseñó el experimento (`n_informative=3` de 6 características totales).

---

### 1.9. Mínimos cuadrados

Finalmente, se utilizó `statsmodels` para ajustar un modelo mediante el método de mínimos cuadrados ordinarios (OLS) sobre los mismos datos artificiales.

```python
xs = sm.add_constant(x)
stat_model = sm.OLS(y, xs)
stat_result = stat_model.fit()
print(stat_result.summary())
```

**Interpretación:** el resumen del modelo OLS arrojó un **R² de 0.976** (R² ajustado 0.974), con un estadístico F de 628.6 y una probabilidad asociada prácticamente nula (p ≈ 6×10⁻⁷³), lo que indica que el modelo en conjunto es altamente significativo. A nivel individual, los coeficientes de **x1, x2 y x3** son estadísticamente significativos (p < 0.001 en los tres casos), con valores muy cercanos a los coeficientes reales usados para generar los datos, mientras que **x4, x5 y x6** no resultan significativos (p > 0.05 en los tres casos), confirmando nuevamente —desde un enfoque estadístico formal— el mismo hallazgo que entregó la importancia de características del árbol de decisión: solo tres de las seis variables generadas influyen realmente sobre la variable objetivo.

---

## 2. Resultados

La exploración inicial permitió identificar un conjunto de **365 registros y 21 columnas**, correspondientes a mediciones diarias de monóxido de carbono (CO) en la estación Copper View de Salt Lake City durante el año 2022. Las estadísticas descriptivas mostraron que la concentración máxima diaria de CO en 8 horas presenta valores entre **0.0 y 1.0 ppm** (media 0.284 ppm), mientras que el valor diario del AQI varía entre **0 y 11** (media 3.15).

El análisis exploratorio y la matriz de correlación mostraron una relación lineal casi perfecta (r = 1.00) entre la concentración de CO y el AQI diario, y una relación débil y negativa (r = -0.13) entre la concentración de CO y las variables de completitud de datos (`Daily Obs Count`, `Percent Complete`).

El modelo de regresión lineal fue entrenado utilizando el 70 % de los datos (255 registros) y evaluado con el 30 % restante (110 registros), obteniendo un **R² de 0.993 en entrenamiento** y de **0.990 en prueba**, con un **MAE de 0.0178 ppm**, un **MSE de 0.00054** y un **RMSE de 0.0232 ppm**. Estos resultados indican un ajuste excelente del modelo, con un margen de error muy pequeño respecto a la escala de la variable objetivo (0–1 ppm).

La comparación entre valores reales y predichos confirmó visualmente el buen desempeño del modelo, y el análisis de residuos no mostró patrones sistemáticos relevantes, sugiriendo un comportamiento razonablemente homoscedástico y consistente con la normalidad de los errores.

Como análisis complementario, sobre datos artificiales generados con `make_regression()` se entrenó un árbol de decisión (MSE de 7,931.6) y un modelo OLS (R² = 0.976). Ambos coincidieron en identificar correctamente que solo 3 de las 6 características generadas (x1, x2, x3) son realmente informativas, validando de forma cruzada la capacidad de ambos métodos para distinguir señal de ruido.

---

## 3. Discusión

El análisis permitió aplicar de forma completa las etapas de un proceso de regresión: exploración de datos, análisis de relaciones, preparación de variables, entrenamiento, predicción y evaluación, aplicado a un problema real de calidad del aire.

Un hallazgo importante es que la variable predictora `Daily AQI Value` está matemáticamente derivada de la propia variable objetivo (el AQI de CO se calcula a partir de su concentración), lo que explica la correlación casi perfecta y el altísimo R² obtenido. En un contexto de predicción operativa (por ejemplo, anticipar la concentración de CO sin conocer aún el AQI del día), sería recomendable excluir `Daily AQI Value` del conjunto de predictores y evaluar el modelo únicamente con variables verdaderamente independientes (`Daily Obs Count`, `Percent Complete`) u otras fuentes de datos (meteorología, tráfico, estacionalidad), ya que estas por sí solas muestran una capacidad predictiva mucho más limitada (correlación de apenas -0.13 con la variable objetivo).

El uso de gráficos facilitó la interpretación de los datos: el histograma reveló la naturaleza asimétrica de la concentración de CO (más días con niveles bajos, pocos días con picos altos), y el análisis de residuos permitió complementar la evaluación del modelo, verificando que los errores no siguen un patrón sistemático que sugiera un mal ajuste o problemas de especificación.

El uso de datos artificiales, mediante `make_regression()`, permitió realizar una segunda prueba controlada donde se conocía de antemano cuáles variables debían ser relevantes. Tanto el árbol de decisión como el modelo de mínimos cuadrados (OLS) identificaron correctamente estas variables, lo que refuerza la validez metodológica del enfoque utilizado a lo largo de todo el proyecto.

---

## 4. Conclusiones

* Se realizó una exploración inicial del conjunto de datos de CO de Salt Lake City (2022) utilizando `head()`, `info()` y `describe()`, confirmando 365 registros sin valores nulos.
* Se analizaron las relaciones entre las variables mediante `pairplot()` y matriz de correlación, detectando una relación casi perfecta entre CO y AQI (derivada de su definición) y relaciones débiles con las variables de completitud de datos.
* Se construyó un modelo de regresión lineal múltiple utilizando una división de 70 % para entrenamiento y 30 % para prueba (`random_state=123`).
* El modelo obtuvo un **R² de 0.990 en prueba**, con errores muy bajos (MAE ≈ 0.018 ppm, RMSE ≈ 0.023 ppm), evidenciando un ajuste excelente, aunque en gran parte explicado por la relación matemática directa con el AQI.
* El análisis de residuos no mostró patrones sistemáticos relevantes, respaldando razonablemente los supuestos de homoscedasticidad y normalidad de errores.
* Se generaron datos artificiales mediante `make_regression()` (100 muestras, 6 características, 3 informativas) para un análisis de validación cruzada metodológica.
* El árbol de decisión y el modelo OLS coincidieron en identificar correctamente las 3 variables realmente informativas de las 6 generadas, validando la coherencia de ambos enfoques.
* Como recomendación, se sugiere evaluar el modelo excluyendo `Daily AQI Value` de los predictores para obtener una medida más realista del poder predictivo del resto de variables disponibles.

---

## 5. Referencias

[1] Notebook de trabajo: *Sem5ProyectoIntegradorTarea_Ordenado (1).ipynb*, 2026.

[2] Conjunto de datos: *ad_viz_plotval_data.csv* — Concentración máxima diaria de CO (8 horas), estación Copper View, Salt Lake City, UT, 2022 (AQS/EPA AirData).
