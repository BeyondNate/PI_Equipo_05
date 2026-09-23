# <h1 align="center">────୨ৎ──── TALLER DE REDES NEURONALES ────୨ৎ────</h1>


# ✿ Introducción

En este taller se trabajaron diferentes metodologías relacionadas con las redes neuronales y el aprendizaje automático, aplicadas principalmente en problemas de clasificación. La idea principal fue observar cómo un modelo puede aprender patrones a partir de los datos y cómo diferentes técnicas pueden influir en su entrenamiento y en los resultados obtenidos. También se pudo notar que los resultados pueden variar entre ejecuciones, por lo que no solo es importante construir el modelo, sino también revisar cómo se entrenó y cómo se está evaluando.

---

# ✿ Documentación

En esta sección se encuentran los archivos utilizados como apoyo y fuente de datos para el desarrollo del taller.


| Archivo                          | Descripción                                                                                      | Visualización                                      |
| --------------------------------- | -------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| 📓 **Notebook**                  | Notebook utilizado para desarrollar el taller y aplicar las metodologías (CNN, Keras y perceptrón). | [🔎 Ver notebook](./Idania%20Parhuay_%20Redes_neuronales_ss.ipynb) |
| 🗂️ **Dataset (TrashNet)**        | Script adaptador que organiza y divide el dataset de imágenes de vidrio/plástico (train/val/test), usado como aporte de datos para entrenar la CNN. | [🔎 Ver script](./trash_dataset.py)                |           |

---
# ✿  Desarollo

## 1. Redes neuronales convolucionales (CNN) reconociendo vidrio y plástico
Una de las metodologías principales trabajadas fue la red neuronal convolucional o CNN, utilizada para clasificar imágenes.

### 1.1. Preparación del dataset

Antes de entrenar cualquier modelo hay que tener las imágenes bien organizadas y divididas en train/val/test; si esto no se hace bien, después el modelo se estaría evaluando con datos que ya "vio" antes, y el resultado no sería confiable.

<div align="center">
  <img src="https://github.com/user-attachments/assets/a7aa1878-000a-4124-aa9b-735dff11fa8b" width="600">
  
  *Imagen 1: ejemplos de vidrio y plástico del dataset, ya cargados en escala de grises*
  
</div>

Acá se ve cómo quedaron algunas muestras ya cargadas: vidrio y plástico en escala de grises, que es justo el formato que espera la CNN que armamos después.

### 1.2. Arquitectura y entrenamiento desde cero

Entrenamos una CNN chiquita desde cero, unos bloques de convolución con ReLU y MaxPooling, y al final una capa que decide entre las dos clases. Así, sin ningún conocimiento previo, el modelo apenas llegó como a 61% de exactitud. Tiene sentido porque parte de pesos aleatorios y no tuvo tantas épocas para aprender.

<div align="center">
  <img src="https://github.com/user-attachments/assets/d0eb82c0-ba48-4312-92ff-96a72f1c5d4b" width="600">
  
*Imagen 2: curva de pérdida de entrenamiento (CNN desde cero) a lo largo de las épocas.*
</div>

La pérdida baja, pero lento y con algunos tramos casi planos, lo que ya adelanta que al modelo le está costando aprender con tan pocas épocas.

<div align="center">
  <img src="https://github.com/user-attachments/assets/bb9d189a-b7b6-439a-b026-0c9c0a7a29e1" width="600">
  
*Imagen 3: métricas de validación (Accuracy vs. ROC-AUC) por época, CNN desde cero.*
</div>

Acá podemos apreciar que el ROC-AUC de validación se mantiene bastante estable y alto (arriba de 0.70) desde temprano, pero el accuracy salta bastante de una época a otra, incluso baja antes de subir. Eso pasa porque el accuracy depende del umbral de decisión (0.5), mientras que el ROC-AUC mide qué tan bien separa las clases en general; el modelo ya distinguía razonablemente bien, pero el punto de corte todavía no estaba bien calibrado.

* **Evaluación final en test:**
  
