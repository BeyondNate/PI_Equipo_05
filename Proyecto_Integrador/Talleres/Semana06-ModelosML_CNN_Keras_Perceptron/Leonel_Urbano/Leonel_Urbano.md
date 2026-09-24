# REDES NEURONALES Y SU APLICACIÓN
## 1. Introducción

Las redes neuronales artificiales constituyen una herramienta de inteligencia artificial que permite desarrollar sistemas capaces de identificar patrones, realizar predicciones y tomar decisiones a partir de datos. Dentro de este campo se encuentran el **Perceptrón**, las **Redes Neuronales Convolucionales (CNN)** y herramientas de desarrollo como **Keras**.

En los ejercicios desarrollados en Google Colab se estudian estos conceptos mediante ejemplos de clasificación de imágenes, clasificación binaria y funcionamiento de una neurona artificial. En el caso de la CNN, el conjunto de datos utilizado considera dos clases: **glass = 0** y **plastic = 1**, con una división de los datos en entrenamiento, validación y prueba. El código del conjunto de datos establece una división de 70 % para entrenamiento, 15 % para validación y 15 % para prueba.

El presente informe interpreta los conceptos de CNN, Keras y Perceptrón, analiza los resultados y gráficas obtenidos en los ejercicios y plantea una posible aplicación de estas herramientas a un proyecto universitario de **automatización del proceso de aclaramiento de agua mediante una solución de quitosano a baja escala**.

---

# 2. CNN: Red Neuronal Convolucional

## 2.1. Concepto

CNN significa **Convolutional Neural Network**, o **Red Neuronal Convolucional**. Es una arquitectura de red neuronal especialmente utilizada para procesar imágenes y otros datos que poseen una estructura espacial.

Su principal característica es el uso de filtros o *kernels* que recorren la imagen para detectar características. En las primeras capas pueden identificarse elementos simples, como bordes y cambios de intensidad, mientras que en capas posteriores se combinan estas características para reconocer formas y estructuras más complejas.

En el ejercicio de clasificación, la CNN se utiliza para diferenciar imágenes correspondientes a las clases:

- **0 = glass**
- **1 = plastic**

## 2.2. Componentes principales

### Convolución(Conv2D)

La convolución utiliza filtros que recorren la imagen. Cada filtro aprende a detectar determinados patrones.

### Activación(ReLU)

La función ReLU se expresa como:

`ReLU(x) = max(0,x)`

Permite introducir no linealidad y facilita que la red aprenda relaciones complejas.

### Pooling(MaxPool)

El *MaxPooling* reduce las dimensiones de la información obtenida, conservando las características más importantes y disminuyendo el costo computacional.

### Capas fully-connected (densas)

Después de extraer las características, estas se utilizan para determinar la clase correspondiente a la imagen.


# 3. Interpretación de las gráficas de la CNN

## 3.1. Pérdida durante el entrenamiento

La gráfica de **Loss** permite observar cómo cambia el error del modelo a medida que aumentan las épocas.

En el entrenamiento de la CNN desde cero, la pérdida presenta una reducción progresiva. Esto significa que el modelo está aprendiendo a partir de los datos de entrenamiento.

Sin embargo, la disminución no es muy pronunciada. Por ello, se observa que el aprendizaje del modelo es limitado y que la red tiene dificultades para encontrar características suficientemente discriminantes entre vidrio y plástico.

<img width="863" height="395" alt="image" src="https://github.com/user-attachments/assets/804f366f-32c7-4d7e-972a-9462b96f3dbe" />


### Interpretación

Una reducción de `Loss` significa que el modelo está mejorando su ajuste a los datos de entrenamiento. No obstante, una pérdida que permanece relativamente elevada indica que el modelo todavía presenta errores importantes.

---

## 3.2. Accuracy y ROC-AUC

La gráfica de **Accuracy** permite observar el porcentaje de predicciones correctas.

En el experimento, la precisión de validación aumenta progresivamente y alcanza aproximadamente **63.27 % en la época 8**.

El valor de **ROC-AUC** se mantiene aproximadamente entre 0.67 y 0.69 durante el entrenamiento.

### Interpretación

El aumento de la accuracy indica que la CNN está aprendiendo a distinguir parcialmente las dos categorías. Sin embargo, los valores muestran que todavía existe una cantidad importante de errores.

El ROC-AUC indica la capacidad del modelo para separar las clases. Los valores obtenidos muestran que existe capacidad de discriminación, aunque todavía limitada.


