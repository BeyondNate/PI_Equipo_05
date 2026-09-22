# 🧠 Redes Neuronales: CNN, Keras y Perceptrón

## 1. Introducción

En esta actividad se trabajaron tres conceptos relacionados con las redes neuronales: **CNN, Keras y Perceptrón**. Se realizaron ejemplos de clasificación de imágenes, clasificación de texto y problemas sencillos de clasificación.

---

## 2. 🖼️ CNN

Una **CNN (Convolutional Neural Network)** es un tipo de red neuronal utilizada principalmente para trabajar con imágenes. Su función es aprender características de las imágenes, como bordes, formas y patrones, para después poder clasificarlas.

De forma sencilla:

```text
Imagen → Características → Patrones → Clasificación
```

En el notebook se trabaja con imágenes de **TrashNet**, utilizando **PyTorch**.

### Códigos importantes

Para seleccionar si se utilizará GPU o CPU:

```python
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
```

Para convertir las imágenes a tensores:

```python
transform_basic = T.Compose([
    T.ToTensor()
])
```

También se crea una CNN mediante una clase:

```python
class SimpleCNN(nn.Module):
    ...
```

La red utiliza capas de convolución, activación y pooling.

**[Insertar aquí una imagen del notebook donde se muestran las imágenes de TrashNet]**

**[Insertar aquí la gráfica o matriz de confusión generada en el notebook]**

---

## 3. 🔄 Data Augmentation y Transfer Learning

En el notebook también se prueban técnicas para mejorar el entrenamiento.

El **Data Augmentation** modifica ligeramente las imágenes para generar mayor variedad en los datos. Por ejemplo:

```python
T.RandomRotation(degrees=10)
```

También se utiliza **Transfer Learning**, aprovechando un modelo que ya fue entrenado previamente:

```python
resnet = models.resnet18(
    weights=models.ResNet18_Weights.DEFAULT
)
```

Esto permite adaptar un modelo existente a nuestro problema en lugar de entrenarlo completamente desde cero.

**[Insertar aquí el resultado generado en el notebook relacionado con Transfer Learning]**

---

## 4. 🛠️ Keras

**Keras** es una herramienta que facilita la creación y entrenamiento de redes neuronales.

En el notebook se utiliza para clasificar reseñas de películas del dataset **IMDB**, determinando si una reseña es positiva o negativa.

Primero se cargan los datos:

```python
(train_data, train_labels), (test_data, test_labels) = \
    imdb.load_data(num_words=10000, index_from=3)
```

Después se crea un modelo utilizando `Sequential` y capas `Dense`:

```python
model = models.Sequential()
model.add(layers.Dense(16, activation='relu'))
```

Finalmente, el modelo se configura con:

```python
model.compile(
    optimizer='rmsprop',
    loss='binary_crossentropy',
    metrics=['accuracy']
)
```

Aquí se define cómo se entrenará el modelo y cómo se medirá su desempeño.

También se utilizó **Dropout** para ayudar a reducir el sobreajuste:

```python
layers.Dropout(...)
```

**[Insertar aquí la gráfica de entrenamiento/validación generada en el notebook]**

---

## 5. ⚪ Perceptrón

El **Perceptrón** es uno de los modelos más básicos de una red neuronal. Recibe entradas, las combina utilizando **pesos y bias**, y genera una salida mediante una función de activación.

```text
Entradas → Pesos + Bias → Función de activación → Salida
```

En el notebook se utiliza una función escalón:

```python
def step_function(x):
    ...
```

También se prueba el perceptrón con ejemplos de compuertas lógicas como **AND, OR y XOR**.

Por ejemplo, para AND:

```text
0 AND 0 → 0
0 AND 1 → 0
1 AND 0 → 0
1 AND 1 → 1
```

Estos ejemplos permiten entender cómo el perceptrón puede separar diferentes clases mediante una frontera de decisión.

**[Insertar aquí la gráfica AND, OR y XOR generada en el notebook]**

---

## 6. 🔗 ¿Qué relación tienen?

Los tres conceptos están relacionados, pero cumplen funciones diferentes:

| Concepto      | ¿Qué es?                     | Uso principal                        |
| ------------- | ---------------------------- | ------------------------------------ |
| 🧠 Perceptrón | Modelo básico de neurona     | Clasificaciones sencillas            |
| 🖼️ CNN       | Tipo de red neuronal         | Principalmente imágenes              |
| 🛠️ Keras     | Herramienta para crear redes | Entrenar modelos de redes neuronales |

En resumen, el **Perceptrón** ayuda a entender la base de las redes neuronales, una **CNN** permite trabajar con problemas más complejos como imágenes y **Keras** facilita la creación y entrenamiento de estos modelos.

---

## 7. 💭 ¿Qué entendí?

Entendí que una red neuronal aprende a partir de ejemplos para poder realizar predicciones. El perceptrón representa una forma básica de este funcionamiento, utilizando entradas, pesos y una función de activación.

También entendí que las CNN están más orientadas al procesamiento de imágenes y que Keras facilita la creación y entrenamiento de modelos de redes neuronales.

---

## 8. 💧 Aplicación a nuestro proyecto

Nuestro proyecto busca automatizar la **clarificación de agua mediante quitosano**, utilizando sensores de **pH y turbidez**.

De los tres conceptos, considero que el **Perceptrón sería el más aplicable** como una primera aproximación, ya que nuestros datos principales serían valores numéricos obtenidos de los sensores.

Podríamos utilizar como entradas:

```text
pH + Turbidez
      ↓
 Perceptrón
      ↓
 Clasificación / decisión
```

Por ejemplo, el modelo podría aprender a clasificar determinadas condiciones del agua a partir de los valores de pH y turbidez.

Una CNN tendría más sentido si nuestro proyecto trabajara directamente con **imágenes**, mientras que Keras podría ser utilizado como herramienta para implementar el modelo, pero no sería el modelo en sí.
