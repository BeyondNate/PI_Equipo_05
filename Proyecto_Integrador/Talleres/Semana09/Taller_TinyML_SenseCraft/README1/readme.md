# Taller TinyML con SenseCraft AI: implementación de un modelo y prueba de salida GPIO

**Proyecto Integrador – Equipo 05 | Semana 09**

---

## Tabla de contenido

1. [Descripción](#1-descripción)
2. [Objetivos](#2-objetivos)
3. [Integrantes del equipo](#3-integrantes-del-equipo)
4. [Materiales y herramientas](#4-materiales-y-herramientas)
5. [Implementación del modelo](#5-implementación-del-modelo)
6. [Pruebas del modelo](#6-pruebas-del-modelo)
7. [Prueba de salida GPIO](#7-prueba-de-salida-gpio)
8. [Resultados y conclusiones](#8-resultados-y-conclusiones)
9. [Referencias](#9-referencias)

---

## 1. Descripción

Este documento presenta la implementación de un modelo de visión por computador en la plataforma **SenseCraft AI**, las pruebas de reconocimiento realizadas por los integrantes del equipo y la configuración de una **salida GPIO** que enciende el LED integrado del dispositivo cuando el modelo detecta la clase configurada.

## 2. Objetivos

- Implementar otro modelo de SenseCraft AI.
- Realizar pruebas del modelo con capturas de pantalla de 2 a 3 integrantes del equipo.
- Probar la salida GPIO, evidenciando el ajuste y el LED encendido ante el reconocimiento correspondiente.

## 3. Integrantes del equipo

| Nombre | Rol | Participación |
|--------|-----|---------------|
| Brad Cardenas | Tester | Participación en las pruebas del modelo |
| Idania Parhuay | Tester | Implementación del modelo preestablecido de SenseCraft AI |
| Marcelo Alarcon | Tester | Participación en las pruebas del modelo  |

## 4. Materiales y herramientas

| Elemento | Detalle |
|----------|---------|
| Dispositivo | Seeed Studio XIAO ESP32S3 Sense |
| Cámara | Módulo de cámara incluido en la placa de expansión Sense (conexión tipo módulo) |
| Indicador de salida | LED integrado en la placa XIAO ESP32S3 |
| Plataforma | [SenseCraft AI](https://sensecraft.seeed.cc/ai/) |
| Alimentación y datos | Cable USB-C |

---

## 5. Implementación del modelo

### 5.1. Modelo seleccionado

| Característica | Detalle |
|----------------|---------|
| Nombre del modelo | Swift-YOLO |
| Categoría del modelo | Humano y Comportamiento |
| Tipo | Detección de género |
| Clases que reconoce | male / female |

### 5.2. Procedimiento

1. Ingresar a [SenseCraft AI](https://sensecraft.seeed.cc/ai/) e iniciar sesión.
2. Conectar el dispositivo al computador mediante USB.
3. Ir a la sección **Pretrained Models** y seleccionar el modelo **Detección de género**.
4. Hacer clic en **Deploy Model** y elegir el dispositivo conectado.
5. Esperar a que finalice la carga del modelo en el dispositivo.
6. Verificar en la vista previa que la cámara muestra las detecciones.

### 5.3. Evidencia de la implementación del modelo de detección

<img width="1242" height="717" alt="image" src="https://github.com/user-attachments/assets/890e5943-f9a4-4121-a595-ab44d4d7affa" />

**Figura 1.** Evidencia de Idania Parhuay en comparación con Leonel Urbano: implementación del modelo de detección de género en SenseCraft AI y verificación de su funcionamiento en la vista previa del dispositivo.

En esta imagen, Idania se encargó de implementar el modelo preestablecido de SenseCraft AI en el dispositivo XIAO ESP32S3 Sense. En la vista previa, el modelo reconoció a Idania Parhuay en la clase female con una confianza del 83 % y a Leonel Urbano en la clase male con una confianza del 76 %. La confianza es el porcentaje de certeza con el que el modelo asigna cada detección a su clase, y solo se muestran las detecciones que superan el umbral configurado (46 %). Con ello se comprueba que el modelo detecta correctamente ambas clases.

## 6. Pruebas del modelo
 
Se realizaron pruebas de reconocimiento con los otros dos integrantes del equipo, Brad Cardenas y Marcelo Alarcon, para verificar el funcionamiento del modelo.
 
### 6.1. Prueba con Brad Cardenas
 
<p align="center">
  <img src="https://github.com/user-attachments/assets/c68c9659-1222-47b0-ae9b-2bb61ce7182e" alt="Figura 2a" width="400">
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/7f7fe97e-3d83-4f48-a224-781c2fec6a35" alt="Figura 2b" width="400">
</p>
<p align="center"><b>Figura 2.</b> Evidencia de Brad Cardenas: prueba de reconocimiento del modelo de detección de género.</p>

En estas imágenes, Brad se encargó de probar el modelo de detección de género con la cámara del dispositivo XIAO ESP32S3 Sense. En la vista previa, el modelo reconoció a Brad en la clase <i>male</i> con una confianza del [86 %] por lo que se comprueba que el modelo detecta correctamente la clase <i>male</i>.
 
### 6.2. Prueba con Marcelo Alarcon
 
<p align="center">
  <img src="https://github.com/user-attachments/assets/438adc18-fd31-4253-a433-82b32cd7f34f" alt="Figura 3a" width="400">
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/931b7396-51d3-417b-bf04-07280026f8c2" alt="Figura 3b" width="400">
</p>
<p align="center"><b>Figura 3.</b> Evidencia de Marcelo Alarcon: prueba de reconocimiento del modelo de detección de género.</p>

En estas imágenes, Marcelo se encargó de probar el modelo de detección de género con la cámara del dispositivo XIAO ESP32S3 Sense. En la vista previa, el modelo reconoció a Marcelo en la clase <i>male</i> con una confianza del [88%] por lo que se comprueba que el modelo detecta correctamente la clase <i>male</i>.
 
---
 
## 7. Prueba de salida GPIO
 
### 7.1. Hardware utilizado
 
La placa XIAO ESP32S3 Sense incluye un LED integrado, por lo que no fue necesario realizar conexiones externas (LED, resistencia, protoboard ni cables jumper). La salida GPIO configurada en SenseCraft AI controla directamente este LED.
 
### 7.2. Configuración de la salida GPIO
 
<p align="center">
  <img src="https://github.com/user-attachments/assets/1526a5b4-4325-45fe-adc4-242467b78c6a" alt="Figura 4" width="400">
</p>
<p align="center"><b>Figura 4.</b> Evidencia de Idania Parhuay: configuración de la salida GPIO en SenseCraft AI.</p>

En esta imagen, Idania se encargó de configurar la salida GPIO en SenseCraft AI para que el LED integrado del dispositivo se active cuando el modelo reconoce la clase <i>male</i>.
 
<p align="center">
  <img src="https://github.com/user-attachments/assets/3859547d-7f89-4655-a7c5-824307c5d84d" />
" alt="Figura 5" width="400">
</p>
<p align="center"><b>Figura 5.</b> :Implementación del GPIO en la clase male para el encendido del LED.</p>


### 7.3. Resultado: LED encendido al reconocer la clase male
 
Cuando el modelo reconoce la clase *<i>male</i>*, la salida GPIO se activa y el LED se enciende. Cuando reconoce la clase *<i>female</i>*, el LED permanece apagado.
 
<p align="center">
  <img src="https://github.com/user-attachments/assets/95dba4b7-1435-478b-abc5-1b945c71a0b7" alt="Figura 6a" width="400">
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/9ba9263d-b884-47a7-afbe-41180e5713ad" alt="Figura 6b" width="400">
</p>
<p align="center"><b>Figura 6.</b> Evidencia de Brad Cardenas: LED encendido ante el reconocimiento de la clase male.</p>

En estas imágenes, Brad se encargó de verificar el encendido del LED integrado del dispositivo cuando el modelo lo reconoció en la clase <i>male</i> con una confianza del [91%]. Con ello se comprueba que la salida GPIO responde correctamente a la detección configurada.

 
<p align="center">
  <img src="https://github.com/user-attachments/assets/44eeb6a0-4bbf-4fb7-9325-eb582499d5ff" alt="Figura 7a" width="400">
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/b717ab5a-dc96-47d5-8e4c-c88744b4a810" alt="Figura 7b" width="400">
</p>
<p align="center"><b>Figura 7.</b> Evidencia de Idania Parhuay: LED apagado ante el reconocimiento de la clase female.</p>

En estas imágenes, Idania se encargó de verificar que el LED integrado del dispositivo permanece apagado cuando el modelo la reconoció en la clase <i>female</i> con una confianza del [83 %]. Con ello se comprueba que la salida GPIO se activa únicamente ante la clase <i>male</i>.

 
<p align="center">
  <img src="https://github.com/user-attachments/assets/65db4e81-bb87-4701-ab64-266056b01e15" alt="Figura 8" width="400">
</p>
<p align="center"><b>Figura 8.</b> Evidencia de Marcelo Alarcon: LED encendido ante el reconocimiento de la clase male.</p>

En esta imagen, Marcelo se encargó de verificar el encendido del LED integrado del dispositivo cuando el modelo lo reconoció en la clase <i>male</i> con una confianza del [85 %]. Con ello se comprueba que la salida GPIO responde correctamente a la detección configurada.
 
---

## 8. Resultados y conclusiones

- El modelo de detección de género fue implementado correctamente en el dispositivo.
- Se verificó su funcionamiento con **[3]** integrantes del equipo, con niveles de confianza entre **[85 %]** y **[92 %]**.
- La salida GPIO se configuró correctamente y el LED se enciende únicamente cuando se reconoce la clase male.
- **Dificultades encontradas:** [Ej.: condiciones de iluminación, umbral de confianza, conexión del dispositivo].
- **Posibles mejoras:** [Ej.: entrenar un modelo propio, controlar otros actuadores, ajustar el umbral de confianza].

