# Taller de Modelado — Calidad del Aire (AQI) por CBSA 2023

Análisis de regresión sobre el dataset real `annual_aqi_by_cbsa_2023.csv` (499 áreas metropolitanas de EE. UU.). Variable objetivo: **Median AQI**. Predictores: días por categoría de AQI y días por contaminante dominante.

---

## Código completo

```python
# Importa bibliotecas para manipulación de datos y visualización.
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline
```

```python
# Carga los datos del archivo CSV.
df1 = pd.read_csv("annual_aqi_by_cbsa_2023.csv")
df1.head()
```

```python
# Información del DataFrame.
df1.info(verbose=True)
```

```python
# Estadísticas descriptivas.
df1.describe().round(1)
```

```python
# Nombres de columnas.
df1.columns
```

```python
# Pairplot de variables relevantes.
cols_pairplot = ['Good Days', 'Moderate Days', 'Unhealthy Days',
                  'Max AQI', '90th Percentile AQI', 'Median AQI']
sns.pairplot(df1[cols_pairplot])
```

```python
# Histograma de la variable objetivo.
df1['Median AQI'].plot.hist(bins=25, figsize=(8,4))
```

```python
# Densidad (KDE) de la variable objetivo.
df1['Median AQI'].plot.density()
```

```python
# Matriz de correlación.
numeric_df1 = df1.select_dtypes(include=[np.number]).drop(columns=['CBSA Code', 'Year'])
numeric_df1.corr().round(4)
```

```python
# Heatmap de correlaciones.
plt.figure(figsize=(12,9))
sns.heatmap(numeric_df1.corr(), annot=True, linewidths=2, fmt=".2f")
plt.title("Correlación entre variables de calidad del aire")
```

```python
# Definición de predictores (x) y objetivo (y).
feature_cols = ['Good Days', 'Moderate Days', 'Unhealthy for Sensitive Groups Days',
                'Unhealthy Days', 'Very Unhealthy Days', 'Hazardous Days',
                'Days CO', 'Days NO2', 'Days Ozone', 'Days PM2.5', 'Days PM10']
target_col = 'Median AQI'
one_column = feature_cols + [target_col]
len_feature = len(one_column)
```

```python
# Separación x / y.
x = df1[feature_cols]
y = df1[target_col]
x.head()
```

```python
from sklearn.model_selection import train_test_split
```

```python
x_train, x_test, y_train, y_test = train_test_split(
    x, y, test_size=0.3, random_state=123
)
```

```python
from sklearn.linear_model import LinearRegression
from sklearn import metrics
```

```python
lm = LinearRegression()
lm.fit(x_train, y_train)
```

```python
print("el termino de interseccion del modelo lineal", lm.intercept_)
print("Los coeficientes del modelo lineal", lm.coef_)
```

```python
# Coeficientes en DataFrame.
cdf = pd.DataFrame(lm.coef_, x.columns, columns=['Coeficientes'])
cdf
```

```python
# Errores estándar y t-statistic.
n = x_train.shape[0]
k = x_train.shape[1]
dfN = n - k
train_pred = lm.predict(x_train)
train_error = np.square(train_pred - y_train)
sum_error = np.sum(train_error)
se = [0]*k
for i in range(k):
  r = (sum_error/dfN)
  r = r/np.sum(np.square(x_train[list(x_train.columns)[i]]-x_train[list(x_train.columns)[i]].mean()))
  se[i] = np.sqrt(r)

cdf['Standard Error'] = se
cdf['t-statistic'] = cdf['Coeficientes']/cdf['Standard Error']
cdf
```

```python
# Dispersión de las primeras 4 variables vs Median AQI.
l = list(cdf.index)
from matplotlib import gridspec
fig = plt.figure(figsize=(18, 10))
gs = gridspec.GridSpec(2, 2)

ax0 = plt.subplot(gs[0]); ax0.scatter(df1[l[0]], df1[target_col]); ax0.set_title(l[0] + " vs. Y")
ax1 = plt.subplot(gs[1]); ax1.scatter(df1[l[1]], df1[target_col]); ax1.set_title(l[1] + " vs. Y")
ax2 = plt.subplot(gs[2]); ax2.scatter(df1[l[2]], df1[target_col]); ax2.set_title(l[2] + " vs. Y")
ax3 = plt.subplot(gs[3]); ax3.scatter(df1[l[3]], df1[target_col]); ax3.set_title(l[3] + " vs. Y")
```

