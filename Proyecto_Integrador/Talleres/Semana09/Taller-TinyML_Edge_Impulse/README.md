<div align="center" style="background-color: #0d1b2a; padding: 25px; border-radius: 10px; color: white; margin-bottom: 20px;">
    <h1 style="color: #4cc9f0; margin-bottom: 5px;">Taller de TinyML</h1>
    <h3 style="color: #e0e1dd; margin-top: 0;">Clasificación de Imágenes con XIAO ESP32S3 Sense y Edge Impulse</h3>
    <p style="color: #a0a0a0; font-size: 14px; margin-bottom: 0;">Universidad Peruana Cayetano Heredia</p>
</div>

## Introducción

Este proyecto documenta la implementación de un sistema de visión artificial (TinyML) utilizando la placa XIAO ESP32S3 Sense y la plataforma Edge Impulse. El objetivo principal es entrenar un modelo de aprendizaje automático capaz de clasificar objetos en tiempo real, específicamente distinguir entre una "Billetera" y una "Laptop", desplegando finalmente el modelo directamente en el microcontrolador.

A lo largo de este taller, se abordan desde la configuración inicial del entorno en Arduino IDE y la captura de datos mediante la cámara, hasta el entrenamiento del modelo en la nube y su despliegue final. Las evidencias presentadas a continuación demuestran el correcto funcionamiento del flujo de trabajo, destacando el rendimiento del modelo y su capacidad de inferencia en el dispositivo.

---

## 1. Creación del Proyecto y Carga de Datos

<div style="background-color: #f8f9fa; border-left: 4px solid #4361ee; padding: 15px; margin-bottom: 20px;">
    <strong>Descripción:</strong> Se creó una cuenta y un nuevo proyecto en Edge Impulse. Posteriormente, se subieron las imágenes capturadas previamente por la cámara del XIAO ESP32S3. Las imágenes se organizaron en dos clases distintas: "Billetera" y "Laptop", utilizando la opción de subida por lotes para asignar automáticamente las etiquetas correspondientes a cada archivo.
</div>

<div align="center">
    <img src="./images/figura1.png" alt="Creación de proyecto en Edge Impulse" width="90%" style="border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
    <br>
    <em>Figura 1: Interfaz de creación del proyecto y flujo para añadir datos existentes.</em>
</div>

<br>

## 2. Verificación del Dataset

<div style="background-color: #f8f9fa; border-left: 4px solid #4361ee; padding: 15px; margin-bottom: 20px;">
    <strong>Descripción:</strong> Tras la carga exitosa de las 200 imágenes (100 por clase), se verificó la correcta recepción de los datos en la plataforma. Edge Impulse dividió automáticamente el conjunto de datos en un 80% para entrenamiento y un 20% para pruebas, asegurando una validación adecuada del modelo.
</div>

<div align="center">
    <img src="./images/figura2.png" alt="Verificación del Dataset" width="90%" style="border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
    <br>
    <em>Figura 2: Confirmación de subida y visualización del dataset dividido (200 items).</em>
</div>

<div style="background-color: #fff3cd; border-left: 4px solid #ffc107; padding: 15px; margin-top: 10px; margin-bottom: 20px;">
    <strong>Interpretación:</strong> La plataforma confirma la recepción de 200 imágenes en total (100 de "Billetera" y 100 de "Laptop"). La división 80/20 es ideal para este tipo de proyectos, ya que garantiza que el modelo tenga suficientes datos para aprender (160 imágenes) y un conjunto independiente para validar su rendimiento (40 imágenes). La lista de muestras con etiquetas "Billetera" y "Laptop" confirma que el etiquetado se realizó correctamente.
</div>

<br>

## 3. Rendimiento del Modelo y Gráficos de Dispersión

<div style="background-color: #f8f9fa; border-left: 4px solid #4361ee; padding: 15px; margin-bottom: 20px;">
    <strong>Descripción:</strong> Se evaluó el rendimiento del modelo entrenado. Los resultados muestran una precisión del 100% y una pérdida de 0.04. A continuación, se presentan dos gráficos de dispersión: el primero corresponde a la distribución general de todos los datos procesados (evidenciando una clara separación entre las clases "Billetera" y "Laptop"), y el segundo muestra la clasificación de una muestra aleatoria específica.
</div>

<div align="center">
    <img src="./images/figura3a.png" alt="Métricas y Gráfico de Dispersión General" width="100%" style="border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
    <br>
    <em>Figura 3a: Matriz de confusión, métricas de rendimiento y gráfico de dispersión general de los datos.</em>
