# <h1 align="center">────୨ৎ──── TALLER DE REGRESIÓN LINEAL ────୨ৎ────</h1>

## ✿ Introducción

En este taller se trabajó con un conjunto de datos relacionado con el **consumo de energía**, utilizando Python para explorar la información y construir un modelo de regresión lineal. Las variables consideradas fueron **Temperatura, Horas de Operación, Carga y Humedad**, mientras que **Consumo de Energía** fue tomada como la variable que se busca explicar y predecir.

El desarrollo del taller permitió poner en práctica diferentes etapas del análisis de datos, desde conocer y revisar el conjunto de datos hasta entrenar un modelo y analizar los resultados obtenidos. A continuación, se presentan los códigos que consideramos más importantes y lo que se pudo comprender a partir de cada uno.

---

## ✿ Documentación

En esta sección se encuentran los archivos utilizados como apoyo y fuente de datos para el desarrollo del taller.


| Archivo                             | Descripción                                                                            | Visualización                                          |
| ----------------------------------- | -------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| 📓 **Notebook de Regresión Lineal** | Notebook utilizado para desarrollar el taller y aplicar el modelo de regresión lineal. | [🔎 Ver notebook](./Taller_Regresión_Lineal_Idania.ipynb) |
| 📊 **Tabla CSV**                    | Conjunto de datos utilizado para el análisis y construcción del modelo.                | [🔎 Ver tabla de datos](./Data_PI_regresion.csv)          |


---

## 1. Exploración inicial de los datos

Uno de los primeros pasos fue conocer cómo estaba conformado el conjunto de datos. Para ello se utilizaron funciones como `head()`, `info()` y `describe()`.

<p align="center">
  <img src="https://github.com/user-attachments/assets/7fb397ca-7295-449f-b3f0-3928905cf01c" width="600">
</p>

También se utilizó:
<p align="center">
  <img src="https://github.com/user-attachments/assets/fd2b8e64-cca8-4d0b-b529-088541ef958c" width="600">
</p>


y:
<p align="center">
  <img src="https://github.com/user-attachments/assets/88aee67e-b090-4568-badd-0d6d6b956184" width="600">
</p>

Estas instrucciones permitieron revisar las primeras observaciones, los tipos de datos, la cantidad de registros y algunas estadísticas descriptivas.

En este caso, se trabajó con **5000 registros y 5 variables numéricas**, sin valores nulos. Además, `describe()` permitió observar valores como la media, desviación estándar, mínimo, máximo y cuartiles de cada variable.

### Importancia:

Esta parte fue importante porque antes de aplicar cualquier modelo es necesario conocer los datos con los que se está trabajando. Revisar su estructura ayuda a detectar posibles problemas y a entender qué representa cada variable.

---

## 2. Definición de las variables para el modelo

Después de revisar los datos, se separaron las variables que serían utilizadas como características de entrada y la variable que se desea predecir.

<p align="center">
  <img src="https://github.com/user-attachments/assets/46d85795-a4a2-459e-83f5-be21538dd784" width="600">
</p>

En este caso, `X` contiene las variables:

* Temperatura
* Horas de Operación
* Carga
* Humedad

Mientras que `y` corresponde a:

* Consumo de Energía

Esta separación es necesaria para indicarle al modelo qué información utilizará para realizar las predicciones y cuál será el resultado que debe aprender a estimar.

### ¿Qué aprendimos?

Aquí comprendimos mejor la diferencia entre las **variables de entrada** y la **variable objetivo**. No se trata simplemente de ingresar todos los datos al modelo, sino de definir qué variables serán utilizadas para explicar o predecir el comportamiento de la variable objetivo.

---

## 3. División de los datos en entrenamiento y prueba

Para entrenar el modelo se dividió el conjunto de datos en dos partes:

<p align="center">
  <img src="https://github.com/user-attachments/assets/ff585bff-b6a2-4a95-a4e6-02550251bc00" width="600">
</p>

Se utilizó el **70 % de los datos para entrenamiento** y el **30 % para prueba**. El parámetro `random_state=123` permite mantener la misma división cada vez que se ejecuta el código.

### ¿Qué aprendimos?