```python
# Predicciones vs reales.
predictions = lm.predict(x_test)
plt.figure(figsize=(10,7))
plt.title("Median AQI real vs. Median AQI predicho")
plt.xlabel("Median AQI real"); plt.ylabel("Median AQI predicho")
plt.scatter(x=y_test, y=predictions)
```

```python
# Histograma de residuos.
plt.figure(figsize=(10,7))
plt.title("Histograma de residuos para verificar la normalidad")
sns.histplot([y_test - predictions], kde=True)
```

```python
# Residuos vs predichos.
plt.figure(figsize=(10,7))
plt.title("Valores residuales vs. predichos")
plt.scatter(x=predictions, y=y_test-predictions)
```

```python
# Métricas de error.
print("MAE:", metrics.mean_absolute_error(y_test, predictions))
print("MSE:", metrics.mean_squared_error(y_test, predictions))
print("RMSE:", np.sqrt(metrics.mean_squared_error(y_test, predictions)))
print("R2:", metrics.r2_score(y_test, predictions))
```

```python
# Árbol de decisión.
from sklearn import tree
tree_model = tree.DecisionTreeRegressor(max_depth=5, random_state=10)
tree_model.fit(x_train, y_train)
```

```python
# Predicciones del árbol y MSE.
test_pred = tree_model.predict(x_test)
plt.scatter(x=y_test, y=test_pred)
print("Mean square error (MSE):", metrics.mean_squared_error(y_test, test_pred))
```

```python
# Importancia de características.
n_features = len(feature_cols)
plt.barh(range(n_features, 0, -1), width=tree_model.feature_importances_, height=0.5)
plt.yticks(range(n_features, 0, -1), feature_cols)
```

```python
# Modelo OLS (statsmodels).
import statsmodels.api as sm
Xs = sm.add_constant(x)
stat_model = sm.OLS(y, Xs)
stat_result = stat_model.fit()
print(stat_result.summary())
```

---

## Interpretaciones (resumidas)

**Pairplot:** más "Good Days" se asocia con AQI más bajo; más "Moderate/Unhealthy Days" se asocia con AQI más alto. Max AQI y 90th Percentile AQI están muy relacionados entre sí.

**Histograma de Median AQI:** distribución concentrada entre 30-60, con cola larga a la derecha (pocas ciudades muy contaminadas).

**Densidad de Median AQI:** confirma la asimetría hacia la derecha observada en el histograma.

**Heatmap de correlaciones:** Moderate Days es la variable más correlacionada con Median AQI (0.85, positiva); Good Days le sigue con -0.63 (negativa). CO y NO2 casi no correlacionan con nada; Ozone y PM2.5 sí influyen.

**Dispersión de 4 variables vs Y:** Good Days baja el AQI, Moderate Days lo sube de forma bastante lineal; Unhealthy(-Sensitive) Days están muy dispersas y cercanas a cero.

**Real vs. predicho (regresión lineal):** buen ajuste general (R²=0.84), con más error en los valores altos de AQI.

**Histograma de residuos:** centrados en cero, forma aproximadamente normal con leve cola derecha.

**Residuos vs. predichos:** dispersión ligeramente creciente con el valor predicho (leve heterocedasticidad).

**Real vs. predicho (árbol de decisión):** ajuste algo mejor que la regresión lineal (MSE 12.29 vs 15.60), con el patrón "escalonado" típico de los árboles.

**Importancia de características (árbol):** Moderate Days domina (~0.62), seguida de Days Ozone (~0.19) y Good Days (~0.18); el resto aporta casi nada.

---

## Conclusiones finales

El AQI mediano de las ciudades de EE. UU. en 2023 se explica principalmente por cuántos días caen en las categorías "Good" y "Moderate", mientras que contaminantes como CO y NO2 casi no aportan información. Tanto la regresión lineal (R²=0.84) como el OLS (R²=0.88) logran buen ajuste, y el árbol de decisión reduce aún más el error, sugiriendo relaciones no del todo lineales. El OLS mostró además una matriz de diseño con rango deficiente (multicolinealidad), por lo que convendría depurar variables redundantes o aplicar regularización (Ridge/Lasso) en un análisis más riguroso. En general, el flujo de trabajo (EDA → regresión lineal → árbol de decisión → OLS) se aplicó correctamente sobre los datos reales, con resultados predictivos consistentes entre modelos.