<div align="center">
  <img src="https://github.com/user-attachments/assets/bd0e808c-3b15-4cc2-be8d-c2e35b374ddd" width="600">

*Imagen 4 : matriz de confusión del modelo CNN entrenado desde cero, sobre el set de test.*
</div>

En números, el modelo cerró con Test accuracy = 0.6107 y Test ROC-AUC = 0.6727, con precision y recall alrededor de 0.61 en ambas clases. Esto confirma lo que ya se veía en la matriz: el modelo funciona apenas un poco mejor que adivinar al azar, porque todavía no tuvo suficiente entrenamiento.

<div align="center">
  <img src="https://github.com/user-attachments/assets/5d7461d2-7038-48fa-99bd-47c19361a2d8" width="600">

*Imagen 5: matriz de confusión del modelo CNN entrenado desde cero, sobre el set de test.*
</div>

La matriz de confusión muestra que el modelo se equivoca de forma bastante pareja entre vidrio y plástico (27 y 31 errores en cada dirección), o sea que no tiene un sesgo fuerte hacia una sola clase, simplemente le falta aprender más.


### 1.3. Transfer learning

Acá en vez de entrenar todo desde cero, se reutiliza una red ya entrenada (ResNet18) y solo se ajustan sus últimas capas a nuestro problema específico.

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

El código muestra el proceso en dos pasos: primero se congelan todas las capas de la red y solo se entrena la última (la que clasifica), y después se descongelan las capas finales del extractor (layer4) para un ajuste más fino.

<div align="center">
  <img  src="https://github.com/user-attachments/assets/613dc586-0e52-44fd-9141-78839085ad10" width="600">

*Imagen 6: salida del cuaderno con las métricas finales del modelo con transfer learning.*
</div>

Con esto el accuracy subió de 61% a 90.60%, y el ROC-AUC a 0.9544, con precision y recall por encima de 0.88 en las dos clases. Fue el salto más grande de todo el taller, y confirma que no hacía falta una arquitectura más compleja: bastaba con aprovechar lo que la red ya "sabía" ver (bordes, texturas, formas) y ajustar solo lo específico de nuestro caso.

### 1.4. Grad-CAM (interpretabilidad)

También vimos Grad-CAM, el cual sirve para visualizar en qué parte de la imagen se está fijando el modelo al momento de decidir, algo que normalmente queda "oculto" dentro de la red.

<div align="center">
  <img src="https://github.com/user-attachments/assets/a255247b-907a-458b-8089-8eaff6d79183" width="600">

*Imagen 7: : mapa de calor Grad-CAM sobre una imagen de botella de vidrio.*  
</div>

El mapa de calor se concentra en el cuerpo de la botella y no en el fondo de la imagen, lo que da bastante más confianza en que el modelo está aprendiendo algo razonable y no memorizando ruido de fondo.

---

## 2. Reseñas de películas con Keras

### 2.1. Vectorización y modelo

Como una red no puede procesar texto directamente, primero se convierte cada reseña en un vector binario según qué palabras contiene (de las 10,000 más frecuentes), y luego una red densa de dos capas aprende a partir de esos vectores si la reseña es positiva o negativa.

<div align="center">
  <img src="https://github.com/user-attachments/assets/3a35106e-55b5-4728-8660-8dc11a04a0a6" width="600">

*Imagen 8: : salida del cuaderno al evaluar el modelo base de Keras sobre el set de test.*  
</div>

El modelo terminó con accuracy = 0.8588 y loss = 0.5923 en test, un resultado bastante bueno para una red tan simple de solo dos capas densas.

### 2.2. Sobreajuste

Comparar la curva de pérdida de entrenamiento contra la de validación sirve para detectar el momento en que el modelo deja de generalizar y empieza a memorizar.

<div align="center">
  <img src="https://github.com/user-attachments/assets/af34359f-724d-4985-a0b1-8f68dc1157a0" width="600">

*Imagen 9: curvas de pérdida de entrenamiento y validación, mostrando el punto de sobreajuste*
</div>