Esta división permite que el modelo aprenda a partir de una parte de los datos y posteriormente pueda ser evaluado con datos que no utilizó durante el entrenamiento. De esta manera, podemos tener una idea de cómo se comporta el modelo frente a información que no ha visto anteriormente.

---

## 4. Entrenamiento del modelo de regresión lineal

Una de las partes principales del taller fue la creación y entrenamiento del modelo:

<p align="center">
  <img src="https://github.com/user-attachments/assets/294c81c9-d09c-4917-b43a-7059b6603396" width="600">
</p>

Con `LinearRegression()` se creó el modelo y mediante `fit()` se realizó el entrenamiento utilizando los datos de entrenamiento.

El modelo obtuvo una intersección de aproximadamente **2.7411** y los siguientes coeficientes:

<p align="center">
  <img src="https://github.com/user-attachments/assets/202638fa-fa16-4c39-9821-e8a47ff420c5" width="600">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/9e4e5646-f91f-4f97-90f1-9fcbb06ad9f5"  width="600">
</p>

Estos valores permiten interpretar cómo cambia el consumo de energía cuando aumenta una variable, manteniendo las demás constantes dentro del modelo.

### ¿Qué nos llamó la atención?

El coeficiente de **Horas de Operación** fue el más alto, con aproximadamente **1.6688**. Esto significa que, dentro del modelo obtenido, esta variable presenta el mayor cambio estimado en el consumo de energía por cada unidad adicional, manteniendo constantes las demás variables.

A partir de los coeficientes también se puede expresar el modelo de la siguiente manera:

```text
Consumo de Energía =
2.7411
+ 0.1371(Temperatura)
+ 1.6688(Horas de Operación)
+ 0.0963(Carga)
+ 0.0268(Humedad)
```

---

## 5. Análisis del error estándar y estadística t

Otro código que consideramos importante fue el utilizado para calcular el **error estándar** y la **estadística t** de los coeficientes.

<p align="center">
  <img src="https://github.com/user-attachments/assets/a9b59f0a-8878-4c27-9aa8-d092577ee5fc" width="600">
</p>

La estadística t permite relacionar el tamaño de cada coeficiente con su error estándar. En los resultados del taller, **Horas de Operación presentó la estadística t más alta**, seguida de Carga, Temperatura y Humedad.

### ¿Qué aprendimos?

Esta parte permitió ir un poco más allá de observar únicamente los coeficientes. Entendimos que también es necesario considerar el error asociado a cada estimación para analizar qué tan alejado se encuentra un coeficiente de cero en relación con su incertidumbre.

---

## 6. Predicciones y representación gráfica

Finalmente, se realizaron predicciones con el modelo y se utilizaron gráficos para observar la relación entre los valores reales y los valores predichos.

<p align="center">
  <img src="https://github.com/user-attachments/assets/3e42b321-0503-4877-b47c-2e45bedffab1" width="600">
</p>

La comparación gráfica permite observar qué tan cerca se encuentran las predicciones realizadas por el modelo de los valores reales.

### ¿Qué aprendimos?

La representación gráfica ayuda a interpretar el comportamiento del modelo de una manera más sencilla. No basta con obtener un resultado numérico; visualizar las predicciones permite identificar de forma más clara si existe una relación cercana entre lo que el modelo estima y los valores que realmente se tienen.

---

## ✿ Conclusiones

El desarrollo del taller permitió comprender de manera práctica cómo se construye un modelo de regresión lineal a partir de un conjunto de datos. Primero fue necesario conocer la estructura de la información y revisar sus principales características. Después, se definieron las variables de entrada y la variable objetivo para poder entrenar correctamente el modelo.

Uno de los resultados que más destacó fue el coeficiente correspondiente a Horas de Operación, cuyo valor fue aproximadamente **1.6688**, siendo el mayor de las variables consideradas. Además, el cálculo del error estándar y la estadística t permitió complementar la interpretación de los coeficientes y no quedarse únicamente con sus valores.

En general, el taller ayudó a entender que la regresión lineal no consiste solamente en ejecutar un modelo, sino en **revisar los datos, seleccionar correctamente las variables, entrenar el modelo y analizar qué significan los resultados obtenidos**. Esto permitió relacionar la parte teórica vista en clase con un caso aplicado de predicción del consumo de energía.
