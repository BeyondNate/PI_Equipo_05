<h1 align="center">────୨ৎ──── TALLER DE REDES NEURONALES ────୨ৎ────</h1>

# ✿ INTRODUCCIÓN

En este taller se trabajaron diferentes metodologías relacionadas con las redes neuronales y el aprendizaje automático, aplicadas principalmente en problemas de clasificación. Se revisó conceptos básicos, como el funcionamiento de un perceptrón, y posteriormente se trabajó con metodologías más completas como las redes neuronales convolucionales (CNN), el data augmentation, el transfer learning y algunas técnicas para evitar el sobreajuste.

La idea principal fue observar cómo un modelo puede aprender patrones a partir de los datos y cómo diferentes técnicas pueden influir en su entrenamiento y en los resultados obtenidos. También se pudo notar que los resultados pueden variar entre ejecuciones, por lo que no solo es importante construir el modelo, sino también revisar cómo se entrenó y cómo se está evaluando.


## 1. Redes neuronales convolucionales (CNN) reconociendo vidrio y plástico
Una de las metodologías principales trabajadas fue la red neuronal convolucional o CNN, utilizada para clasificar imágenes.

### 1.1. Preparación del dataset

Primero armamos un dataset con fotos de vidrio y plástico (TrashNet), y entrenamos una CNN chiquita desde cero: unos bloques de convolución con ReLU y MaxPooling, y al final una capa que decide entre las dos clases.

<p align="center">
  <img src="https://github.com/user-attachments/assets/a7aa1878-000a-4124-aa9b-735dff11fa8b" width="600">
</p>

### 1.2. Arquitectura y entrenamiento desde cero

Entrenamos una CNN chiquita desde cero: unos bloques de convolución con ReLU y MaxPooling, y al final una capa que decide entre las dos clases. Así, sin ningún conocimiento previo, el modelo apenas llegó como a 61% de exactitud. Tiene sentido porque parte de pesos aleatorios y no tuvo tantas épocas para aprender.

<p align="center">
  <img src="https://github.com/user-attachments/assets/d0eb82c0-ba48-4312-92ff-96a72f1c5d4b" width="600">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/bb9d189a-b7b6-439a-b026-0c9c0a7a29e1" width="600">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/5d7461d2-7038-48fa-99bd-47c19361a2d8" width="600">
</p>


### 1.3. Transfer learning

Lo que sí cambió todo fue usar transfer learning: en vez de entrenar desde cero, partimos de una ResNet18 ya entrenada y solo reentrenamos sus últimas capas. Con eso el accuracy subió a 90%. Ahí entendí por qué se usa tanto en la práctica: no hace falta reinventar todo, con aprovechar lo que el modelo ya "sabe" ver (bordes, texturas, formas) y enseñarle solo lo específico de nuestro caso ya es suficiente.

```python
resnet = models.resnet18(weights=models.ResNet18_Weights.DEFAULT)

in_features = resnet.fc.in_features

resnet.fc = nn.Linear(in_features, num_classes)

resnet = resnet.to(device)

for name, param in resnet.named_parameters():
    param.requires_grad = False

for param in resnet.fc.parameters():
    param.requires_grad = True

resnet
```
Luego en el Fine-tuning:

```python
for name, param in resnet.named_parameters():
    if name.startswith("layer4") or name.startswith("fc"):
        param.requires_grad = True

trainable_params = [p for p in resnet.parameters() if p.requires_grad]
len(trainable_params)
```

### 1.4. Grad-CAM (interpretabilidad)

También vimos Grad-CAM, que muestra en qué parte de la imagen se fija el modelo para decidir. Fue interesante ver que efectivamente se concentraba en el cuerpo de la botella y no en el fondo, o sea que no estaba "adivinando" sino aprendiendo algo razonable.

<p align="center">
  <img src="https://github.com/user-attachments/assets/a255247b-907a-458b-8089-8eaff6d79183" width="600">
</p>

---

## 2. Reseñas de películas con Keras

### 2.1. Vectorización y modelo

Acá la idea es parecida pero con texto: se convierten las reseñas en vectores según qué palabras contienen, y una red densa de dos capas decide si son positivas o negativas. Terminó con casi 86% de exactitud.

### 2.2. Sobreajuste

Lo que más rescato de esta parte fue ver el sobreajuste con mis propios ojos: en la gráfica, la pérdida de entrenamiento sigue bajando pero la de validación empieza a subir en algún punto. Ahí es cuando el modelo ya no está aprendiendo, solo está memorizando.

