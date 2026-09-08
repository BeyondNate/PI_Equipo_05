# Evaluación – Matriz Morfológica VDI 2206 – Quitosano

## Instrucciones

| **Paso** | **Detalle**                                                                       |
| -------: | --------------------------------------------------------------------------------- |
|        1 | Matriz : revisa subfunciones y alternativas; lee 'Notas' para incompatibilidades. |
|        2 | Conceptos : combina 1 alternativa por subfunción (A, B, C).                       |
|        3 | Criterios : define pesos (suman 1,0) y justifícalos en tu proyecto.               |
|        4 | Puntajes (1-5) : valora cada concepto por criterio (5 = mejor).                   |
|        5 | Pugh : compara B y C contra A con +/0/-; revisa sumas.                            |
|        6 | Ponderado : calcula Total ponderado = SUMA(peso * puntaje).                       |
|        7 | Selecciona el(los) concepto(s) líder(es) y registra riesgos/ensayos a planificar. |

---

## Notas

| **Notas de interfaz / compatibilidad**                                                             |
| -------------------------------------------------------------------------------------------------- |
| El ESP32 incluye su propio módulo de Wifi. De seleccionarse no será necesario adquirir uno aparte. |
| La alimentación debe ser compatible con el ESP32 y los sensores.                                   |
| La bomba debe permitir una dosificación controlada del quitosano.                                  |
| El sistema de agitación debe poder ser controlado desde el ESP32.                                  |
| Los sensores de pH y turbidez requieren acondicionamiento adecuado de señal.                       |
| La solución debe mantener la portabilidad del dispositivo.                                         |
| El material de la carcasa debe ser adecuado para el entorno de uso.                                |

---

## Matriz

| **N.º** | **Función**                                | **Solución 1**         | **Solución 2**                      | **Solución 3**    | **Solución 4**            |
| ------: | ------------------------------------------ | ---------------------- | ----------------------------------- | ----------------- | ------------------------- |
|       1 | Fuente de energía del dispositivo portátil | Pilas recargables      | Batería Li 3.7/5 V 5000 mAh         | Conexión directa  |                           |
|       2 | Encendido del equipo                       | Interruptor deslizante | Interruptor basculante              | Pulsador          |                           |
|       3 | Acondicionar energía eléctrica de entrada  | Fuente switching       | LM7805                              | Buck DC-DC        |                           |
|       4 | Acondicionar energía para sensores         | LDO 3.3 V              | Step-down                           | Filtro + divisor  |                           |
|       5 | Acondicionar energía para comunicación     | Regulador 3.3 V        | Filtro + regulador                  | DC-DC aislado     |                           |
|       6 | Controlar el sistema                       | ESP32                  | Arduino + HC-05                     | ESP8266           |                           |
|       7 | Detectar turbidez                          | Sensor de turbidez     | Nefelométrico                       | LED + fotodiodo   |                           |
|       8 | Detectar pH                                | Electrodo de vidrio    | ISFET                               | Sonda de pH       |                           |
|       9 | Bombear quitosano                          | Bomba peristáltica     | Bomba dosificadora electromagnética | Bomba de jeringa  |                           |
|      10 | Controlar agitador                         | PWM                    | Relé + temporizador                 | Driver + encoder  |                           |
|      11 | Sujetar dispositivo/cajita                 | Brazo fijo             | Brazo articulado                    | Brazo telescópico |                           |
|      12 | Señal visual                               | LED simple             | LED RGB                             | Baliza            |                           |
|      13 | Señal sonora                               | Buzzer                 | Zumbador                            | Altavoz           |                           |
|      14 | Mostrar datos                              | LCD 16×2               | OLED                                | TFT               |                           |
|      15 | Transmitir datos                           | Wi-Fi                  | Bluetooth                           | GSM               | Wi-Fi integrado del ESP32 |
|      16 | Carcasa                                    | PLA                    | ABS                                 | PETG              |                           |
|      17 | Envase de quitosano                        | Vidrio borosilicato    | HDPE/PP                             | Sello hermético   |                           |

---

## Conceptos

