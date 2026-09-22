# Redes Neuronales

## 1. Perceptrón

El perceptrón es el modelo más básico de una neurona artificial. Recibe entradas, las multiplica por pesos, suma un bias y aplica una función de activación.

```python
def perceptron(inputs, weights, bias, activation_func):
    weighted_sum = np.dot(inputs, weights) + bias
    return activation_func(weighted_sum)
```

### Conceptos importantes

* **Inputs:** datos de entrada.
* **Weights:** importancia de cada entrada.
* **Bias:** modifica el punto de decisión.
* **Activation function:** determina la salida.

### Funciones de activación

```python
def step_function(x):
    return 1 if x >= 0 else 0
```

```python
np.tanh(x)
```

### AND, OR y XOR

El perceptrón permite representar problemas como AND y OR.

Sin embargo, un solo perceptrón no puede resolver XOR, lo que demuestra la necesidad de utilizar redes con más de una neurona o capa.

### Imagen

<!-- Colocar aquí imagen de AND, OR y XOR -->

---

# 2. Clasificación con Keras

Se utilizó Keras para construir una red neuronal de clasificación binaria.

### Construcción del modelo

```python
model = models.Sequential()

model.add(layers.Dense(16, activation='relu'))
model.add(layers.Dense(16, activation='relu'))
model.add(layers.Dense(1, activation='sigmoid'))
```

### Funciones importantes

#### `compile()`

```python
model.compile(
    optimizer='rmsprop',
    loss='binary_crossentropy',
    metrics=['accuracy']
)
```

Configura el optimizador, la función de pérdida y las métricas.

#### `fit()`

```python
model.fit(
    x_train,
    y_train,
    epochs=20,
    batch_size=512,
    validation_data=(x_val, y_val)
)
```

Entrena el modelo utilizando los datos de entrenamiento y validación.

#### `evaluate()`

Permite medir el rendimiento del modelo utilizando datos de prueba.

#### `predict()`

```python
predictions = model.predict(x_test)
```

Genera predicciones para nuevos datos.

### Conceptos importantes

* `Dense`: capa completamente conectada.
* `ReLU`: función de activación utilizada en las capas ocultas.
* `Sigmoid`: produce una probabilidad para clasificación binaria.
* `Loss`: mide el error del modelo.
* `Accuracy`: mide la cantidad de predicciones correctas.

### Imagen

<!-- Colocar aquí imagen de la arquitectura de Keras -->

---

# 3. Overfitting

El **overfitting** ocurre cuando el modelo aprende demasiado los datos de entrenamiento y pierde capacidad para generalizar a datos nuevos.

### Imagen

<!-- Colocar aquí gráfica de entrenamiento y validación -->

---

# 4. Regularización y Dropout

### Regularización L2

```python
layers.Dense(
    16,
    activation='relu',
    kernel_regularizer=regularizers.l2(0.001)
)
```

Ayuda a controlar el sobreajuste penalizando pesos demasiado grandes.

### Dropout

```python
layers.Dropout(0.5)
```

Desactiva aleatoriamente parte de las neuronas durante el entrenamiento.

**Importancia:** ambas técnicas ayudan a mejorar la generalización del modelo.

---

# 5. CNN

Una **CNN (Convolutional Neural Network)** está diseñada especialmente para trabajar con imágenes.

Las convoluciones permiten detectar características como bordes, formas y patrones.

### Función principal

```python
nn.Conv2d(...)
```

Realiza la operación de convolución sobre la imagen.

### Dataset

Se trabajó con dos clases:

* `glass = 0`
* `plastic = 1`

El dataset se divide en:

* 70 % entrenamiento
* 15 % validación
* 15 % prueba

Las imágenes se convierten a escala de grises para trabajar con un solo canal.

### Funciones importantes

```python
nn.Conv2d()
```

Extrae características de las imágenes.

```python
nn.CrossEntropyLoss()
```

Calcula el error de clasificación.

```python
torch.optim.Adam()
```

Actualiza los pesos del modelo durante el entrenamiento.

```python
model.train()
```

Activa el modo entrenamiento.

```python
model.eval()
```

Activa el modo evaluación.

### Imagen

<!-- Colocar aquí imagen de la CNN -->

---

# 6. Data Augmentation

Data Augmentation genera variaciones de las imágenes de entrenamiento mediante transformaciones.

Ejemplo:

```python
T.RandomRotation(degrees=10)
```

### Importancia

Ayuda a que el modelo generalice mejor y reduzca la dependencia de las imágenes originales.

### Imagen

<!-- Colocar aquí comparación antes/después de Data Augmentation -->

---

# 7. Transfer Learning

Transfer Learning consiste en utilizar un modelo previamente entrenado y adaptarlo a un nuevo problema.

Ejemplo:

```python
models.resnet18(
    weights=models.ResNet18_Weights.DEFAULT
)
```

Después se adapta la capa final:

```python
resnet.fc = nn.Linear(
    in_features,
    num_classes
)
```

### Importancia

Permite aprovechar características que el modelo ya aprendió y evita entrenar toda la red desde cero.

### Imagen

<!-- Colocar aquí imagen de ResNet / Transfer Learning -->

---

# 8. Grad-CAM

Grad-CAM permite visualizar qué zonas de una imagen influyeron en la predicción del modelo.

### Importancia

Permite interpretar las decisiones de una CNN y observar qué partes de la imagen está utilizando para realizar una clasificación.

### Imagen

<!-- Colocar aquí resultado de Grad-CAM -->

---

# 9. Funciones principales aprendidas

| Función              | Importancia                                |
| -------------------- | ------------------------------------------ |
| `np.dot()`           | Calcula la combinación de entradas y pesos |
| `Dense()`            | Construye capas de una red neuronal        |
| `compile()`          | Configura el entrenamiento                 |
| `fit()`              | Entrena el modelo                          |
| `evaluate()`         | Evalúa el modelo                           |
| `predict()`          | Realiza predicciones                       |
| `Conv2d()`           | Extrae características de imágenes         |
| `CrossEntropyLoss()` | Calcula el error de clasificación          |
| `Adam()`             | Optimiza los pesos                         |
| `Dropout()`          | Ayuda a reducir overfitting                |
| `l2()`               | Aplica regularización                      |
| `model.train()`      | Modo entrenamiento                         |
| `model.eval()`       | Modo evaluación                            |

---

# Conclusión

Se estudiaron tres niveles principales:

1. **Perceptrón:** permitió comprender entradas, pesos, bias y funciones de activación.
2. **Keras:** permitió construir y entrenar redes neuronales para clasificación.
3. **CNN:** permitió aplicar redes neuronales al procesamiento y clasificación de imágenes.

También se aprendieron técnicas para mejorar y analizar los modelos, como regularización, Dropout, Data Augmentation, Transfer Learning y Grad-CAM.

# ¿Cómo podría usarse en nuestro proyecto?

Podriamos usarlo para poder entrenar a una IA a que pueda predecir la turbidez solo con ver la imágen.
