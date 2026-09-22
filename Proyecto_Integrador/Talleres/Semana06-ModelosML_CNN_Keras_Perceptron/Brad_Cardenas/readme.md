#  Redes Neuronales

---

## CNN — Redes Neuronales Convolucionales

Permiten el análisis de imágenes mediante los píxeles cercanos. Usan pequeños filtros llamados *kernels* que recorren la imagen buscando patrones.

### Componentes clave

- **Convolución (Conv2D)**: aplica filtros para obtener mapas de características. Aprende bordes, texturas y estructuras más complejas en capas profundas.
- **Activación (ReLU)**: introduce no linealidad. `ReLU(x) = max(0, x)`. Si el valor es negativo → 0. Si es positivo → lo deja igual.
- **Pooling (MaxPool)**: reduce la resolución espacial, mantiene la información importante. Ayuda a generalizar y reduce el costo computacional.

### Dataset: TrashNet

Clasificación binaria de residuos (vidrio vs. plástico) con imágenes en escala de grises, divididas en train/validation/test.

```python
DataClass = TrashDataset
DataClass.classes
```

### Visualizar ejemplos del dataset

```python
def show_glass_plastic(dataset):
    glass = []
    plastic = []
    for i in range(len(dataset)):
        x, y = dataset[i]
        label = int(y.item())
        if label == 0 and len(glass) < 6:
            glass.append((x, label))
        elif label == 1 and len(plastic) < 6:
            plastic.append((x, label))
        if len(glass) == 6 and len(plastic) == 6:
            break
    # ... se grafican las imágenes
```

![Plastic_Vidrio](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana06-ModelosML_CNN_Keras_Perceptron/Capturas/plasticoVidrio.png)

### Modelo 1: CNN desde cero

```python
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
        )
```

Se evalúa con:
- **Accuracy**: porcentaje de aciertos.
- **ROC-AUC**: qué tan bien separa las clases.
- **Matriz de confusión** y **Precision/Recall/F1**: útiles cuando hay desbalance.

### Curvas de entrenamiento

```python
plt.plot(history_scratch["train_loss"])
plt.title("Pérdida de entrenamiento (CNN desde cero)")
plt.show()

plt.plot(history_scratch["val_acc"], label="Accuracy")
plt.plot(history_scratch["val_auc"], label="ROC-AUC")
plt.legend()
plt.show()
```

![Pérdida de entrenamiento](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana06-ModelosML_CNN_Keras_Perceptron/Capturas/perdidaEntrenamiento.png)
![Métrica Validacion](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana06-ModelosML_CNN_Keras_Perceptron/Capturas/MetricaValidacion.png)

- A partir de la época 4 mejora la exactitud hasta 63.27%.
- El ROC-AUC se mantiene entre 0.67 y 0.69: hay aprendizaje, pero moderado.
- La pérdida va disminuyendo.

### Matriz de confusión

```python
plt.imshow(cm)
plt.title("Matriz de confusión (CNN desde cero)")
plt.xlabel("Predicción")
plt.ylabel("Real")
plt.colorbar()
plt.show()
```

![MatrizConfusion](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana06-ModelosML_CNN_Keras_Perceptron/Capturas/MatrizConfusion.png)

### Data Augmentation

Ayuda a generalizar, pero debe aplicarse con cuidado (rotaciones pequeñas y traslaciones suaves; inversiones no siempre tienen sentido).

```python
transform_aug = T.Compose([
    T.RandomRotation(degrees=10),
    T.RandomAffine(degrees=0, translate=(0.05, 0.05)),
    T.ToTensor()
])
```

### Transfer Learning

Reutilizar un modelo (ResNet18) ya entrenado en un dataset grande. Ventaja: suele mejorar el rendimiento con pocos datos.

```python
resnet = models.resnet18(weights=models.ResNet18_Weights.DEFAULT)
resnet.fc = nn.Linear(resnet.fc.in_features, num_classes)

for param in resnet.parameters():
    param.requires_grad = False
for param in resnet.fc.parameters():
    param.requires_grad = True
```

**Fine-tuning**: luego se descongelan las últimas capas (`layer4` y `fc`) para ajustarlas mejor al dataset.

### Comparación global

```python
print(f"CNN desde cero (sin aug)  | acc={test_acc:.4f} | auc={test_auc:.4f}")
print(f"CNN desde cero (con aug)  | acc={test_acc_aug:.4f} | auc={test_auc_aug:.4f}")
print(f"Transfer learning (ResNet) | acc={test_acc_tl:.4f} | auc={test_auc_tl:.4f}")
```

![tresModelos](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana06-ModelosML_CNN_Keras_Perceptron/Capturas/global.jpg)

