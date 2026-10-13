
# 🤖 Implementación de Modelo en SenseCraft AI con Salida GPIO

![Plataforma](https://play-lh.googleusercontent.com/Lg_-OXz_8Ha3MXh-iZDA4YfaZjQ12kaQDyOHhCfeJHLWLj-To65S4LOF4MpxN7_SRwIny4YW_tJC7KT6Tgotbw)
![Hardware](https://files.seeedstudio.com/wiki/SeeedStudio-XIAO-ESP32S3/img/xiaoesp32s3sense.jpg)

## 📋 Descripción

Este proyecto documenta la implementación de un modelo de visión por computador en **SenseCraft AI**, las pruebas de reconocimiento realizadas por integrantes del equipo y la configuración de una **salida GPIO** que activa un LED cuando el modelo detecta la clase configurada.

## 🎯 Objetivos

- [x] Implementar otro modelo de SenseCraft AI.
- [x] Realizar pruebas del modelo con capturas de 2 a 3 integrantes del equipo.
- [x] Probar la salida GPIO con captura del ajuste y del LED encendido ante el reconocimiento.

## 👥 Integrantes del equipo

| Nombre | Rol |
|--------|-----|
| Brad Cardenas | Tester |
| Idania Parguay] | Tester |
| Marcelo Alarcon | Tester |

## 🧰 Materiales y herramientas

- Dispositivo: **[Ej: Grove Vision AI V2 / XIAO ESP32S3 Sense / reComputer]**
- Plataforma: [SenseCraft AI](https://sensecraft.seeed.cc/ai/)
- LED + resistencia de [220 Ω]
- Cables jumper y protoboard
- Cable USB-C
- Computador con navegador compatible (Chrome / Edge)

---

## 1️⃣ Implementación del modelo

### Modelo seleccionado

- **Nombre del modelo:** [Ej: Detección de personas / Reconocimiento de gestos / Detección de objetos]
- **Tipo:** [Clasificación / Detección de objetos / Pose]
- **Clases que reconoce:** [Ej: persona, mano, taza]
- **Motivo de elección:** [Breve justificación]

### Pasos realizados

1. Ingresar a [SenseCraft AI](https://sensecraft.seeed.cc/ai/) e iniciar sesión.
2. Conectar el dispositivo al computador mediante USB.
3. Ir a la sección **Pretrained Models** y seleccionar el modelo **[nombre del modelo]**.
4. Hacer clic en **Deploy Model** y elegir el dispositivo conectado.
5. Esperar a que finalice la carga del modelo en el dispositivo.
6. Verificar en la vista previa (*Preview*) que la cámara muestra las detecciones.

📸 **Captura de la implementación:**

![Implementación del modelo](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana09/Taller_TinyML_SenseCraft/assets/3.png)
![Implementación del modelo11](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana09/Taller_TinyML_SenseCraft/assets/2.png)
---

## 2️⃣ Pruebas del modelo

Se realizaron pruebas de reconocimiento con distintos integrantes del equipo para verificar el funcionamiento del modelo.

### 🧑 Integrante 1: Brad

![Prueba Integrante 1](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana09/Taller_TinyML_SenseCraft/assets/bradcino.jpeg)


### 🧑 Integrante 2: Idania

![Prueba Integrante 2](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana09/Taller_TinyML_SenseCraft/assets/idaniaa.jpeg)


### 🧑 Integrante 3: Marcelo

![Prueba Integrante 3](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana09/Taller_TinyML_SenseCraft/assets/mamacerlo.jpeg)


--- 

## 3️⃣ Prueba de salida GPIO

### Conexión del hardware

| Componente | Pin del dispositivo |
|------------|---------------------|
| LED (ánodo, + ) con resistencia | [GPIO X / Pin D#] |
| LED (cátodo, − ) | GND |



📸 **Captura del ajuste GPIO:**

![Configuración GPIO](images/06_configuracion_gpio.png)

### Resultado: LED encendido al reconocer la clase

Cuando el modelo reconoce **[clase]**, la salida GPIO se activa y el LED se enciende.


### 🧑 Integrante 1: Brad

![Prueba Integrante 1](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana09/Taller_TinyML_SenseCraft/assets/bradluz.jpeg)


### 🧑 Integrante 2: Idania

![Prueba Integrante 2](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana09/Taller_TinyML_SenseCraft/assets/idanialuz.jpeg)


### 🧑 Integrante 3: Marcelo

![Prueba Integrante 3](https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Talleres/Semana09/Taller_TinyML_SenseCraft/assets/marceloluz.jpeg)

---

## ✅ Resultados y conclusiones

- El modelo **[nombre del modelo]** fue implementado correctamente en el dispositivo.
- Se verificó su funcionamiento con **[3]** integrantes del equipo, con confianza entre **[85 % y 92 %]**.
- La salida GPIO se configuró exitosamente y el LED se enciende únicamente cuando se reconoce la clase **[clase]**.
- **Dificultades encontradas:** [Ej: ajustes de iluminación, umbral de confianza, conexión del dispositivo].
- **Posibles mejoras:** [Ej: entrenar un modelo propio, controlar otros actuadores, ajustar el umbral]
## 📚 Referencias

- [SenseCraft AI – Seeed Studio](https://sensecraft.seeed.cc/ai/)
- [Documentación de Seeed Studio Wiki](https://wiki.seeedstudio.com/)

---

<p align="center">Hecho con amor </p>