## 3.3. Matriz de confusión

La matriz de confusión del modelo CNN presenta:

<img width="364" height="341" alt="image" src="https://github.com/user-attachments/assets/cf021a71-9d8e-46cb-9f8e-578259daee91" />


La interpretación es:

- 36 imágenes de vidrio fueron clasificadas correctamente.
- 48 imágenes de vidrio fueron clasificadas como plástico.
- 25 imágenes de plástico fueron clasificadas como vidrio.
- 40 imágenes de plástico fueron clasificadas correctamente.

El modelo obtiene aproximadamente:

- **Accuracy = 56.38 %**
- **ROC-AUC = 61.91 %**

### Interpretación

El resultado evidencia que el modelo entrenado desde cero presenta dificultades para diferenciar las clases. Aunque reconoce correctamente una parte de las imágenes, la cantidad de errores todavía es considerable.


# 4. CNN con aumento de datos

El **Data Augmentation** consiste en aplicar transformaciones a las imágenes de entrenamiento, por ejemplo pequeñas rotaciones o desplazamientos.

Su finalidad es aumentar la variedad de ejemplos disponibles para la red y mejorar su capacidad de generalización.

En el experimento:

- CNN sin aumento: **Accuracy = 56.38 %**
- CNN con aumento: **Accuracy = 54.36 %**

Además:

- CNN sin aumento: **ROC-AUC = 61.91 %**
- CNN con aumento: **ROC-AUC = 63.32 %**

### Interpretación

El aumento de datos produce una mejora respecto al modelo original, aunque la mejora es moderada.

Esto demuestra que las transformaciones ayudan a la red a trabajar con imágenes ligeramente diferentes a las originales, pero no solucionan por completo las limitaciones del modelo.


# 5. Aprendizaje por Transfer Learning con ResNet

El aprendizaje por transferencia consiste en utilizar una red neuronal previamente entrenada y adaptarla a un nuevo problema.

En el ejercicio se utiliza **ResNet**.

Los resultados obtenidos y la matriz de confusion son:

<img width="552" height="285" alt="image" src="https://github.com/user-attachments/assets/f4a43e07-f946-4680-9bfa-56168da27e4d" />

### Interpretación

El modelo identifica correctamente:

- 73 imágenes de vidrio.
- 61 imágenes de plástico.

Los errores son:

- 3 vidrios clasificados como plástico.
- 12 plásticos clasificados como vidrio.

La comparación de los modelos es:

<img width="508" height="87" alt="image" src="https://github.com/user-attachments/assets/9d0645d4-365e-4a1d-b397-31fcc160d2d5" />

Estos resultados muestran que el aprendizaje por transferencia consigue un rendimiento considerablemente mayor en este experimento.


# 6. Keras

## 6.1. Concepto

**Keras** es una herramienta de alto nivel para construir, entrenar y evaluar modelos de aprendizaje automático y redes neuronales.

Es importante diferenciar Keras de CNN y Perceptrón:

- **Perceptrón:** modelo neuronal sencillo.
- **CNN:** arquitectura de red neuronal especializada principalmente en imágenes.
- **Keras:** herramienta que permite programar y entrenar diferentes modelos.

En el ejercicio de Keras se trabaja con un problema de clasificación binaria utilizando el conjunto de datos IMDB.

El modelo está compuesto por capas de entrada, capas ocultas y una capa de salida. En las capas ocultas se emplea la función ReLU y en la salida una función sigmoide.

---

# 7. Interpretación de las gráficas de Keras

## 7.1. Pérdida de entrenamiento y validación

<img width="826" height="813" alt="image" src="https://github.com/user-attachments/assets/fc4c3d64-ea87-4361-8d57-5431b155c7b9" />

Azul: error del modelo en entrenamiento
Anaranjado: error sobre datos de validación

### Interpretación

Este comportamiento es característico del **sobreajuste u overfitting**.

El modelo empieza a adaptarse demasiado a los datos de entrenamiento y pierde capacidad de generalización frente a datos nuevos.

En la evaluación se obtiene aproximadamente:

- **Loss = 0.5871**
- **Accuracy = 85.96 %**

La accuracy es considerable, pero la gráfica demuestra que es necesario controlar el sobreajuste.

## 7.2. Comparación con un modelo más pequeño

<img width="835" height="813" alt="image" src="https://github.com/user-attachments/assets/9283297f-644c-44cc-aad3-2fcc7449f6ab" />

En el segundo experimento se reduce el número de neuronas de la capa oculta.


