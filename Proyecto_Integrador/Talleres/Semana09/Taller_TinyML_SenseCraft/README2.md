# README 2 – Modelo propio del grupo

## Integrantes
- Leonel Urbano
- Gael Milla

## Objetivo del modelo
Clasificar imágenes en tiempo real usando la XIAO ESP32S3 Sense y SenseCraft AI.

- Clases: lentes y celulares.

## Datos
- Origen: [tomados en tiempo real]
- Cantidad de imágenes: [N] de lentes y [N] de celulares
- Archivo: `datos.zip`

## Entrenamiento
- Parámetros usados: [Para lentes, 138 fotos de diferentes lentes en diferentes angulos. Para celulares, 138 fotos de diferentes celulares, en diferentes angulos]
- Confianza: [50%]
- Captura: ![](img/entrenamiento.png)

## Vista previa y despliegue
- Vista previa del modelo: ![](img/preview.png)
- Despliegue en la placa: ![](img/deploy.png)

## Salida GPIO
- Clase que activa el LED: [lentes]
- Captura del ajuste: ![](img/gpio_ajuste.png)

| Integrante | Captura | Resultado |
|---|---|---|
| [Leonel Urbano] | ![](img/gpio1.png) | LED encendido al mostrar [objeto] |
| [Gael Milla] | ![](img/gpio2.png) | LED encendido al mostrar [objeto] |

## Archivos del repositorio
- `datos.zip`
- Modelo exportado: `[nombre del archivo]`