### Grad-CAM (Interpretabilidad)

Mapa de calor sobre la imagen que muestra qué zonas influyeron más en la predicción.

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

![GRAM](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana06-ModelosML_CNN_Keras_Perceptron/Capturas/Gram.png)

- Zonas claras/amarillas: mayor importancia para la predicción.
- Zonas oscuras/moradas: menor contribución.

### Guardar y cargar modelos

```python
torch.save(model_scratch.state_dict(), "models/cnn_scratch.pth")
torch.save(model_aug.state_dict(), "models/cnn_aug.pth")
torch.save(resnet.state_dict(), "models/resnet_transfer.pth")
```

Guarda los modelos para poder recuperarlos después sin tener que reentrenarlos.

**Qué se aprendió:** cómo armar una CNN desde cero, cómo mejorarla con data augmentation, cómo aprovechar un modelo preentrenado (transfer learning) para tener mejores resultados con menos datos, y cómo interpretar qué está mirando el modelo con Grad-CAM.

---

##  Clasificación binaria con Keras

Con Keras no hay que programar todo desde cero, permite construir y entrenar redes de forma más sencilla. Ejemplo: clasificar reseñas de películas (IMDB) en positivas o negativas.

```python
from keras.datasets import imdb
from keras import models, layers, optimizers

(train_data, train_labels), (test_data, test_labels) = imdb.load_data(num_words=10000, index_from=3)
```

### Vectorización (one-hot encoding)

```python
def vectorizar(sequences, dim=10000):
    restults = np.zeros((len(sequences), dim))
    for i, sequences in enumerate(sequences):
        restults[i, sequences] = 1
    return restults
```

Convierte cada reseña en un vector de 0 y 1: 0 = palabra ausente, 1 = palabra presente.

### Creación del modelo

```python
model = models.Sequential()
model.add(layers.Dense(16, activation='relu', input_shape=(10000,)))
model.add(layers.Dense(16, activation='relu'))
model.add(layers.Dense(1, activation='sigmoid'))

model.compile(optimizer='rmsprop',
              loss='binary_crossentropy',
              metrics=['accuracy'])
```

2 capas ocultas de 16 neuronas y 1 capa de salida con 1 neurona.

### Análisis del resultado

```python
plt.plot(epoch, loss_values, '.-', label='training')
plt.plot(epoch, val_loss_values, '--', label='val')
plt.legend()
plt.show()
```

![entrenamiento vs. validación](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana06-ModelosML_CNN_Keras_Perceptron/Capturas/Keras.png)

- Azul: error del modelo en entrenamiento.
- Anaranjado: error sobre datos de validación.
- La curva anaranjada no cae al final → **sobreajuste**: la red aprende demasiado bien los datos de entrenamiento y pierde capacidad de generalización.

Resultado en test: nivel de error 0.6, exactitud 86.1%.

### Comparando con un modelo más pequeño

```python
model2 = models.Sequential()
model2.add(layers.Dense(4, activation='relu', input_shape=(10000,)))
model2.add(layers.Dense(1, activation='sigmoid'))
```

![modelo pequeño vs. el original](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana06-ModelosML_CNN_Keras_Perceptron/Capturas/modelopeque%C3%B1o.png)

- Con la muestra original hubo sobreajuste.
- Con el modelo más chico, el mínimo de pérdida se mantiene más épocas y el incremento posterior es menor. Sigue habiendo sobreajuste, pero menos pronunciado.

### Regularización

```python
from keras import regularizers
model3.add(layers.Dense(16, activation='relu', kernel_regularizer=regularizers.l2(0.001)))
```

![Aquí va la gráfica con regularización]()

Se ve un pico porque la regularización modifica el aprendizaje y puede dar un error más alto en algunas épocas.

### Dropout

```python
model4.add(layers.Dense(16, activation='relu'))
model4.add(layers.Dropout(0.5))
```

![Aquí va la gráfica con dropout]()

Durante el entrenamiento se apagan aleatoriamente el 50% de las neuronas. Esto obliga a la red a aprender de diferentes combinaciones de neuronas, para reducir el sobreajuste.

### Predicciones

```python
predictions = model.predict(x_test)
```

La predicción del índice 10 dio 99.4%: reseña positiva con un 99.4% de probabilidad.

**Qué se aprendió:** cómo armar una red densa simple con Keras y, sobre todo, cómo detectar y reducir el sobreajuste con distintas técnicas (modelo más chico, regularización, dropout).

---

##  Perceptrón

Modelo sencillo de IA que recibe datos, los combina con pesos y produce una salida (predicción).

```python
def step_function(x):
    return 1 if x >= 0 else 0

def tanh_activation(x):
    return np.tanh(x)

def perceptron(inputs, weights, bias, activation_func):
    weighted_sum = np.dot(inputs, weights) + bias
    return activation_func(weighted_sum)
```

