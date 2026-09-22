#  Redes Neuronales

Resumen del trabajo práctico sobre redes neuronales, dividido en tres bloques: **CNN (PyTorch)**, **Clasificación binaria con Keras** y **Perceptrón**.

---

##  CNN — Convolutional Neural Networks

Las CNN permiten el análisis de imágenes a partir de los píxeles cercanos entre sí, usando pequeños filtros (*kernels*) que recorren la imagen buscando patrones (bordes, texturas, formas).

### Componentes clave

- **Convolución (`Conv2D`)**: aplica filtros para generar mapas de características. En capas profundas aprende estructuras cada vez más complejas.
- **Activación (`ReLU`)**: introduce no linealidad. `ReLU(x) = max(0, x)` — si el valor es negativo se convierte en 0, si es positivo se mantiene igual.
- **Pooling (`MaxPool`)**: reduce la resolución espacial conservando la información importante, ayudando a generalizar y a reducir el costo computacional.

![Espacio para imagen: diagrama de una CNN (conv + relu + pooling)]()

### Dataset: TrashNet

Clasificación binaria de residuos (vidrio vs. plástico) a partir de imágenes en escala de grises, dividido en *train/validation/test*.

```python
import numpy as np
import torch
from torch.utils.data import DataLoader
import torchvision.transforms as T
from trash_dataset import TrashDataset

DataClass = TrashDataset
```

**Por qué es importante:** separar los datos en particiones evita que el modelo se evalúe con datos que ya vio en entrenamiento, dando una medida realista de qué tan bien generaliza.

### Modelo 1: CNN desde cero

```python
import torch.nn as nn

class SimpleCNN(nn.Module):
    def __init__(self, num_classes):
        super().__init__()
        self.features = nn.Sequential(
            nn.Conv2d(1, 16, kernel_size=3, padding=1),
            nn.ReLU(),
            nn.MaxPool2d(2),

            nn.Conv2d(16, 32, kernel_size=3, padding=1),
            nn.ReLU(),
            nn.MaxPool2d(2),

            nn.Conv2d(32, 64, kernel_size=3, padding=1),
            # ...
        )
```

**Qué se aprendió:** cómo apilar bloques de convolución + activación + pooling para construir un extractor de características desde cero, y cómo medir su desempeño con:

- **Accuracy**: porcentaje de aciertos.
- **ROC-AUC**: capacidad de separar clases.
- **Matriz de confusión** y **Precision/Recall/F1**: útiles cuando hay desbalance entre clases.

Resultado: el modelo mejora a partir de la época 4, alcanzando ~63% de accuracy y un ROC-AUC entre 0.67-0.69. Aprendizaje moderado, con la pérdida disminuyendo progresivamente.

![Espacio para imagen: curvas de pérdida y accuracy del modelo desde cero]()
![Espacio para imagen: matriz de confusión del modelo desde cero]()

### Data Augmentation

```python
transform_aug = T.Compose([
    T.RandomRotation(degrees=10),
    T.RandomAffine(degrees=0, translate=(0.05, 0.05)),
    T.ToTensor()
])
```

**Por qué es importante:** aumentar artificialmente la variedad de datos de entrenamiento (rotaciones y traslaciones leves) ayuda al modelo a generalizar mejor, evitando que memorice ejemplos específicos. Hay que aplicarlo con cuidado: inversiones horizontales/verticales pueden no tener sentido según el dominio del problema.

### Transfer Learning

Reutilizar un modelo (ResNet18) preentrenado en un dataset grande (ImageNet) en lugar de entrenar todo desde cero.

```python
import torchvision.models as models

resnet = models.resnet18(weights=models.ResNet18_Weights.DEFAULT)
in_features = resnet.fc.in_features
resnet.fc = nn.Linear(in_features, num_classes)

# Congelar todas las capas, entrenar solo la última
for param in resnet.parameters():
    param.requires_grad = False
for param in resnet.fc.parameters():
    param.requires_grad = True
```

