t# Predicción de la Concentración Máxima Diaria de CO mediante Regresión Lineal Múltiple: Caso Salt Lake City, UT (2022)

## Introducción

La calidad del aire es uno de los factores ambientales que más impacto tiene sobre la salud de las personas, sobre todo en ciudades con tráfico vehicular constante e industria cercana. Entre los contaminantes que se monitorean con mayor frecuencia está el **monóxido de carbono (CO)**, un gas que no se puede ver ni oler, pero que en concentraciones altas es peligroso porque impide que la sangre transporte oxígeno de forma adecuada.

Este informe presenta un análisis realizado sobre datos reales de calidad del aire de la estación **Copper View**, en Salt Lake City, Utah, correspondientes al año 2022. El objetivo es construir un modelo de **regresión lineal múltiple** que permita predecir la concentración máxima diaria de CO (medida en un promedio móvil de 8 horas) a partir de otras variables numéricas disponibles en el conjunto de datos, y así entender qué tan bien se puede explicar el comportamiento de este contaminante y qué variables están más relacionadas con él.

Los datos utilizados fueron descargados directamente del portal de datos abiertos de la Agencia de Protección Ambiental de los Estados Unidos (EPA) [1].

## Metodología

El análisis se desarrolló siguiendo estos pasos:

1. **Carga y exploración de datos (EDA):** se importó el archivo `ad_viz_plotval_data.csv`, correspondiente a 365 registros diarios del año 2022 de la estación Copper View. Se revisó la estructura del dataset, tipos de datos, valores nulos (no se encontraron) y estadísticas descriptivas básicas.

2. **Análisis gráfico exploratorio:** se generaron gráficos de dispersión por pares (*pairplot*), un histograma y una curva de densidad de la variable objetivo, y una matriz de correlación (heatmap) entre las variables numéricas.

3. **Preparación de variables:** se definió como variable objetivo (**y**) la columna `Daily Max 8-hour CO Concentration`. Como variables predictoras (**X**) se conservaron únicamente las columnas numéricas relevantes, descartando identificadores, fechas, códigos y nombres de texto que no aportan valor predictivo (fecha, fuente, ID de sitio, unidades, nombre del sitio, códigos AQS/CBSA/FIPS, condado, estado, etc.).

4. **División de datos:** el conjunto se dividió en 70 % para entrenamiento y 30 % para prueba, usando `train_test_split` de scikit-learn con semilla fija (`random_state=123`) para que los resultados sean reproducibles.

5. **Entrenamiento del modelo:** se entrenó un modelo de **Regresión Lineal Múltiple** (`LinearRegression`) sobre el conjunto de entrenamiento.

6. **Significancia estadística:** se calculó el error estándar y el estadístico t de cada coeficiente, con el fin de identificar qué variables influyen de manera más fuerte y confiable sobre la predicción del CO.

7. **Evaluación del modelo:** se calculó el coeficiente de determinación (R²) tanto en entrenamiento como en prueba, además de las métricas de error MAE, MSE y RMSE sobre el conjunto de prueba. También se analizaron los residuos (diferencia entre valores reales y predichos) para verificar que el modelo cumple razonablemente los supuestos de normalidad y homocedasticidad.

8. **Pruebas complementarias (exploratorias):** de forma adicional y con fines comparativos, se realizaron pruebas metodológicas sobre un conjunto de datos sintético (generado con `make_regression`), incluyendo un modelo de **árbol de decisión** (`DecisionTreeRegressor`) para evaluar importancia de variables, y un ajuste de **Mínimos Cuadrados Ordinarios (OLS)** con la librería `statsmodels` para obtener un resumen estadístico más completo del modelo lineal. Estas pruebas no se aplicaron al conjunto de datos real de CO, sino que sirvieron como ejercicio metodológico de comparación entre técnicas.

## Resultados

A continuación se presentan los resultados obtenidos en cada etapa del análisis, con espacio para insertar las imágenes generadas en el notebook y su respectiva interpretación.

### Relaciones entre variables
![variasVariables](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana05-IA-Colab/CLASIFICACION/Brad_Cardenas/capturas/variasvariables.png)

