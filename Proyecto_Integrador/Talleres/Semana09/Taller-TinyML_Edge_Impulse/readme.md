<div align="center" style="background-color: #0d1b2a; padding: 20px; border-radius: 10px; color: white;">
    <h1 style="color: #4cc9f0; margin-bottom: 5px;">Taller de TinyML</h1>
    <h3 style="color: #e0e1dd; margin-top: 0;">Clasificación de Imágenes con XIAO ESP32S3 Sense y Edge Impulse</h3>
    <p style="color: #a0a0a0; font-size: 14px;">Universidad Peruana Cayetano Heredia</p>
</div>

<br>

## Introducción

Este proyecto documenta la implementación de un sistema de visión artificial (TinyML) utilizando la placa XIAO ESP32S3 Sense y la plataforma Edge Impulse. El objetivo principal es entrenar un modelo de aprendizaje automático capaz de clasificar objetos en tiempo real, específicamente distinguir entre una "Billetera" y una "Laptop", desplegando finalmente el modelo directamente en el microcontrolador.

A lo largo de este taller, se abordan desde la configuración inicial del entorno en Arduino IDE y la captura de datos mediante la cámara, hasta el entrenamiento del modelo en la nube y su despliegue final. Las evidencias presentadas a continuación demuestran el correcto funcionamiento del flujo de trabajo, destacando el rendimiento del modelo y su capacidad de inferencia en el dispositivo.

---

## 1. Creación del Proyecto y Carga de Datos

<div style="background-color: #f8f9fa; border-left: 4px solid #4361ee; padding: 15px; margin-bottom: 20px;">
    <strong>Descripción:</strong> Se creó una cuenta y un nuevo proyecto en Edge Impulse. Posteriormente, se subieron las imágenes capturadas previamente por la cámara del XIAO ESP32S3. Las imágenes se organizaron en dos clases distintas: "Billetera" y "Laptop", utilizando la opción de subida por lotes para asignar automáticamente las etiquetas correspondientes a cada archivo.
</div>

<div align="center">
    <img src="./images/figura1.png" alt="Creación de proyecto y carga de datos" width="90%" style="border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
    <br>
    <em>Figura 1: Interfaz de creación del proyecto y flujo para añadir datos existentes.</em>
</div>

<br>

## 2. Verificación del Dataset

<div style="background-color: #f8f9fa; border-left: 4px solid #4361ee; padding: 15px; margin-bottom: 20px;">
    <strong>Descripción:</strong> Tras la carga exitosa de las 100 imágenes (50 por clase), se verificó la correcta recepción de los datos en la plataforma. Edge Impulse dividió automáticamente el conjunto de datos en un 81% para entrenamiento y un 19% para pruebas, asegurando una validación adecuada del modelo.
</div>

<div align="center">
    <img src="./images/figura2.png" alt="Verificación del Dataset" width="90%" style="border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
    <br>
    <em>Figura 2: Confirmación de subida y visualización del dataset dividido.</em>
</div>

<br>

## 3. Rendimiento del Modelo y Gráfico de Dispersión

<div style="background-color: #f8f9fa; border-left: 4px solid #4361ee; padding: 15px; margin-bottom: 20px;">
    <strong>Descripción:</strong> Se evaluó el rendimiento del modelo entrenado utilizando la versión cuantizada (int8). Los resultados muestran una precisión del 100% y una pérdida de 0.04. Además, se presentan dos gráficos de dispersión: el primero corresponde a una muestra aleatoria de la clasificación, y el segundo muestra la distribución general de todos los datos procesados, evidenciando una clara separación entre las clases "Billetera" y "Laptop".
</div>

<div align="center">
    <img src="./images/figura3.png" alt="Métricas y Gráfico de Dispersión General" width="100%" style="border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
    <br>
    <em>Figura 3: Matriz de confusión, métricas de rendimiento y gráfico de dispersión general de los datos.</em>
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

<br>

## 5. Visualización con SenseCraft

<div style="background-color: #f8f9fa; border-left: 4px solid #4361ee; padding: 15px; margin-bottom: 20px;">
    <strong>Descripción:</strong> Como alternativa de visualización, se utilizó la plataforma SenseCraft para cargar el modelo y probar la cámara en vivo. La interfaz muestra la vista previa de la cámara junto con barras de progreso que indican el porcentaje de confianza de la clasificación. Se observa claramente cómo el sistema identifica correctamente la "Billetera" y la "Laptop" en diferentes posiciones.
</div>

<div align="center">
    <img src="./images/figura5.png" alt="Visualización en SenseCraft" width="90%" style="border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">
    <br>
    <em>Figura 5: Interfaz de SenseCraft mostrando la clasificación en tiempo real de ambos objetos.</em>
</div>

<br>

---

<div align="center" style="background-color: #f1f1f1; padding: 15px; border-radius: 8px; color: #333;">
    <strong>Conclusión:</strong> El taller se completó exitosamente, logrando implementar un sistema de TinyML funcional en el XIAO ESP32S3 Sense. Las evidencias demuestran que el modelo es capaz de clasificar imágenes con alta precisión y baja latencia, validando el uso de Edge Impulse y Arduino IDE para proyectos de visión artificial en el borde.
</div>