En la gráfica se nota clarito el punto donde la curva de entrenamiento (azul) sigue bajando, pero la de validación (naranja) empieza a subir. Esa separación es la señal típica de sobreajuste: el modelo ya no está aprendiendo patrones generales, solo se está aprendiendo las reseñas de entrenamiento de memoria.

### 2.3. Regularización y Dropout

Son dos formas distintas de frenar ese sobreajuste: la regularización castiga los pesos muy grandes, y el Dropout apaga aleatoriamente la mitad de las neuronas mientras entrena, para que la red no dependa demasiado de unas pocas.

<div align="center">
  <img src="https://github.com/user-attachments/assets/102e7ede-db00-4a22-8380-14b4fd543ba4" width="600">

*Imagen 10: curva de validación con Dropout comparada con el modelo original.*
</div>

Con Dropout, la curva de validación se mantiene más estable en comparación con el modelo original, aunque sí se ve un pico al inicio porque la red todavía se está acomodando a entrenar con neuronas apagándose al azar.

--- 

## 3. El perceptrón

El perceptrón es solo una suma ponderada de entradas más un sesgo, pasada por una función de activación para tomar una decisión binaria (0 o 1). Se usó como ejemplo un equipo industrial que decide si hay riesgo de sobrecalentamiento según su temperatura y vibración.

```python
def perceptron(inputs, weights, bias, activation_func):
z = np.dot(inputs, weights) + bias
return activation_func(z)

# Pesos y sesgo (bias) del perceptron
weights = np.array([0.5,-0.5])
bias = -30

# Entradas: temperatura y vibracion del equipo
inputs = np.array( [temperatura, vibracion])
output_step = perceptron(inputs, weights, bias, step_function)

```
El código traduce esa idea a un caso concreto: un equipo industrial donde la temperatura y la vibración (las entradas) se combinan con pesos y un sesgo fijos para decidir si hay riesgo de sobrecalentamiento. Con los valores usados, el resultado fue que no había alerta.

### 3.1. Compuertas lógicas: AND, OR y XOR

Esta parte sirve para ver hasta dónde llega un solo perceptrón, qué problemas puede resolver y cuáles no.

<div align="center">
  <img src="https://github.com/user-attachments/assets/e206dd9d-a390-4f2f-b59d-fac71606ce45" width="600">

*Imagen 11: fronteras de decisión del perceptrón para las compuertas AND, OR y XOR.*
</div>

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
# ✿  Discusión

Sobre todos estos resultados hay dos puntos que vale la pena discutir. Primero, un número aislado puede llevar a conclusiones apresuradas si no se contrasta con su gráfico: el 61% de exactitud de la CNN desde cero, visto solo como cifra, parece un fracaso, pero la matriz de confusión muestra que el error está repartido de forma pareja entre clases, lo que indica falta de entrenamiento y no un problema estructural del modelo. Segundo, la variabilidad de resultados entre distintas corridas del mismo código, evidenciada al comparar con compañeros, confirma que la reproducibilidad importa tanto como el resultado en sí; fijar semillas y documentar la versión del entorno debería ser lo ideal [1].

---

# ✿  Conclusión

En conclusión, los tres bloques del taller demuestran un mismo principio aplicado a distintas escalas, pesos, sesgo y una función de activación, y que el rendimiento de un modelo no depende tanto de qué tan compleja es su arquitectura, sino de cuánto conocimiento previo se aprovecha: el salto de 61% a 90.6% de exactitud en la CNN se explica casi enteramente por el uso de transfer learning, no por cambios estructurales. De forma parecida, en la red de Keras el modelo base ya alcanzaba una exactitud aceptable (85.9%), y el aporte real de la regularización y el Dropout fue de generalización, no de exactitud bruta.

---
#  Bibliografía

[1] I.C.Parhuay Meza, "Taller de Redes Neuronales: clasificación de imágenes con
    CNN, análisis de texto con Keras y fundamentos del perceptrón," trabajo de curso,
    *Fac. de Ciencias e Ingeniería, Universidad Peruana Cayetano Heredia*, Lima, Perú, 2026.