### 1. Distribución de la variable objetivo

![Histograma concentración de CO](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana05-IA-Colab/CLASIFICACION/Brad_Cardenas/capturas/histograma.png)
![Densidad de la concentración de CO](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana05-IA-Colab/CLASIFICACION/Brad_Cardenas/capturas/densidad.png)

**Interpretación:** La concentración máxima diaria de CO durante 2022 tiene un promedio de **0.28 ppm**, con una desviación estándar de **0.23 ppm**. El valor mínimo registrado fue **0.0 ppm** y el máximo **1.0 ppm**. La mitad de los días registró concentraciones iguales o menores a **0.2 ppm** (mediana), lo que indica que la mayoría de los días tuvo niveles bajos de CO, con algunos picos ocasionales más altos que generan una distribución sesgada hacia la derecha (cola larga hacia valores altos). Esto es típico en variables de contaminación, donde la mayoría de los días son "normales" pero existen episodios puntuales de mayor contaminación.

---

### 2. Matriz de correlación

![Mapa de calor de correlaciones](imagenes/02_heatmap_correlacion.png)

**Interpretación:** La variable con mayor correlación con el CO es, por mucho, el **Daily AQI Value** (índice de calidad del aire diario), con una correlación de **0.996**, es decir, prácticamente perfecta. Esto tiene una explicación lógica: el AQI de ese día se calcula matemáticamente a partir de la concentración de CO (y de otros contaminantes), por lo que no es una variable "externa" que explique el CO, sino casi una transformación directa de la propia variable objetivo. Por otro lado, variables como `Daily Obs Count` (cantidad de observaciones horarias registradas en el día) y `Percent Complete` (porcentaje de completitud de las mediciones) muestran una correlación baja y negativa (alrededor de **-0.13**), lo cual sugiere una relación débil: días con más observaciones o mayor completitud de datos no necesariamente coinciden con más o menos CO.

---

### 3. Coeficientes del modelo y significancia estadística

![Tabla de coeficientes, error estándar y t-statistic](imagenes/03_tabla_coeficientes.png)

**Interpretación:** Los coeficientes muestran que, por cada unidad que sube el `Daily AQI Value`, la predicción de CO aumenta en promedio **0.085 ppm**, y por cada observación horaria adicional (`Daily Obs Count`), la predicción de CO aumenta **0.257 ppm**, mientras que un mayor `Percent Complete` está asociado a una ligera disminución del CO predicho (**-0.061**). Las variables `Site Latitude` y `Site Longitude` no aportan nada al modelo, ya que todos los registros provienen de la misma estación y por lo tanto son constantes (su coeficiente es prácticamente cero y no tienen ningún poder explicativo real). En cuanto a la significancia estadística, tanto el `Daily AQI Value` (t ≈ 189) como `Daily Obs Count` (t ≈ 114) y `Percent Complete` (t ≈ -114) tienen valores de t muy altos en valor absoluto, lo que indica que su efecto sobre el CO es estadísticamente muy sólido y no se debe al azar.

---

### 4. Relación de las variables más importantes con la variable objetivo

![Dispersión de las 4 variables más importantes vs. CO](imagenes/04_dispersión_variables_importantes.png)

**Interpretación:** Al graficar el `Daily AQI Value` contra el CO se observa una relación prácticamente lineal y muy estrecha, confirmando la fuerte correlación mencionada anteriormente. En cambio, variables como `Daily Obs Count` y `Percent Complete` muestran nubes de puntos mucho más dispersas, sin un patrón claro, lo que confirma que su relación con el CO es débil y probablemente poco útil desde un punto de vista práctico (más allá de que estadísticamente resulten "significativas" dentro del modelo).

---

### 5. Ajuste del modelo (R² de entrenamiento)

![Valor de R² en el conjunto de entrenamiento](imagenes/05_r2_entrenamiento.png)

**Interpretación:** El modelo obtuvo un **R² de entrenamiento de 0.993**, lo que significa que las variables utilizadas logran explicar el **99.3 %** de la variabilidad del CO en los datos de entrenamiento. Este valor tan alto se debe principalmente a la presencia del `Daily AQI Value`, que como se explicó, está matemáticamente ligado a la propia variable objetivo.

