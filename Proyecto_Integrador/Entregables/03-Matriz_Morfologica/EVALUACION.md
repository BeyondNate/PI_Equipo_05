# Evaluación – Matriz Morfológica VDI 2206 – Clarificación de agua con Quitosano

## Instrucciones

| **Paso** | **Detalle**                                                                                                                       |
| -------: | --------------------------------------------------------------------------------------------------------------------------------- |
|        1 | **Matriz:** revisar las subfunciones y alternativas disponibles; considerar las notas de interfaz y compatibilidad.               |
|        2 | **Conceptos:** combinar una alternativa por cada subfunción para formar los conceptos A, B y C.                                   |
|        3 | **Criterios:** definir los pesos de evaluación, cuya suma debe ser 1,0, y justificarlos según los requerimientos del proyecto.    |
|        4 | **Puntajes (1-5):** valorar cada concepto según cada criterio, donde 5 representa el mejor desempeño.                             |
|        5 | **Pugh:** comparar los conceptos B y C respecto al concepto A como referencia, utilizando +, 0 y -.                               |
|        6 | **Ponderado:** calcular el resultado mediante `Total ponderado = Σ (peso × puntaje)`.                                             |
|        7 | **Selección:** identificar el concepto líder y registrar los principales riesgos y ensayos que deberán realizarse posteriormente. |

---

## Notas de interfaz / compatibilidad

| **Notas**                                                                                                                                                     |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| El ESP32 incluye Wi-Fi y Bluetooth integrados, por lo que no es necesario adquirir módulos de comunicación adicionales cuando se seleccione esta alternativa. |
| La alimentación debe ser compatible con el ESP32, sensores, bomba y sistema de agitación.                                                                     |
| La bomba debe permitir una dosificación controlada de la mezcla de quitosano.                                                                                 |
| El sensor de nivel debe ser compatible con el recipiente y permitir detectar adecuadamente el nivel de agua.                                                  |
| El sistema de agitación debe poder ser controlado desde el ESP32.                                                                                             |
| Los sensores de pH y turbidez requieren un acondicionamiento adecuado de señal.                                                                               |
| La solución debe mantener la portabilidad del dispositivo.                                                                                                    |
| El material de la carcasa debe ser adecuado para el entorno de uso y proteger los componentes electrónicos.                                                   |
| El envase de quitosano debe permitir conservar y dosificar adecuadamente la mezcla preparada.                                                                 |

---

# Conceptos

Los conceptos se construyen combinando una alternativa de cada función de la matriz morfológica.

| **Concepto**          | **Almacenar líquido**      | **Procesamiento y control de señales** | **Medir/Detectar**                             | **Nivel de agua**                 | **Mecánica**                        | **Actuar**          | **Control**            | **Energía / Comunicación** |
| --------------------- | -------------------------- | -------------------------------------- | ---------------------------------------------- | --------------------------------- | ----------------------------------- | ------------------- | ---------------------- | -------------------------- |
| **Concepto A (Base)** | Botella plástica HDPE o PP | ESP32                                  | Turbidez salida analógica + pH con electrodo    | Sensor de nivel vertical          | Sensor de Nivel de Agua Vertical (Plástico)+ Bomba peristáltica                  | Control por PWM     | Interruptor deslizante | Fuente 5VDC / Wi-Fi        |
| **Concepto B**        | Botella plástica HDPE o PP | ESP32                                  | Turbidez salida analógica + pH con sonda        | Sensor de nivel                   | Control eléctrico de nivel de agua/Boya + Bomba peristáltica                  | Control por PWM     | Interruptor basculante | Fuente 5VDC / Wi-Fi        |
| **Concepto C**        | Envase con sello hermético | ESP32                                  | Nefelométrico de turbidez + pH de estado sólido | Control eléctrico de nivel / boya | Bomba dosificadora electromagnética | Relé + temporizador | Interruptor basculante | Fuente 5VDC / GSM          |


### Configuración de los conceptos

**Concepto A (Base):**

* T1.2 – Batería de litio 3.7/5 V 5000 mAh
* T2.1 – Interruptor deslizante
* T3.3 – Convertidor DC-DC tipo buck
* T4.1 – Regulador LDO 5 V
* T5.1 – Regulador 5 V dedicado
* T6.1 – ESP32
* T7.1 – Sensor de turbidez 0–1000 NTU
* T8.1 – Sensor de pH con electrodo de vidrio
* T9.3 – Sensor de nivel de agua vertical
* T10.1 – Bomba peristáltica
* T11.1 – Control por PWM
* T12.1 – Brazo fijo
* T13.1 – LED simple
* T14.1 – Buzzer
* T15.1 – Pantalla LCD 16 × 2
* T16.4 – Wi-Fi y Bluetooth integrado del ESP32
* T17.1 – PLA
* T18.2 – Plástico HDPE o PP

**Concepto B:**

* T1.3 – Conexión directa
* T2.2 – Interruptor basculante
* T3.3 – Convertidor DC-DC tipo buck
* T4.2 – Módulo step-down regulado
* T5.1 – Regulador 5 V dedicado
* T6.1 – ESP32
* T7.1 – Sensor de turbidez 0–1000 NTU
* T8.3 – Sensor de pH con sonda
* T9.1 – Control eléctrico de nivel de agua / boya
* T10.1 – Bomba peristáltica
* T11.1 – Control por PWM
* T12.2 – Brazo articulado
* T13.2 – LED RGB
* T14.1 – Buzzer
* T15 – Pantalla LCD 16 × 2
* T16.4 – Wi-Fi y Bluetooth integrado del ESP32
* T17.1 – PLA
* T18.2 – Plástico HDPE o PP

