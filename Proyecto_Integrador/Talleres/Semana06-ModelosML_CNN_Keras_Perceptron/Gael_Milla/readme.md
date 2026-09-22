# 🧠 Redes Neuronales: CNN, Keras y Perceptrón

## 1. Introducción

En esta actividad se trabajaron diferentes conceptos relacionados con las **redes neuronales artificiales**, principalmente las **Redes Neuronales Convolucionales (CNN)**, el uso de **Keras** para crear modelos de clasificación y el funcionamiento de un **Perceptrón**.

A través de diferentes ejemplos se pudo observar cómo una red neuronal puede aprender a partir de datos y posteriormente realizar predicciones.

---

# 2. 🖼️ Redes Neuronales Convolucionales (CNN)

Una **CNN (Convolutional Neural Network)** es un tipo de red neuronal utilizada principalmente para trabajar con **imágenes**.

Su funcionamiento consiste en analizar diferentes partes de una imagen y aprender características que permitan identificar objetos o clasificarlos.

Por ejemplo:

```text
Imagen
   ↓
Extracción de características
   ↓
Patrones y formas
   ↓
Clasificación
   ↓
Resultado
```

Una CNN puede aprender progresivamente características más complejas:

```text
Píxeles → bordes → formas → partes del objeto → objeto completo
```

### Componentes principales

#### 🔹 Convolución (Conv2D)

La capa de convolución utiliza **filtros o kernels** que recorren la imagen para encontrar características importantes.

Estos filtros pueden detectar patrones como bordes, formas o texturas.

#### 🔹 ReLU

Es una función de activación que ayuda a introducir no linealidad en la red.

#### 🔹 Pooling

Permite reducir el tamaño de los datos manteniendo información importante. Esto ayuda a disminuir el costo computacional del modelo.

---

### 📌 Espacio para imagen

> **[Colocar aquí una imagen que represente el funcionamiento de una CNN o las capas convolucionales]**

---

# 3. 🗑️ CNN aplicada a clasificación de imágenes

En el documento se utiliza el dataset **TrashNet**, que contiene imágenes de diferentes tipos de residuos.

El objetivo es utilizar una CNN para realizar una **clasificación automática de imágenes**.

Primero se prepara el entorno utilizando herramientas como:

- PyTorch
- Torchvision
- NumPy
- Matplotlib
- Scikit-learn
- Pillow

Un código importante para seleccionar el dispositivo de entrenamiento es:

```python
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
```

Este código permite utilizar la **GPU mediante CUDA** cuando está disponible. La GPU puede realizar muchas operaciones en paralelo, lo que resulta útil para entrenar redes neuronales.

---

### 📊 Carga y preparación de datos

También se utilizan transformaciones para preparar las imágenes antes de entregarlas al modelo.

Por ejemplo:

```python
transform_basic = T.Compose([
    T.ToTensor()
])
```

Esto convierte las imágenes a tensores que pueden ser utilizados por PyTorch.

Posteriormente se utilizan `DataLoader` para organizar los datos en grupos o **batches**:

```python
train_loader = DataLoader(
    train_dataset,
    batch_size=batch_size,
    shuffle=True
)
```

---

### 📌 Espacio para imagen

> **[Colocar aquí una captura de las imágenes del dataset TrashNet]**

---

# 4. 🧠 CNN entrenada desde cero

En el documento se construye una CNN básica utilizando bloques de:

- Convolución
- ReLU
- Pooling

La estructura principal se crea mediante una clase:

```python
class SimpleCNN(nn.Module):
    ...
```

Esto permite definir la arquitectura de la red.

Para entrenar el modelo se utiliza una función de pérdida y un optimizador:

```python
criterion = nn.CrossEntropyLoss()

optimizer = torch.optim.Adam(
    model_scratch.parameters(),
    lr=1e-3
)
```

`CrossEntropyLoss` permite calcular el error de clasificación, mientras que `Adam` se encarga de actualizar los pesos del modelo durante el entrenamiento.

---

## 4.1 📈 Evaluación del modelo

Durante el entrenamiento se analizan diferentes métricas, entre ellas:

- **Accuracy:** porcentaje de predicciones correctas.
- **ROC-AUC:** permite analizar la capacidad del modelo para separar las diferentes clases.
- **Matriz de confusión:** permite observar qué clases fueron clasificadas correctamente o confundidas.

