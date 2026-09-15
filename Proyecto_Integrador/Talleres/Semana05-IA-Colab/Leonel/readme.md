# Regresión Lineal y Modelado Predictivo

En este proyecto desarrollo un análisis de datos enfocado en el **consumo de energía**, utilizando técnicas de análisis exploratorio y modelos de aprendizaje automático para estudiar la relación entre diferentes variables y el consumo energético.

Trabajo con un conjunto de datos de **5000 registros**, compuesto por las variables `Temperatura`, `Horas_Operacion`, `Carga`, `Humedad` y `Consumo_Energia`. Mi objetivo principal es analizar cómo las condiciones de operación y las variables ambientales se relacionan con el consumo de energía y utilizar esta información para realizar predicciones.

Comienzo realizando una exploración general del conjunto de datos mediante **Pandas**, revisando su estructura, tipos de datos, cantidad de registros, valores nulos y estadísticas descriptivas. También analizo la distribución y relación entre las variables mediante diferentes visualizaciones, como gráficos de dispersión, `pairplot` y matrices de correlación, buscando identificar patrones y posibles relaciones relevantes.

Posteriormente, preparo los datos para construir modelos predictivos y aplico **Regresión Lineal**, utilizando las variables disponibles como características para estimar el `Consumo_Energia`. Analizo los resultados obtenidos por el modelo, sus coeficientes y diferentes métricas de evaluación para comprender qué tan bien logra representar los datos.

Además, complemento el análisis utilizando **`statsmodels`**, lo que me permite profundizar en la interpretación estadística del modelo mediante elementos como los coeficientes, errores estándar y pruebas estadísticas. También realizo un análisis de los **residuos**, con el propósito de observar los errores de predicción y comprobar el comportamiento del modelo.

Finalmente, incorporo un **Árbol de Decisión para regresión** como una alternativa a la regresión lineal. Esto me permite comparar diferentes enfoques de modelado y analizar la importancia de las variables utilizadas para explicar el consumo energético.

Para desarrollar el proyecto utilizo principalmente **Python**, junto con librerías como `NumPy`, `Pandas`, `Matplotlib`, `Seaborn`, `Scikit-learn` y `Statsmodels`. El notebook está desarrollado en **Google Colab** y busca mostrar de manera práctica el proceso completo de exploración de datos, construcción de modelos, evaluación e interpretación de resultados.
