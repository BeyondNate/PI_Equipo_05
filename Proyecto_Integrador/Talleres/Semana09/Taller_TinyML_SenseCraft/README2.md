# Taller TinyML con Sensecraft AI - Implementación de modelo propio.

## Integrantes
- Leonel Urbano
- Gael Milla
## Descripción del proceso
El proyecto consiste en desarrollar e implementar un sistema de clasificación de imágenes mediante técnicas de TinyML, utilizando la placa Seeed Studio XIAO ESP32S3 Sense y la plataforma SenseCraft AI. El propósito es distinguir dos categorías de objetos: lentes y celulares.
Para construir el conjunto de datos, se tomaron fotografías de distintos modelos de lentes y celulares, procurando variar sus posiciones y ángulos de orientación. De esta manera, el modelo recibe ejemplos visuales variados y puede aprender las características que permiten diferenciar ambas clases. Las imágenes recopiladas se cargaron en SenseCraft AI, donde se organizaron por categorías y se utilizaron para entrenar el modelo de clasificación.
Una vez entrenado, el modelo se desplegó en la placa XIAO ESP32S3 Sense. La cámara captura imágenes del objeto que se coloca frente al dispositivo y el modelo realiza la predicción en tiempo real. Como salida física, se utiliza el LED integrado en la placa para indicar la detección de una de las clases.


## Objetivo del modelo

**Objetivo general:**

Desarrollar e implementar un modelo propio de TinyML capaz de clasificar imágenes en tiempo real para distinguir entre lentes y celulares, utilizando la placa XIAO ESP32S3 Sense y la plataforma SenseCraft AI.

**Objetivos específicos:**
- Crear un conjunto de datos mediante la captura de fotografías de diferentes lentes y celulares.
- Variar las posiciones y los ángulos de los objetos fotografiados para proporcionar ejemplos visuales diversos al modelo.
- Organizar y etiquetar las imágenes en dos clases: lentes y celulares.
- Entrenar un modelo de clasificación de imágenes en SenseCraft AI y establecer un nivel de confianza del 53%.
- Desplegar el modelo entrenado en la placa XIAO ESP32S3 Sense.
- Comprobar el funcionamiento del sistema mediante la cámara y la señal del LED integrado.

## Datos
- Fueron tomados en tiempo real.
- Cantidad de imágenes: 138 de lentes y 138 de celulares
- Archivo: `datos.zip`
## 3. Elementos utilizados

| Elemento | Descripción |
|---|---|
| Dispositivo | Seeed Studio XIAO ESP32S3 Sense |
| Cámara | Módulo de cámara incluido en la placa de expansión Sense (conexión tipo módulo) |
| Indicador de salida | LED integrado en la placa XIAO ESP32S3 |
| Plataforma | [SenseCraft AI](https://sensecraft.seeed.cc/ai/) |
| Alimentación y datos | Cable USB-C |

## Procedimiento
1. Preparación del dispositivo: Conectar la placa XIAO ESP32S3 Sense al equipo mediante el cable USB-C y verificar que sea reconocida para trabajar con SenseCraft AI.

2. Captura de imágenes: Fotografiar diferentes lentes y celulares. Para cada categoría, tomar imágenes desde distintas posiciones y ángulos, procurando incluir variedad de modelos y apariencias.

3. Organización de los datos: Separar las fotografías en dos clases, lentes y celulares, y cargarlas en el proyecto de SenseCraft AI con sus etiquetas correspondientes.

4. Entrenamiento: Utilizar las imágenes para entrenar el modelo de clasificación. En este proyecto se recopilaron 138 fotografías de lentes y 138 de celulares, para un total de 276 imágenes.

5. Configuración de confianza: Establecer el nivel de confianza indicado para el proyecto en 53%. Este valor funciona como umbral para aceptar una predicción; no representa por sí mismo el porcentaje de exactitud del modelo.

6. Vista previa y comprobación: Utilizar la vista previa de SenseCraft AI para comprobar las predicciones del modelo con imágenes de las dos categorías.

7. Despliegue: Transferir o desplegar el modelo entrenado en la XIAO ESP32S3 Sense, siguiendo el flujo de implementación de SenseCraft AI.

8. Prueba física: Presentar lentes y celulares frente a la cámara y observar la clase reconocida y la respuesta del LED integrado.
   
## Entrenamiento
- Parámetros usados: Para lentes, 138 fotos de diferentes lentes en diferentes angulos. Para celulares, 138 fotos de diferentes celulares, en diferentes angulos
- El nivel de confianza es del 50%.
- Captura: ![](img/Entrenamiento.jpeg)
- **Figura5.** Evidencia de Leonel Urbano . En esta imagen, Leonel se encargó de recopilar imagenes de los lentes.
- Data: ![](img/data.jpeg)


## Vista previa y despliegue
- Vista previa y despliegue del modelo: ![](img/preview.jpg)
**Figura 5.** Evidencia de Gael milla. En esta imagen, Gael se encargó de Comprobar el correcto funcionamiento del modelo.
## Salida GPIO
- La clase que activa el LED es "lentes"

| Integrante | Captura | Resultado |
|---|---|---|
 [Leonel Urbano] | ![](img/gpio1.jpeg) | LED encendido al mostrar celular |
| [Gael Milla] | ![](img/gpio2.jpg) | LED encendido al mostrar celular |

## Conclusiones:

- Se desarrolló un proyecto de TinyML orientado a la clasificación de imágenes en dos categorías: lentes y celulares, utilizando la placa XIAO ESP32S3 Sense y la plataforma SenseCraft AI.
  
- La recopilación de 138 imágenes por clase permitió formar un conjunto de datos equilibrado de 276 fotografías. La variación de posiciones y ángulos buscó proporcionar diversidad visual durante el entrenamiento.
  
- Se realizó el entrenamiento y despliegue de un modelo de tipo clasificación de imágenes (Image Classification) para efectuar predicciones a partir de las imágenes capturadas por la cámara.
  
- La integración del LED permite contar con una señal física de salida asociada a la clase configurada. Sin embargo, es necesario verificar la discrepancia registrada entre la clase lentes y las pruebas que indican que el LED se encendió al mostrar un celular.
  
- Para evaluar con mayor precisión el desempeño del sistema, en futuras pruebas sería conveniente registrar predicciones correctas e incorrectas con imágenes que no se hayan utilizado durante el entrenamiento y calcular métricas como la exactitud.

## Archivos del repositorio
- `datos.zip`