### Interpretación

Reducir el tamaño de la red puede disminuir la tendencia al sobreajuste, debido a que el modelo tiene menos parámetros para memorizar los datos. Con la muestra más pequeña el mínimo de pérdida se mantiene durante más épocas y el incremento posterior es mucho menor

Sin embargo, reducir demasiado la complejidad también puede limitar la capacidad de aprendizaje.


## 7.3. Regularización

<img width="826" height="813" alt="image" src="https://github.com/user-attachments/assets/12662f0d-1ebc-4472-84ae-a006ab475ef0" />

Vemos un pico porque el modelo aprende directamente de los datos de entrenamiento.
La regularización modifica el aprendizaje y puede producir un error más alto en algunas épocas


La regularización incorpora una penalización sobre los pesos del modelo. La gráfica compara el comportamiento del modelo original con el modelo regularizado.

### Interpretación

La regularización busca evitar que los pesos alcancen valores excesivos y reducir la dependencia del modelo respecto a determinadas características de los datos de entrenamiento. Su objetivo principal es mejorar la generalización.


## 7.4. Dropout

El ejercicio también utiliza **Dropout del 50 %**.

Durante el entrenamiento se desactivan aleatoriamente el 50% de las neuronas. esto obliga a la red a aprender de diferentes combinaciones de neuronas

<img width="835" height="813" alt="image" src="https://github.com/user-attachments/assets/243f81d4-90bd-477d-a2e9-78166e0959fe" />

### Interpretación

El Dropout constituye una técnica para reducir el sobreajuste.

La comparación de las curvas de validación permite analizar si esta técnica consigue controlar mejor el comportamiento del modelo frente a datos que no fueron utilizados directamente durante el entrenamiento.

## 7.5. Predicciones

<img width="416" height="183" alt="image" src="https://github.com/user-attachments/assets/38851647-c185-4142-9413-80ee17eb2035" />

El modelo tiene una certeza o probabilidad del 99.4% de que el texto analizado en el índice 10 corresponde a una reseña positiva. Es una predicción con un nivel de confianza extremadamente alto.

# 8. Perceptrón

## 8.1. Concepto

El Perceptrón es uno de los modelos más sencillos de una neurona artificial.

Recibe varias entradas, las multiplica por pesos, agrega un sesgo y aplica una función de activación. El Perceptrón permite comprender el principio fundamental de muchas redes neuronales: transformar entradas mediante pesos para producir una salida.

# 9. Ejemplo de sobrecalentamiento

En el ejemplo se utilizan variables como:

- Temperatura.
- Vibración.

Los pesos y el bias determinan la decisión final.

Se obtiene:

<img width="565" height="157" alt="image" src="https://github.com/user-attachments/assets/fff0953b-f414-4b47-84f9-99c8c92fbd38" />

- Función escalón = **0**
- Función tanh ≈ **-0.9999**

### Interpretación

El valor de la suma ponderada resulta negativo. Por esta razón, la función escalón genera una salida igual a 0 y la función `tanh` produce un valor cercano a -1.

El ejemplo demuestra cómo un Perceptrón puede utilizar variables de entrada para producir una decisión.


# 10. Perceptrón tipo AND

El Perceptrón puede representar la operación lógica AND.

<img width="370" height="270" alt="image" src="https://github.com/user-attachments/assets/96b6a647-900c-485d-a8c5-eb0fa8c6bdf0" />

### Interpretación

La salida solamente es 1 cuando las dos entradas tienen valor 1. Esto demuestra que el problema AND puede ser resuelto mediante una única frontera de decisión lineal.

# 11. Perceptrón tipo OR

<img width="375" height="135" alt="image" src="https://github.com/user-attachments/assets/5ef4f8f4-9000-4690-9f70-418b750cdcda" />

### Interpretación

La salida es 1 cuando al menos una de las entradas es 1. Al igual que AND, OR puede representarse mediante un Perceptrón simple.

# 12. Compuerta XOR

<img width="503" height="505" alt="image" src="https://github.com/user-attachments/assets/9a06b35f-c16c-4f19-8684-995ed56c3aa4" />

### Interpretación de la gráfica

Los puntos correspondientes a las clases no pueden separarse correctamente mediante una única frontera lineal.

Por esta razón:

**Un solo Perceptrón no puede resolver XOR.**

Para resolver este tipo de problema es necesario utilizar una arquitectura con más de una neurona y, normalmente, más de una capa. Este ejemplo permite observar una de las principales limitaciones del Perceptrón simple.