**Concepto C:**

* T1.3 – Conexión directa
* T2.2 – Interruptor basculante
* T3.1 – Fuente conmutada
* T4.2 – Módulo step-down regulado
* T5.3 – Convertidor DC-DC aislado
* T6.1 – ESP32
* T7.2 – Sensor nefelométrico de turbidez
* T8.2 – Sensor de pH de estado sólido (ISFET)
* T9.2 – Sensor de nivel
* T10.2 – Bomba dosificadora electromagnética
* T11.3 – Driver con retroalimentación mediante encoder
* T12.3 – Brazo telescópico
* T13.3 – Baliza
* T14.3 – Altavoz pequeño
* T15.3 – Pantalla TFT a color
* T16.3 – Módulo GSM
* T17.2 – ABS
* T18.3 – Envase con sello hermético

---

# Criterios de evaluación

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

# Puntajes (1-5)

| **Criterio**              | **Concepto A** | **Concepto B** | **Concepto C** |
| ------------------------- | -------------: | -------------: | -------------: |
| Precisión de dosificación |              4 |              5 |              4 |
| Costo                     |              4 |              4 |              3 |
| Complejidad técnica       |              4 |              4 |              3 |
| Consumo energético        |              4 |              4 |              3 |
| Mantenimiento e higiene   |              4 |              4 |              3 |
| Confiabilidad             |              4 |              5 |              3 |
| Portabilidad              |              4 |              5 |              3 |

> **Nota:** los puntajes deben validarse nuevamente después de incorporar el sensor de nivel, ya que esta nueva función forma parte de la configuración completa de cada concepto.

---

# Análisis de Pugh

Se toma el **Concepto A** como alternativa base de referencia.

| **Criterio**              | **Base (A)** | **B vs A** | **C vs A** |
| ------------------------- | -----------: | ---------: | ---------: |
| Precisión de dosificación |            0 |          1 |          0 |
| Costo                     |            0 |          0 |         -1 |
| Complejidad técnica       |            0 |          0 |         -1 |
| Consumo energético        |            0 |          0 |         -1 |
| Mantenimiento e higiene   |            0 |          0 |         -1 |
| Confiabilidad             |            0 |          1 |         -1 |
| Portabilidad              |            0 |          1 |         -1 |
| **Suma**                  |              |     **+3** |     **-6** |

### Interpretación

El **Concepto B** presenta una ventaja respecto al concepto base principalmente en precisión de dosificación, confiabilidad y portabilidad. En los demás criterios presenta un desempeño equivalente.

El **Concepto C** presenta un desempeño inferior al Concepto A en costo, complejidad técnica, consumo energético, mantenimiento e higiene, confiabilidad y portabilidad. Por ello, presenta una valoración global desfavorable frente a la alternativa base.

---

# Evaluación ponderada

| **Criterio**              | **Peso** | **Concepto A** | **Peso × A** | **Concepto B** | **Peso × B** | **Concepto C** | **Peso × C** |
| ------------------------- | -------: | -------------: | -----------: | -------------: | -----------: | -------------: | -----------: |
| Precisión de dosificación |     0.25 |              4 |         1.00 |              5 |         1.25 |              4 |         1.00 |
| Costo                     |     0.15 |              4 |         0.60 |              4 |         0.60 |              3 |         0.45 |
| Complejidad técnica       |     0.15 |              4 |         0.60 |              4 |         0.60 |              3 |         0.45 |
| Consumo energético        |     0.15 |              4 |         0.60 |              4 |         0.60 |              3 |         0.45 |
| Mantenimiento e higiene   |     0.10 |              4 |         0.40 |              4 |         0.40 |              3 |         0.30 |
| Confiabilidad             |     0.10 |              4 |         0.40 |              5 |         0.50 |              3 |         0.30 |
| Portabilidad              |     0.10 |              4 |         0.40 |              5 |         0.50 |              3 |         0.30 |
| **Total ponderado**       | **1.00** |                |     **4.00** |                |     **4.45** |                |     **3.25** |

---

# Selección del concepto

De acuerdo con la evaluación ponderada, el **Concepto B** obtiene el mayor resultado con un puntaje de **4.45**, seguido del Concepto A con **4.00** y del Concepto C con **3.25**.

Por lo tanto, el **Concepto B se selecciona como concepto líder** para continuar con el desarrollo del prototipo de clarificación de agua mediante quitosano.

La selección deberá ser posteriormente validada mediante pruebas experimentales que permitan comprobar principalmente:

1. La precisión de dosificación de la mezcla de quitosano.
2. La respuesta del sensor de turbidez antes y después del proceso de clarificación.
3. La medición de pH del agua.
4. El correcto funcionamiento del sensor de nivel.
5. La capacidad del sistema de agitación para mantener condiciones reproducibles.
6. El consumo energético del dispositivo durante una operación completa.
7. La confiabilidad del sistema de control y comunicación.