<p align="center">
  <img src="https://github.com/user-attachments/assets/af34359f-724d-4985-a0b1-8f68dc1157a0" width="600">
</p>


### 2.3. Regularización y Dropout

Después probamos dos formas de frenar eso: regularización (castigando pesos muy grandes) y Dropout (apagando neuronas al azar mientras entrena). Las dos ayudan a que la curva de validación no se dispare tanto.

<p align="center">
  <img src="https://github.com/user-attachments/assets/102e7ede-db00-4a22-8380-14b4fd543ba4" width="600">
</p>

--- 

## 3. El perceptrón (la base de todo)

El perceptrón es solo una suma ponderada de entradas más un sesgo, pasada por una función de activación. Se usó como ejemplo un equipo industrial que decide si hay riesgo de sobrecalentamiento según su temperatura y vibración.

### 3.1. Compuertas lógicas: AND, OR y XOR

Lo que más me quedó grabado fue ver que un solo perceptrón puede resolver AND y OR (se puede trazar una línea que separe las clases), pero no puede resolver XOR, porque ahí no existe esa línea. Recién con varias neuronas juntas se puede.

El código traduce esa idea a un caso concreto: un equipo industrial donde la temperatura y la vibración (las entradas) se combinan con pesos y un sesgo fijos para decidir si hay riesgo de sobrecalentamiento. Con los valores usados, el resultado fue que no había alerta.

<p align="center">
  <img src="https://github.com/user-attachments/assets/e206dd9d-a390-4f2f-b59d-fac71606ce45" width="600">
</p>

En el gráfico se ve que para XOR no existe ninguna línea recta que separe los puntos (0,1) y (1,0) de los otros dos, a diferencia de AND y OR donde sí se puede trazar una. Por eso XOR necesita más de un perceptrón combinado para resolverse.

---

## 4. ¿Por qué a cada uno le sale distinto?

Algo que noté comparando con mis compañeros es que, corriendo el mismo código, a nadie le salen los mismos números exactos. Esto pasa porque en ningún momento se fija una semilla para los pesos iniciales de la red, ni para el orden en que el DataLoader mezcla los datos en cada época. A eso se suma que cada laptop puede entrenar con GPU o con CPU, y que las librerías se instalan sin una versión fija, así que pueden variar según cuándo se corra el notebook. Lo único que sí se mantiene igual es la partición de train/val/test, porque esa parte del código sí usa una semilla fija.

--- 

## 5. Cómo lo uso en mi proyecto (clarificación de agua con quitosano)

### 5.1. Perceptrón → lógica de dosificación

Es básicamente lo que ya hace nuestro sistema: toma pH y turbidez como entradas, las combina con la base de datos estandarizada, y esa combinación decide cuánta mezcla de quitosano dosificar. Es la misma lógica del ejemplo de sobrecalentamiento, solo que aplicada a agua en vez de a un motor.

### 5.2. Transfer learning → verificación visual del agua

Tendría sentido si en algún momento agregamos una cámara para verificar visualmente si el agua ya quedó clara, en vez de depender solo de la sonda. No tendría sentido entrenar una CNN desde cero con las pocas fotos que podríamos tomar del reactor; mejor partir de un modelo ya entrenado y solo ajustar las últimas capas.

### 5.3. Grad-CAM → interpretabilidad del sistema

Nos daría algo que hoy no tenemos: poder ver en qué parte de la imagen del vaso se fija el modelo (¿en los grumos sedimentados? ¿en el color del agua de arriba?), igual que se vio con la botella.

### 5.4. Reproducibilidad → calibración de sensores

La parte de por qué cambian los resultados entre laptops también aplica a nuestros sensores: si nuestras sondas de pH y turbidez no están bien calibradas entre una prueba y otra, nos va a pasar lo mismo que a un modelo sin semilla fija, resultados que varían aunque el proceso sea "el mismo".

---

# ✿ Conclusión

Lo que más me llevo de este taller es que un mismo principio —pesos, sesgo y una función de activación— se repite en distintas escalas: desde un perceptrón que decide con dos números si algo se está sobrecalentando, hasta una CNN con transfer learning que distingue vidrio de plástico con 90% de exactitud. Y creo que varias de estas ideas, sobre todo el perceptrón y el transfer learning, se pueden llevar directo a nuestro proyecto de clarificación de agua.