**Fine-tuning**: luego se descongelan las últimas capas (`layer4` y `fc`) para ajustarlas al nuevo dataset con una tasa de aprendizaje más baja.

**Por qué es importante:** el transfer learning suele mejorar mucho el rendimiento cuando se cuenta con pocos datos, ya que aprovecha patrones visuales genéricos (bordes, texturas) ya aprendidos por el modelo base.

### Comparación de resultados

| Modelo | Accuracy | ROC-AUC |
|---|---|---|
| CNN desde cero (sin aug) | ver output | ver output |
| CNN desde cero (con aug) | ver output | ver output |
| Transfer Learning (ResNet) | ver output | ver output |

### Grad-CAM (Interpretabilidad)

Genera un mapa de calor que indica qué regiones de la imagen influyeron más en la predicción del modelo.

```python
def grad_cam(model, image_tensor, target_class=None):
    model.eval()
    activations = {}
    gradients = {}

    def forward_hook(module, inp, out):
        activations["value"] = out

    def backward_hook(module, grad_in, grad_out):
        gradients["value"] = grad_out[0]

    handle_fwd = model.layer4.register_forward_hook(forward_hook)
    handle_bwd = model.layer4.register_full_backward_hook(backward_hook)
    # ...
```

**Por qué es importante:** las CNN suelen funcionar como "cajas negras". Grad-CAM permite entender e interpretar en qué zonas de la imagen se está fijando el modelo para tomar su decisión (zonas claras/amarillas = mayor importancia; oscuras/moradas = menor contribución).

![Espacio para imagen: imagen original, mapa Grad-CAM y superposición]()

### Guardar y cargar modelos

```python
torch.save(model_scratch.state_dict(), "models/cnn_scratch.pth")
torch.save(model_aug.state_dict(), "models/cnn_aug.pth")
torch.save(resnet.state_dict(), "models/resnet_transfer.pth")
```

**Por qué es importante:** permite reutilizar un modelo ya entrenado sin necesidad de reentrenarlo desde cero cada vez.

---

##  Clasificación binaria con Keras

Ejemplo clásico: clasificar reseñas de películas de IMDB en positivas o negativas, usando Keras en lugar de programar todo manualmente.

```python
from keras.datasets import imdb
from keras import models, layers, optimizers

(train_data, train_labels), (test_data, test_labels) = imdb.load_data(
    num_words=10000, index_from=3
)
```

### Vectorización (one-hot encoding)

```python
def vectorizar(sequences, dim=10000):
    resultados = np.zeros((len(sequences), dim))
    for i, sequence in enumerate(sequences):
        resultados[i, sequence] = 1
    return resultados
```

**Por qué es importante:** las redes neuronales necesitan entradas numéricas de tamaño fijo. Esta función convierte cada reseña (una secuencia de palabras) en un vector de 10000 posiciones donde 1 indica que la palabra está presente y 0 que está ausente.

### Construcción del modelo

```python
model = models.Sequential()
model.add(layers.Dense(16, activation='relu', input_shape=(10000,)))
model.add(layers.Dense(16, activation='relu'))
model.add(layers.Dense(1, activation='sigmoid'))

model.compile(optimizer='rmsprop',
              loss='binary_crossentropy',
              metrics=['accuracy'])
```

- Dos capas ocultas de 16 neuronas con activación ReLU.
- Una capa de salida con 1 neurona y activación **sigmoid** (produce una probabilidad entre 0 y 1, ideal para clasificación binaria).

### Entrenamiento y sobreajuste

```python
history = model.fit(partial_x_train, partial_y_train,
                     epochs=20, batch_size=512,
                     validation_data=(x_val, y_val))
```

![Espacio para imagen: curva de pérdida entrenamiento vs. validación]()

**Qué se aprendió:** la curva de validación deja de mejorar mientras la de entrenamiento sigue bajando — esto es **sobreajuste (overfitting)**: la red memoriza los datos de entrenamiento y pierde capacidad de generalizar a datos nuevos.

