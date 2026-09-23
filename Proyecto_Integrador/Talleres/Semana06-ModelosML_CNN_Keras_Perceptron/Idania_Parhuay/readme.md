<h1 align="center">────୨ৎ──── TALLER DE REDES NEURONALES ────୨ৎ────</h1>

# ✿ INTRODUCCIÓN

En este taller se trabajaron diferentes metodologías relacionadas con las redes neuronales y el aprendizaje automático, aplicadas principalmente en problemas de clasificación. La idea principal fue observar cómo un modelo puede aprender patrones a partir de los datos y cómo diferentes técnicas pueden influir en su entrenamiento y en los resultados obtenidos. También se pudo notar que los resultados pueden variar entre ejecuciones, por lo que no solo es importante construir el modelo, sino también revisar cómo se entrenó y cómo se está evaluando.


## 1. Redes neuronales convolucionales (CNN) reconociendo vidrio y plástico
Una de las metodologías principales trabajadas fue la red neuronal convolucional o CNN, utilizada para clasificar imágenes.

### 1.1. Preparación del dataset

Antes de entrenar cualquier modelo hay que tener las imágenes bien organizadas y divididas en train/val/test; si esto no se hace bien, después el modelo se estaría evaluando con datos que ya "vio" antes, y el resultado no sería confiable.

<p align="center">
  <img src="https://github.com/user-attachments/assets/a7aa1878-000a-4124-aa9b-735dff11fa8b" width="600">
  
  *Imagen 1: ejemplos de vidrio y plástico del dataset, ya cargados en escala de grises*
  
</p>

Acá se ve cómo quedaron algunas muestras ya cargadas: vidrio y plástico en escala de grises, que es justo el formato que espera la CNN que armamos después.

### 1.2. Arquitectura y entrenamiento desde cero

Entrenamos una CNN chiquita desde cero, unos bloques de convolución con ReLU y MaxPooling, y al final una capa que decide entre las dos clases. Así, sin ningún conocimiento previo, el modelo apenas llegó como a 61% de exactitud. Tiene sentido porque parte de pesos aleatorios y no tuvo tantas épocas para aprender.

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

Lo que sí cambió todo fue usar transfer learning, ya que en vez de entrenar desde cero, partimos de una ResNet18 ya entrenada y solo reentrenamos sus últimas capas. Con eso el accuracy subió a 90%. Ahí entendí por qué se usa tanto en la práctica: no hace falta reinventar todo, con aprovechar lo que el modelo ya "sabe" ver (bordes, texturas, formas) y enseñarle solo lo específico de nuestro caso ya es suficiente.

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

También vimos Grad-CAM, el cual sirve para visualizar en qué parte de la imagen se está fijando el modelo al momento de decidir, algo que normalmente queda "oculto" dentro de la red.

<p align="center">
  <img src="https://github.com/user-attachments/assets/a255247b-907a-458b-8089-8eaff6d79183" width="600">
</p>

El mapa de calor se concentra en el cuerpo de la botella y no en el fondo de la imagen, lo que da bastante más confianza en que el modelo está aprendiendo algo razonable y no memorizando ruido de fondo.

---

## 2. Reseñas de películas con Keras

### 2.1. Vectorización y modelo

Como una red no puede procesar texto directamente, primero se convierte cada reseña en un vector binario según qué palabras contiene (de las 10,000 más frecuentes), y luego una red densa de dos capas aprende a partir de esos vectores si la reseña es positiva o negativa. Con este modelo se llegó a casi 86% de exactitud en test.

### 2.2. Sobreajuste

Comparar la curva de pérdida de entrenamiento contra la de validación sirve para detectar el momento en que el modelo deja de generalizar y empieza a memorizar.

<p align="center">
  <img src="https://github.com/user-attachments/assets/af34359f-724d-4985-a0b1-8f68dc1157a0" width="600">
</p>

En la gráfica se nota clarito el punto donde la curva de entrenamiento (azul) sigue bajando, pero la de validación (naranja) empieza a subir. Esa separación es la señal típica de sobreajuste: el modelo ya no está aprendiendo patrones generales, solo se está aprendiendo las reseñas de entrenamiento de memoria.

### 2.3. Regularización y Dropout

Después probamos dos formas de frenar eso: regularización (castigando pesos muy grandes) y Dropout (apagando neuronas al azar mientras entrena). Las dos ayudan a que la curva de validación no se dispare tanto.

<p align="center">
  <img src="https://github.com/user-attachments/assets/102e7ede-db00-4a22-8380-14b4fd543ba4" width="600">
</p>

Con Dropout, la curva de validación se mantiene bastante más estable en comparación con el modelo original, aunque sí se ve un pico al inicio porque la red todavía se está acomodando a entrenar con neuronas apagándose al azar.

--- 

## 3. El perceptrón (la base de todo)

El perceptrón es solo una suma ponderada de entradas más un sesgo, pasada por una función de activación. Se usó como ejemplo un equipo industrial que decide si hay riesgo de sobrecalentamiento según su temperatura y vibración.

### 3.1. Compuertas lógicas: AND, OR y XOR

Esta parte sirve para ver hasta dónde llega un solo perceptrón, qué problemas puede resolver y cuáles no.

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

Es básicamente lo que ya hace nuestro sistema: toma pH y turbidez como entradas, las combina con la base de datos estandarizada, y esa combinación decide cuánta mezcla de quitosano dosificar. Es la misma lógica del ejemplo de sobrecalentamiento que vimos, solo que mi proyecto esta aplicada al agua en vez de a un motor.

### 5.2. Transfer learning → verificación visual del agua

Tendría sentido si en algún momento agregamos una cámara para verificar visualmente si el agua ya quedó clara, en vez de depender solo de la sonda. No tendría sentido entrenar una CNN desde cero con las pocas fotos que podríamos tomar del reactor; mejor partir de un modelo ya entrenado y solo ajustar las últimas capas.

### 5.3. Grad-CAM → interpretabilidad del sistema

Nos daría algo que actualmente no tenemos, que es poder ver en qué parte de la imagen del vaso se fija el modelo (¿en los grumos sedimentados? ¿en el color del agua de arriba?), igual que se vio con la botella.

### 5.4. Reproducibilidad → calibración de sensores

La parte de por qué cambian los resultados entre laptops también aplica a nuestros sensores: si nuestras sondas de pH y turbidez no están bien calibradas entre una prueba y otra, nos va a pasar lo mismo que a un modelo sin semilla fija, resultados que varían aunque el proceso sea "el mismo".

---
# ✿ Discusión






---

# ✿ Conclusión

Lo que más me llevo de este taller es que un mismo principio: pesos, sesgo y una función de activación, se repite en distintas escalas: desde un perceptrón que decide con dos números si algo se está sobrecalentando, hasta una CNN con transfer learning que distingue vidrio de plástico con 90% de exactitud. Y creo que varias de estas ideas, sobre todo el perceptrón y el transfer learning, se pueden llevar directo a nuestro proyecto de clarificación de agua.