En el documento se observa que el modelo alcanza aproximadamente **63.27 % de exactitud** en uno de los resultados mostrados.

### 📌 Espacio para imagen

> **[Colocar aquí las curvas de entrenamiento]**

### 📌 Espacio para imagen

> **[Colocar aquí la matriz de confusión]**

---

# 5. 🔄 Data Augmentation

El **Data Augmentation** consiste en realizar pequeñas modificaciones a las imágenes de entrenamiento para aumentar la variedad de los datos.

En el documento se utilizan transformaciones como:

```python
T.RandomRotation(degrees=10)
```

y:

```python
T.RandomAffine(
    degrees=0,
    translate=(0.05, 0.05)
)
```

Esto permite que el modelo vea imágenes ligeramente diferentes y pueda mejorar su capacidad de generalización.

Sin embargo, el aumento de datos debe utilizarse con cuidado, ya que una transformación demasiado fuerte podría modificar características importantes de la imagen.

---

# 6. 🔁 Transfer Learning

El **Transfer Learning** consiste en utilizar un modelo que ya fue entrenado previamente y adaptarlo a un nuevo problema.

En el documento se utiliza:

```python
resnet = models.resnet18(
    weights=models.ResNet18_Weights.DEFAULT
)
```

Se utiliza **ResNet18**, un modelo previamente entrenado, y se modifica su última capa para adaptarlo al problema de clasificación.

La principal ventaja es que el modelo ya posee características aprendidas de imágenes, por lo que no es necesario comenzar completamente desde cero.

También se realiza **Fine-tuning**, donde algunas capas del modelo previamente entrenado se vuelven a entrenar:

```python
if name.startswith("layer4") or name.startswith("fc"):
    param.requires_grad = True
```

---

### 📌 Espacio para imagen

> **[Colocar aquí una comparación entre CNN desde cero y Transfer Learning]**

---

# 7. 🔎 Grad-CAM

El documento también presenta **Grad-CAM**, una técnica de interpretabilidad.

Su objetivo es generar un mapa de calor que permita observar **qué regiones de una imagen tuvieron mayor influencia en la predicción del modelo**.

Esto permite tener una idea de dónde está concentrando su atención la red neuronal.

```text
Imagen
   ↓
Modelo CNN
   ↓
Predicción
   ↓
Grad-CAM
   ↓
Mapa de regiones importantes
```

### 📌 Espacio para imagen

> **[Colocar aquí la imagen obtenida mediante Grad-CAM]**

---

# 8. 💾 Guardar y cargar modelos

Finalmente, el documento muestra cómo guardar los pesos de un modelo:

```python
torch.save(
    model_scratch.state_dict(),
    "models/cnn_scratch.pth"
)
```

Esto permite conservar el modelo entrenado y utilizarlo posteriormente sin tener que entrenarlo nuevamente desde cero.

---

# 9. 🛠️ Clasificación binaria con Keras

**Keras** es una biblioteca que permite construir y entrenar redes neuronales de una manera más sencilla.

A diferencia de la CNN anterior, en esta parte se trabaja con un problema de **clasificación de reseñas de películas**.

El objetivo es determinar si una reseña es:

```text
0 → Negativa
1 → Positiva
```

Para esto se utiliza el dataset **IMDB** incluido en Keras:

```python
from keras.datasets import imdb
from keras import models, layers, optimizers
```

---

## 9.1 📚 Preparación de los datos

Se descargan los datos mediante:

```python
(train_data, train_labels), (test_data, test_labels) = \
    imdb.load_data(num_words=10000, index_from=3)
```

Las reseñas están representadas mediante números, donde cada número corresponde a una palabra.

Después se obtiene el diccionario:

```python
word_index = imdb.get_word_index()
```

---

## 9.2 🔢 One-Hot Encoding

Las secuencias de números deben transformarse en vectores que puedan ser utilizados por la red neuronal.

Para esto se crea una función:

```python
def vectorizar(sequences, dim=10000):
    ...
```

Después se utiliza:

```python
x_train = vectorizar(train_data)
x_test = vectorizar(test_data)
```

Esto convierte los datos en una representación adecuada para el modelo.

---