### Técnicas para reducir el sobreajuste

1. **Modelo más pequeño** (menos neuronas → menos capacidad de memorizar):
```python
model2 = models.Sequential()
model2.add(layers.Dense(4, activation='relu', input_shape=(10000,)))
model2.add(layers.Dense(1, activation='sigmoid'))
```

2. **Regularización L2** (penaliza pesos grandes):
```python
from keras import regularizers
model3.add(layers.Dense(16, activation='relu',
                         kernel_regularizer=regularizers.l2(0.001)))
```

3. **Dropout** (apaga aleatoriamente neuronas durante el entrenamiento):
```python
model4.add(layers.Dense(16, activation='relu'))
model4.add(layers.Dropout(0.5))
```

**Por qué es importante:** cada técnica obliga a la red a no depender demasiado de patrones específicos del set de entrenamiento, mejorando su capacidad de generalización sobre datos nuevos.

![Espacio para imagen: comparación de curvas — original vs. regularización vs. dropout]()

### Predicciones

```python
predictions = model.predict(x_test)
```

---

## 🚩 Perceptrón

El bloque más simple de una red neuronal: recibe datos, los combina mediante pesos y un sesgo (*bias*), y produce una salida.

```python
import numpy as np

def step_function(x):
    return 1 if x >= 0 else 0

def tanh_activation(x):
    return np.tanh(x)

def perceptron(inputs, weights, bias, activation_func):
    weighted_sum = np.dot(inputs, weights) + bias
    return activation_func(weighted_sum)
```

**Por qué es importante:** la función de activación transforma la suma ponderada en la salida que la neurona usa para "decidir". La función escalón da una salida binaria (0 o 1); `tanh` da un valor continuo entre -1 y 1, aportando más matices.

### Ejemplo: detección de sobrecalentamiento

```python
temperatura = 100
vibracion = 50

weights = np.array([0.5, -0.5])
bias = -30

inputs = np.array([temperatura, vibracion])

output_step = perceptron(inputs, weights, bias, step_function)
output_tanh = perceptron(inputs, weights, bias, tanh_activation)
```

### Compuertas lógicas: AND, OR y XOR

```python
def test_perceptron(inputs, weights, bias, activation_func):
    for p, q in [(0, 0), (0, 1), (1, 0), (1, 1)]:
        input_data = np.array([p, q])
        prediction = perceptron(input_data, weights, bias, activation_func)
        print(f'Entradas: ({p}, {q}), Predicción: {prediction}')
```

- **AND**: con pesos `[0.4, 0.4]` y bias `-0.5`, da 1 solo cuando ambas entradas son 1.
- **OR**: con pesos `[2, 1]` y bias `-0.5`, da 1 cuando al menos una entrada es 1.
- **XOR**: da 1 cuando las entradas son diferentes.

![Espacio para imagen: plano cartesiano con las 4 combinaciones de entrada]()
![Espacio para imagen: fronteras de decisión de AND y OR]()
![Espacio para imagen: fronteras de decisión intentando resolver XOR]()

**Por qué es importante — la limitación del perceptrón simple:**

- Un solo perceptrón puede resolver problemas **linealmente separables** (AND, OR), donde una sola línea recta separa las clases.
- **XOR no es linealmente separable**: no existe una única línea que separe correctamente las combinaciones donde XOR=1 de las que XOR=0.
- **Solución:** se necesitan **2 perceptrones + una capa de salida** (es decir, una red multicapa) para resolver XOR. Este es el motivo histórico por el cual surgieron las redes neuronales multicapa (MLP): superar la limitación del perceptrón simple ante problemas no lineales.

---

## Conclusiones generales