</div>

<div style="background-color: #fff3cd; border-left: 4px solid #ffc107; padding: 15px; margin-top: 10px; margin-bottom: 20px;">
    <strong>Interpretación:</strong> La matriz de confusión muestra un rendimiento perfecto: el 100% de las billeteras fueron clasificadas correctamente como "Billetera" y el 100% de las laptops como "Laptop", sin falsos positivos ni falsos negativos. El F1-Score de 1.00 en ambas clases confirma un equilibrio perfecto entre precisión y recall. El gráfico de dispersión general muestra una separación clara entre los clusters de ambas clases, lo que indica que el modelo aprendió características distintivas y discriminativas.
</div>

<div align="center">
    <img src="./images/figura3b.png" alt="Gráfico de Dispersión General" width="80%" style="border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
    <br>
    <em>Figura 3b: Gráfico de dispersión general mostrando la separación de las clases.</em>
</div>

<div style="background-color: #fff3cd; border-left: 4px solid #ffc107; padding: 15px; margin-top: 10px; margin-bottom: 20px;">
    <strong>Interpretación:</strong> Este gráfico de dispersión representa la distribución de todas las muestras del conjunto de datos en el espacio de características aprendido por el modelo. Los puntos amarillos corresponden a "Billetera - correct" y los verdes a "Laptop - correct". La clara separación entre ambos grupos demuestra que el modelo logró encontrar un límite de decisión efectivo, lo que se traduce en la alta precisión obtenida.
</div>

<div align="center">
    <img src="./images/figura3c.png" alt="Clasificación de Muestra Aleatoria" width="80%" style="border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
    <br>
    <em>Figura 3c: Clasificación de una muestra aleatoria (Billetera_092) con un 100% de confianza.</em>
</div>

<div style="background-color: #fff3cd; border-left: 4px solid #ffc107; padding: 15px; margin-top: 10px; margin-bottom: 20px;">
    <strong>Interpretación:</strong> Al cargar una muestra aleatoria del conjunto de pruebas (Billetera_092), el modelo la clasificó correctamente como "Billetera" con una confianza del 100%. El gráfico de la derecha (Detailed result) muestra los valores de probabilidad para cada clase: 1.00 para Billetera y 0.00 para Laptop, lo que confirma que el modelo no tiene ambigüedad en esta predicción. La imagen de la billetera sobre la alfombra verde permite verificar visualmente que la clasificación es coherente con el objeto real.
</div>

<br>

## 4. Inferencia en Tiempo Real (Monitor Serie)

<div style="background-color: #f8f9fa; border-left: 4px solid #4361ee; padding: 15px; margin-bottom: 20px;">
    <strong>Descripción:</strong> Utilizando el Monitor Serie de Arduino IDE a 115200 baudios, se observó la inferencia del modelo en tiempo real. Al colocar una billetera frente a la cámara, el sistema arrojó una confianza superior al 89%. De manera similar, al presentar una laptop, la predicción cambió correctamente a "Laptop" con una confianza del 83%, demostrando la eficacia del modelo desplegado.
</div>

<div align="center">
    <img src="./images/figura4.png" alt="Predicciones en Monitor Serie" width="90%" style="border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
    <br>
    <em>Figura 4: Salida del Monitor Serie mostrando las predicciones para "Billetera" y "Laptop".</em>
</div>

<div style="background-color: #fff3cd; border-left: 4px solid #ffc107; padding: 15px; margin-top: 10px; margin-bottom: 20px;">
    <strong>Interpretación:</strong> El Monitor Serie muestra el tiempo de inferencia (DSP: 14 ms), lo que indica que el modelo es lo suficientemente rápido para aplicaciones en tiempo real. Los valores de predicción oscilan entre 0.89 y 0.76 para "Billetera" cuando este objeto está frente a la cámara, y cambian drásticamente a 0.83 para "Laptop" cuando se presenta el otro objeto. La consistencia en las predicciones a lo largo del tiempo demuestra que el modelo es robusto y estable en condiciones reales de uso.
</div>

<br>

---

<div align="center" style="background-color: #f1f1f1; padding: 15px; border-radius: 8px; color: #333;">
    <strong>Conclusión:</strong> El taller se completó exitosamente, logrando implementar un sistema de TinyML funcional en el XIAO ESP32S3 Sense. Las evidencias demuestran que el modelo es capaz de clasificar imágenes con alta precisión y baja latencia, validando el uso de Edge Impulse y Arduino IDE para proyectos de visión artificial en el borde.
</div>