# 10. 🧠 Creación del modelo con Keras

El modelo se construye utilizando `Sequential`:

```python
model = models.Sequential()

model.add(
    layers.Dense(
        16,
        activation='relu',
        input_shape=(10000,)
    )
)
```

También se agregan capas adicionales y una capa de salida.

La idea general del modelo es:

```text
Datos de entrada
      ↓
Capa de 16 neuronas
      ↓
Capa de 16 neuronas
      ↓
Capa de salida
      ↓
Positiva / Negativa
```

Después se configura el modelo:

```python
model.compile(
    optimizer='rmsprop',
    loss='binary_crossentropy',
    metrics=['accuracy']
)
```

Aquí se define:

- `optimizer`: método utilizado para ajustar los pesos.
- `loss`: función utilizada para medir el error.
- `accuracy`: métrica utilizada para medir la exactitud.

---

# 11. 📈 Entrenamiento y sobreajuste

El modelo se entrena utilizando:

```python
model.fit(
    partial_x_train,
    partial_y_train,
    epochs=20,
    ...
)
```

Durante el entrenamiento se comparan las pérdidas de entrenamiento y validación.

Esto permite observar si aparece **sobreajuste (overfitting)**.

El sobreajuste ocurre cuando el modelo aprende demasiado bien los datos de entrenamiento, pero pierde capacidad para generalizar con datos que no ha visto.

En el documento, el modelo alcanza aproximadamente:

- **Error:** 0.6
- **Exactitud:** 86.1 %

---

### 📌 Espacio para imagen

> **[Colocar aquí las curvas de pérdida de entrenamiento y validación]**

---

# 12. 🛡️ Regularización y Dropout

Para intentar reducir el sobreajuste se presentan dos técnicas.

### Regularización

La regularización modifica el entrenamiento para evitar que el modelo dependa demasiado de determinados pesos.

### Dropout

El Dropout desactiva aleatoriamente una parte de las neuronas durante el entrenamiento.

En el documento se utiliza:

```python
layers.Dropout(...)
```

La idea es obligar al modelo a aprender características más generales.

---

# 13. 🔮 Predicciones

Una vez entrenado el modelo, se pueden realizar predicciones utilizando:

```python
predictions = model.predict(x_test)
```

Por ejemplo, en el documento una predicción obtiene aproximadamente un **99.4 %** para una reseña positiva.

Esto representa la confianza que el modelo asigna a esa clasificación.

---

# 14. ⚪ Perceptrón

El **Perceptrón** es un modelo sencillo de inteligencia artificial inspirado en una neurona.

Recibe datos de entrada, los combina utilizando **pesos** y un **bias**, y posteriormente utiliza una función de activación para obtener una salida.

Su funcionamiento puede representarse como:

```text
Entradas
   ↓
Pesos + Bias
   ↓
Suma ponderada
   ↓
Función de activación
   ↓
Salida
```

Una forma simplificada de representarlo es:

\[
y = f(w_1x_1 + w_2x_2 + b)
\]

Donde:

- `x` → entradas.
- `w` → pesos.
- `b` → bias.
- `f` → función de activación.
- `y` → salida.

---

# 15. ⚙️ Funciones de activación

En el documento se utiliza una función escalón:

```python
def step_function(x):
    ...
```

Esta función convierte el resultado en una salida de **0 o 1**.

También se experimenta con otras funciones de activación, como `tanh`.

La función de activación es importante porque transforma la suma ponderada en la salida que utilizará la neurona para tomar una decisión.

---

# 16. 🏭 Ejemplo del Perceptrón

Se utiliza un ejemplo relacionado con el **sobrecalentamiento de un equipo industrial**.

Se consideran factores como:

```python
temperatura = 100
vibracion = 50
```

Luego se definen pesos y bias para realizar la predicción.

El perceptrón combina estos valores y obtiene una salida dependiendo de la función de activación utilizada.

---

### 📌 Espacio para imagen

> **[Colocar aquí una captura del ejemplo del equipo industrial y sus resultados]**

---

# 17. 🔢 Perceptrón AND, OR y XOR

También se prueba el funcionamiento del perceptrón mediante compuertas lógicas.

## AND

El perceptrón produce `1` solamente cuando **las dos entradas son 1**.

Por ejemplo:

```text
0 AND 0 → 0
0 AND 1 → 0
1 AND 0 → 0
1 AND 1 → 1
```

En el documento se utiliza una configuración con:

```python
weights = [0.4, 0.4]
bias = -0.5
```

---

## OR

En este caso, el perceptrón produce `1` cuando **al menos una entrada es 1**.

```text
0 OR 0 → 0
0 OR 1 → 1
1 OR 0 → 1
1 OR 1 → 1
```

Se utiliza, por ejemplo:

```python
weights_3 = np.array([2, 1])
bias_3 = -0.5
```

---

## XOR

La compuerta XOR produce `1` cuando las entradas son diferentes:

```text
0 XOR 0 → 0
0 XOR 1 → 1
1 XOR 0 → 1
1 XOR 1 → 0
```

El documento muestra gráficamente las fronteras de decisión para observar por qué XOR presenta una dificultad mayor para un único perceptrón.

---

### 📌 Espacio para imagen

> **[Colocar aquí la gráfica de las fronteras de decisión AND, OR y XOR]**

---

# 18. 🔗 Relación entre CNN, Keras y Perceptrón

Los tres conceptos están relacionados con las redes neuronales, pero **no representan exactamente lo mismo**.

| Concepto | ¿Qué es? | Principal aplicación |
|---|---|---|
| 🧠 **Perceptrón** | Modelo básico de neurona artificial | Clasificaciones sencillas |
| 🖼️ **CNN** | Tipo de red neuronal | Principalmente imágenes |
| 🛠️ **Keras** | Biblioteca para crear redes neuronales | Construcción y entrenamiento de modelos |

Una forma sencilla de entenderlo sería:

```text
Perceptrón
   ↓
Unidad básica de una red neuronal

CNN
   ↓
Red neuronal especializada en imágenes

Keras
   ↓
Herramienta para construir y entrenar redes neuronales
```

Además, el documento utiliza **PyTorch** para la parte de CNN y **Keras** para el ejemplo de clasificación de reseñas.

---

# 19. 💭 ¿Qué entendí?

A partir de la actividad entendí que las redes neuronales pueden utilizarse para aprender patrones a partir de datos y posteriormente realizar predicciones.

El **Perceptrón** permite comprender de manera sencilla cómo una neurona artificial utiliza entradas, pesos, bias y una función de activación para producir una salida.

Las **CNN** son más especializadas y permiten trabajar con imágenes, aprendiendo características como bordes, formas y patrones.

Por otro lado, **Keras** facilita la creación y entrenamiento de redes neuronales sin tener que implementar todos los procesos matemáticos desde cero.

También entendí que no basta con entrenar un modelo: es necesario evaluar sus resultados y revisar problemas como el **sobreajuste**. Técnicas como Data Augmentation, Transfer Learning, regularización y Dropout pueden ayudar dependiendo del problema.

---

# 20. 💧 Aplicación a nuestro proyecto

Nuestro proyecto consiste en desarrollar un sistema automatizado para la **clarificación de agua mediante dosificación de quitosano**.

El sistema utilizará sensores de:

- **pH**
- **Turbidez**

Los datos obtenidos por estos sensores podrían utilizarse como entradas para un modelo que ayude a analizar las condiciones del agua.

### Posibles aplicaciones

**Perceptrón:**

Podría utilizarse para realizar una clasificación sencilla a partir de los valores obtenidos por los sensores.

```text
pH + Turbidez
      ↓
 Perceptrón
      ↓
Clasificación / decisión
```

**CNN:**

Su principal ventaja está relacionada con el procesamiento de imágenes, por lo que su aplicación tendría más sentido si nuestro proyecto incorporara imágenes como parte de la información analizada.

**Keras:**

No sería un modelo en sí mismo, sino una herramienta que podría utilizarse para construir y entrenar un modelo de red neuronal utilizando los datos de nuestros sensores.

---

## ❓ ¿Cuál utilizaríamos en nuestro proyecto?

Antes de completar esta última parte, **¿cuál de los tres consideras que sería más útil o aplicable para nuestro proyecto de clarificación de agua: Perceptrón, CNN o Keras?**

Dime cuál elegirías y luego completamos esta sección final justificando **por qué encaja con los sensores de pH y turbidez**.