- Las **CNN** son ideales para datos con estructura espacial (imágenes), combinando convolución, activación y pooling.
- **Transfer learning** ahorra tiempo y mejora resultados cuando hay pocos datos disponibles.
- **Grad-CAM** ayuda a interpretar qué "mira" un modelo de visión al hacer una predicción.
- **Keras** simplifica la construcción de redes densas, pero sigue siendo necesario controlar el **sobreajuste** con regularización, dropout o reduciendo el tamaño del modelo.
- El **perceptrón** es la unidad básica de una red neuronal, pero por sí solo solo resuelve problemas linealmente separables — de ahí la necesidad de redes multicapa.
---
# El uso para el proyecto
## 1. Perceptrón / Red densa pequeña → dosificación de quitosano

Esta es la aplicación más directa y realista para un ESP32.

- **Entradas**: turbidez inicial, pH inicial, quizás volumen de agua o conductividad.
- **Salida**: dosis de quitosano recomendada (regresión) o categoría de dosis (bajo/medio/alto, clasificación).
- Es exactamente el mismo patrón que el ejemplo del perceptrón de "sobrecalentamiento" que vimos: combina variables de sensores con pesos aprendidos y decide una acción.

```python
# Igual que perceptron(inputs, weights, bias, activation_func)
inputs = np.array([turbidez, ph])
dosis = perceptron(inputs, weights, bias, tanh_activation)
```

La diferencia con el proyecto real es que en vez de fijar los pesos a mano, **entrenarías** una red pequeña (Keras, 1-2 capas densas, como el modelo IMDB pero mucho más chico) con datos experimentales: pruebas de laboratorio donde midan turbidez/pH inicial y anoten qué dosis de quitosano dio mejor resultado final.

```python
model = models.Sequential()
model.add(layers.Dense(8, activation='relu', input_shape=(2,)))  # turbidez, pH
model.add(layers.Dense(1, activation='linear'))  # dosis recomendada (regresión)
model.compile(optimizer='adam', loss='mse')
```

**Por qué conviene:** una fórmula fija no captura bien relaciones no lineales entre turbidez/pH y dosis óptima; una red pequeña sí puede aprenderlas a partir de los propios ensayos del proyecto.

## 2. Detección de "punto final" de floculación (clasificación binaria)

Igual que el ejemplo IMDB (positiva/negativa), podrían entrenar un clasificador binario:

- **Entrada**: lecturas de turbidez a lo largo del tiempo (o turbidez + tiempo transcurrido).
- **Salida**: "agua clarificada" (1) vs "aún turbia" (0).

Esto le permite al ESP32 decidir automáticamente cuándo pasar de floculación/sedimentación a la medición final, en vez de usar un tiempo fijo de espera.

## 3. CNN — probablemente no la necesitan

Las CNN tienen sentido si hay **cámara** midiendo turbidez visualmente (imagen del agua). Si el proyecto usa solo un sensor de turbidez óptico/electrónico (lo más común y barato), no hace falta CNN — sería sobrediseño. Solo la mencionaría si en algún momento consideran análisis visual de imágenes del agua.

## 4. Limitación real: correr esto en un ESP32

Aquí está el punto clave a aclarar en el informe: Keras/TensorFlow normal no corre en un ESP32. Lo que se usa es **TensorFlow Lite Micro** (o TinyML):

1. Entrenás el modelo en la compu con Keras (como hicieron en el notebook).
2. Lo convertís a formato `.tflite` cuantizado.
3. Lo cargás al ESP32 con la librería `TensorFlowLite_ESP32` o `EloquentTinyML`.

Dado que es un modelo tan chico (2-3 entradas, un par de capas), esto es totalmente viable en un ESP32.

## 5. Alternativa más simple si el tiempo apremia

Si entrenar una red no es viable por falta de datos experimentales suficientes, puede quedar documentado como **trabajo futuro** y usar mientras tanto una función/tabla de reglas simple (similar al perceptrón con pesos fijados a mano, sin entrenar) — es válido igual y conceptualmente conecta con lo del perceptrón AND/OR que vimos.

¿Quieres que te arme un diagrama del flujo completo (sensores → red neuronal → ESP32 → actuador de dosificación) para meter en el informe, o un ejemplo de código Keras + conversión a TFLite Micro más desarrollado?