| **Concepto**          | **Almacenar líquido**      | **Procesamiento y control de señales** | **Medir/Detectar**                                       | **Mecánica**                        | **Actuar**          | **Control**            | **Energía** |
| --------------------- | -------------------------- | -------------------------------------- | -------------------------------------------------------- | ----------------------------------- | ------------------- | ---------------------- | ----------- |
| **Concepto A (Base)** | Botella plástica HDPE o PP | ESP32                                  | Sensores: Turbidez salida analógica, pH con electrodo    | Bomba peristáltica                  | Control por PWM     | Interruptor deslizante | Fuente 5VDC |
| **Concepto B**        | Botella plástica HDPE o PP | ESP32                                  | Sensores: Turbidez salida analógica, pH con sonda        | Bomba peristáltica                  | Control por PWM     | ON/OFF                 | Fuente 5VDC |
| **Concepto C**        | Envase con sello hermético | ESP32                                  | Sensores: Nefelométrico de turbidez, pH de estado sólido | Bomba dosificadora electromagnética | Relé + temporizador | ON/OFF                 | Fuente 5VDC |

---

## Criterios

| **Criterio**              | **Peso** |
| ------------------------- | -------: |
| Precisión de dosificación |     0.25 |
| Costo                     |     0.15 |
| Complejidad técnica       |     0.15 |
| Consumo energético        |     0.15 |
| Mantenimiento e higiene   |     0.10 |
| Confiabilidad             |     0.10 |
| Portabilidad              |     0.10 |
| **Total**                 | **1.00** |

---

## Puntajes (1-5)

| **Criterio**              | **Solución 1** | **Solución 2** | **Solución 3** |
| ------------------------- | -------------: | -------------: | -------------: |
| Precisión de dosificación |              4 |              5 |              4 |
| Costo                     |              4 |              4 |              3 |
| Complejidad técnica       |              4 |              4 |              3 |
| Consumo energético        |              4 |              4 |              3 |
| Mantenimiento e higiene   |              4 |              4 |              3 |
| Confiabilidad             |              4 |              5 |              3 |
| Portabilidad              |              4 |              5 |              3 |

---

## Pugh

| **Criterio**              | **Base (A)** | **B vs A** | **C vs A** |
| ------------------------- | -----------: | ---------: | ---------: |
| Precisión de dosificación |            0 |          1 |          0 |
| Costo                     |            0 |          0 |         -1 |
| Complejidad técnica       |            0 |          0 |         -1 |
| Consumo energético        |            0 |          0 |         -1 |
| Mantenimiento e higiene   |            0 |          0 |         -1 |
| Confiabilidad             |            0 |          1 |         -1 |
| Portabilidad              |            0 |          1 |         -1 |
| **Suma**                  |              |      **3** |     **-6** |

---

## Ponderado

| **Criterio**              | **Peso** | **Concepto A (Base)** | **Concepto B** | **Concepto C** |           | **Peso*Concepto A (Base)** | **Peso*Concepto B** | **Peso*Concepto C** |
| ------------------------- | -------: | --------------------: | -------------: | -------------: | --------- | -------------------------: | ------------------: | ------------------: |
| Precisión de dosificación |     0.25 |                     4 |              5 |              4 |           |                          1 |                1.25 |                   1 |
| Costo                     |     0.15 |                     4 |              4 |              3 |           |                        0.6 |                 0.6 |                0.45 |
| Complejidad técnica       |     0.15 |                     4 |              4 |              3 |           |                        0.6 |                 0.6 |                0.45 |
| Consumo energético        |     0.15 |                     4 |              4 |              3 |           |                        0.6 |                 0.6 |                0.45 |
| Mantenimiento e higiene   |      0.1 |                     4 |              4 |              3 |           |                        0.4 |                 0.4 |                 0.3 |
| Confiabilidad             |      0.1 |                     4 |              5 |              3 |           |                        0.4 |                 0.5 |                 0.3 |
| Portabilidad              |      0.1 |                     4 |              5 |              3 |           |                        0.4 |                 0.5 |                 0.3 |
| **Total**                 |  **1.0** |                       |                |                | **TOTAL** |                      **4** |            **4.45** |            **3.25** |