### Ejemplo: sobrecalentamiento de un equipo industrial

```python
temperatura = 100
vibracion = 50

weights = np.array([0.5, -0.5])
bias = -30

inputs = np.array([temperatura, vibracion])

output_step = perceptron(inputs, weights, bias, step_function)
output_tanh = perceptron(inputs, weights, bias, tanh_activation)
```

![Aquí va la salida impresa con los resultados de output_step y output_tanh]()

- La suma ponderada es negativa → la función escalón da 0.
- La función tanh transforma cualquier valor entre -1 y 1.
- La función de activación transforma la suma ponderada en la salida que usa la neurona para decidir.

### Perceptrón tipo AND, OR y XOR

```python
def test_perceptron(inputs, weights, bias, activation_func):
    for p, q in [(0, 0), (0, 1), (1, 0), (1, 1)]:
        input_data = np.array([p, q])
        prediction = perceptron(input_data, weights, bias, activation_func)
        print(f'Entradas: ({p}, {q}), Predicción: {prediction}')
```

- **AND** (pesos [0.4, 0.4], bias -0.5): da 1 solo cuando ambas entradas son 1.
- **OR** (pesos [2, 1], bias -0.5): da 1 cuando al menos una entrada es 1.
- **XOR**: da 1 cuando las entradas son diferentes.

![Aquí va el gráfico del plano cartesiano con las 4 combinaciones]()
![Aquí va el gráfico con las fronteras de decisión de OR y AND]()
![Aquí va el gráfico con los círculos donde XOR = 0]()

- OR separa (0,0) del resto.
- AND separa (1,1) del resto.
- 1 perceptrón no puede resolver XOR. Se necesitan 2 perceptrones + una capa de salida.

**Qué se aprendió:** el perceptrón es la unidad más básica de una red neuronal, pero solo puede resolver problemas donde una línea recta separa las clases (como AND y OR). XOR no es así, y por eso se necesita más de una capa — esta es la razón por la que existen las redes multicapa.

---

## 🔧 Cómo se aplicaría esto al proyecto de clarificación de agua con quitosano

El proyecto busca automatizar la dosificación de quitosano y el proceso de clarificación de agua usando un ESP32, sensores de turbidez y pH. Estas son las ideas del cuadernillo que se conectan directamente:

### 1. El perceptrón para decidir la dosis de quitosano

Es la misma lógica que el ejemplo de sobrecalentamiento: en vez de temperatura y vibración, las entradas serían turbidez y pH, y la salida sería la dosis de quitosano a aplicar.

```python
inputs = np.array([turbidez, ph])
dosis = perceptron(inputs, weights, bias, tanh_activation)
```

La diferencia es que en el proyecto real los pesos no se ponen a mano: se ajustan (se "entrenan") con datos de pruebas de laboratorio, igual que se entrenó el modelo de Keras con las reseñas de IMDB.

### 2. Un modelo simple tipo Keras para aprender la dosis correcta

Con datos de pruebas (turbidez inicial, pH inicial, y la dosis que dio buen resultado), se podría armar una red chica parecida a la del ejemplo de IMDB, pero mucho más simple:

```python
model = models.Sequential()
model.add(layers.Dense(8, activation='relu', input_shape=(2,)))  # turbidez y pH
model.add(layers.Dense(1, activation='linear'))  # dosis recomendada
model.compile(optimizer='adam', loss='mse')
```

### 3. Detectar cuándo el agua ya está clarificada

Similar a la clasificación binaria positiva/negativa de las reseñas, se podría clasificar el estado del agua como "clarificada" o "aún turbia" según las lecturas del sensor de turbidez, para que el ESP32 sepa cuándo parar el proceso en vez de usar un tiempo fijo.

### 4. Por qué no haría falta CNN

Las CNN sirven para analizar imágenes. Si el proyecto solo usa sensores de turbidez y pH (no una cámara), no se necesita CNN. Solo tendría sentido si en algún momento se agrega una cámara para observar visualmente el agua.

### 5. Limitación a tener en cuenta

Un modelo entrenado con Keras no corre directamente en un ESP32. Para eso existe **TensorFlow Lite Micro**, que permite convertir el modelo entrenado a un formato liviano que sí puede cargarse en el microcontrolador. Como el modelo sería muy chico (pocas entradas, pocas neuronas), esto es viable.

**En resumen:** el perceptrón y el modelo de Keras del cuadernillo son la base conceptual para automatizar la decisión de "cuánto quitosano dosificar" y "cuándo detener el proceso", reemplazando reglas fijas por un modelo que aprende de los propios datos del proyecto.