# 14. Aplicación al proyecto de aclaramiento de agua con quitosano

## 14.1. Descripción

El proyecto universitario consiste en desarrollar a baja escala un sistema automatizado para el **aclaramiento de agua mediante una solución de quitosano**.

El sistema podría medir diferentes variables del proceso, como:

- Turbidez inicial.
- Turbidez final.
- pH.
- Temperatura.
- Dosis de quitosano.
- Tiempo de mezcla.
- Velocidad de agitación.

Estas variables pueden utilizarse como entradas para un modelo de aprendizaje automático.

---

# 15. ¿Cuál de los tres utilizar?

Para un proyecto de baja escala basado principalmente en **datos numéricos de sensores**, una alternativa inicial sería utilizar un **modelo neuronal sencillo basado en el concepto de Perceptrón**, implementado mediante **Keras**.


# 16. ¿Cómo utilizar el Perceptrón?

Se podría desarrollar inicialmente un sistema de clasificación:

- `0 = proceso no adecuado`
- `1 = proceso adecuado`

Las variables de entrada podrían ser:

- `X1 = pH`
- `X2 = turbidez inicial`
- `X3 = dosis de quitosano`
- `X4 = temperatura`
- `X5 = tiempo de mezcla`
- `X6 = velocidad de agitación`

El modelo calcularía:

`z = X1W1 + X2W2 + X3W3 + X4W4 + X5W5 + X6W6 + b`

Después utilizaría una función de activación para generar la decisión.

Por ejemplo:

`Salida = 1 → condiciones adecuadas`

`Salida = 0 → condiciones no adecuadas`


# 18. Función de Keras en el proyecto

Keras podría utilizarse para construir y entrenar la red neuronal.

Una arquitectura general sería:

**Sensores → adquisición de datos → procesamiento → modelo Keras → predicción → sistema de control**

Los sensores proporcionarían las variables del proceso.

Los datos obtenidos experimentalmente formarían el conjunto de entrenamiento.

Después de entrenar el modelo, este podría recibir nuevas condiciones de operación y generar una predicción.

# 22. Conclusiones

1. El **Perceptrón** representa una forma básica de neurona artificial y permite comprender el funcionamiento de las redes neuronales mediante entradas, pesos, bias y funciones de activación.

2. La **CNN** es una arquitectura especializada en el procesamiento de imágenes. En el ejercicio de clasificación de vidrio y plástico, el modelo entrenado desde cero obtuvo resultados limitados, mientras que el aprendizaje por transferencia con ResNet alcanzó mejores resultados dentro del conjunto de datos utilizado.

3. **Keras** es una herramienta para implementar y entrenar modelos de aprendizaje automático y redes neuronales. En el ejercicio permitió estudiar clasificación binaria y técnicas para controlar el sobreajuste, como la reducción de la arquitectura, regularización y Dropout.

4. Las gráficas de entrenamiento y validación permiten identificar el comportamiento del aprendizaje. Cuando la pérdida de entrenamiento disminuye mientras la pérdida de validación aumenta, existe evidencia de sobreajuste.

5. Para el proyecto de **aclaramiento de agua con quitosano a baja escala**, si las entradas principales son variables numéricas provenientes de sensores, se puede comenzar con un modelo neuronal sencillo y utilizar Keras como herramienta de implementación.

6. La **CNN** sería especialmente útil si posteriormente se incorpora una cámara para analizar imágenes del agua y determinar visualmente su nivel de claridad.

7. Una implementación progresiva permitiría comenzar con sensores, generar datos experimentales, entrenar un modelo, evaluar sus predicciones y posteriormente integrar el modelo al sistema automatizado de dosificación y control.


# 23. Arquitectura Hipotética

La estructura conceptual recomendada para desarrollar el proyecto sería:

```text
MEDICIÓN
   ↓
Sensores de pH, temperatura y turbidez
   ↓
ADQUISICIÓN DE DATOS
   ↓
Base de datos experimental
   ↓
ENTRENAMIENTO
   ↓
Red neuronal implementada con Keras
   ↓
PREDICCIÓN
   ↓
Dosis/condición de operación
   ↓
ACTUADORES
   ↓
Bomba de dosificación + mezclador
   ↓
MEDICIÓN DE TURBIDEZ FINAL
   ↓
RETROALIMENTACIÓN
```

De esta manera, el proyecto puede integrar **instrumentación, inteligencia artificial y automatización** en un sistema de tratamiento de agua a pequeña escala.