---

### 6. Valores reales vs. predichos (conjunto de prueba)

![Dispersión de valores reales vs. predichos](imagenes/06_reales_vs_predichos.png)

**Interpretación:** Los puntos se agrupan de forma muy cercana a la línea diagonal de referencia (predicción perfecta), lo cual indica que el modelo generaliza muy bien también en datos que no vio durante el entrenamiento. No se observan desviaciones importantes en ningún rango de valores, ni sobreestimación ni subestimación sistemática.

---

### 7. Análisis de residuos

![Histograma de residuos](imagenes/07_histograma_residuos.png)

**Interpretación:** El histograma de los residuos muestra una forma aproximadamente simétrica y centrada en cero, lo cual es una buena señal: sugiere que los errores del modelo se comportan de manera similar a una distribución normal, cumpliendo uno de los supuestos clave de la regresión lineal.

![Residuos vs. valores predichos](imagenes/08_residuos_vs_predichos.png)

**Interpretación:** Los residuos se distribuyen de forma bastante aleatoria alrededor de la línea horizontal en cero, sin formar un patrón en forma de embudo o de curva. Esto indica que no hay evidencia fuerte de heterocedasticidad (es decir, el error del modelo no crece ni se reduce sistemáticamente según el valor predicho), lo cual respalda la validez del modelo lineal utilizado.

---

### 8. Métricas finales de desempeño

![Métricas finales: MAE, MSE, RMSE y R²](imagenes/09_metricas_finales.png)

**Interpretación:** Sobre el conjunto de prueba, el modelo obtuvo:

- **MAE (Error Absoluto Medio):** 0.018 ppm
- **MSE (Error Cuadrático Medio):** 0.0005
- **RMSE (Raíz del Error Cuadrático Medio):** 0.023 ppm
- **R² de prueba:** 0.990

Considerando que los valores de CO en el conjunto de datos van de 0 a 1 ppm con una media de 0.28 ppm, un error promedio de apenas 0.018–0.023 ppm es muy pequeño en términos relativos. Además, el R² de prueba (0.990) es muy similar al de entrenamiento (0.993), lo que indica que el modelo **no está sobreajustado**: aprendió un patrón real y lo aplica igual de bien a datos nuevos.

## Discusión

Aunque el modelo obtuvo un desempeño estadístico excelente (R² cercano a 0.99), es importante interpretar este resultado con cuidado. La variable predictora más influyente, `Daily AQI Value`, no es una variable verdaderamente "externa" al CO: el índice de calidad del aire (AQI) se calcula a partir de la concentración de contaminantes, incluyendo el propio CO. Esto implica que una parte importante del alto poder predictivo del modelo proviene de una especie de "fuga de información" (*data leakage*), más que de un verdadero patrón causal entre variables independientes y el CO.

Por otro lado, dado que todos los registros provienen de una sola estación de monitoreo (Copper View), variables como la latitud y longitud del sitio resultan completamente constantes y sin utilidad predictiva, y el resto de variables disponibles (número de observaciones diarias, porcentaje de completitud) tienen una relación débil con el CO.

Como trabajo futuro, sería recomendable:

- Repetir el análisis excluyendo el `Daily AQI Value` para evaluar qué tan bien predicen el CO variables verdaderamente independientes (por ejemplo, temperatura, humedad, velocidad del viento o tráfico vehicular, si estuvieran disponibles).
- Incluir datos de varias estaciones de monitoreo y de varios años, para tener variabilidad geográfica y temporal real.
- Probar modelos no lineales o de regularización (Ridge, Lasso, árboles de decisión) que puedan capturar relaciones más complejas y sean más robustos frente a variables poco informativas.

## Referencias

[1] U.S. Environmental Protection Agency, "Air Quality Data — Salt Lake City, UT (Carbon Monoxide, 2022)," *AirData*, 2022. [Online]. Available: https://www.epa.gov/outdoor-air-quality-data/download-daily-data. [Accessed: 17-Sep-2026].
