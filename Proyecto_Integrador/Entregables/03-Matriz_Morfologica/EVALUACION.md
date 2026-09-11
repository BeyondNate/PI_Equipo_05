# Evaluación – Matriz Morfológica VDI 2206 – Clarificación de agua con Quitosano

## Instrucciones

| **Paso** | **Detalle** |
|------|---------|
| 1 | Matriz : revisa subfunciones y alternativas; lee 'Notas' para incompatibilidades. |
| 2 | Conceptos : representa las tres soluciones preliminares obtenidas de la matriz morfológica. |
| 3 | Criterios : define pesos (suman 1,0) y justifícalos en tu proyecto. |
| 4 | Puntajes (1-5) : valora cada concepto por criterio (5 = mejor). |
| 5 | Pugh : compara soluciones preliminares B y C contra A con +/0/-; revisa sumas. |
| 6 | Ponderado : calcula Total ponderado = SUMA(peso * puntaje). |
| 7 | Selecciona el(los) concepto(s) líder(es) y registra riesgos/ensayos a planificar. |

---

## Notas de interfaz / compatibilidad

| **Notas de interfaz** |
|------------------------------------|
| El ESP32 integra WiFi y Bluetooth, por lo que no requiere un módulo externo para estas funciones. |
| La alimentación debe ser compatible con el ESP32, sensores, bombas y actuadores. |
| Los sensores de turbidez, pH y nivel de agua requieren una conexión y acondicionamiento adecuados. |
| La bomba debe permitir una dosificación controlada de la solución de quitosano. |
| El sistema debe permitir controlar el posicionamiento y giro del agitador. |
| La estructura debe permitir sujetar correctamente la cajita de sensores. |
| La carcasa debe proteger los componentes electrónicos del contacto con el agua. |
| El sistema debe mantener una implementación viable para el prototipo. |
---

# Matriz
| N.º | Función | Solución 1 | Solución 2 | Solución 3 |
|-----:|---------|------------|------------|------------|
| 1 | Fuente de energía del dispositivo | Batería de litio 3.7/5 V 5000 mAh | Conexión directa | ... |
| 2 | Encendido del equipo | Interruptor deslizante | Interruptor basculante | ... |
| 3 | Acondicionar energía eléctrica de entrada | Fuente conmutada | — | Convertidor DC-DC tipo buck |
| 4 | Acondicionar energía para sensores | Regulador LDO 5 V | Módulo step-down regulado | ... |
| 5 | Acondicionar energía para módulo de comunicación | Regulador 5 V dedicado | — | Convertidor DC-DC aislado |
| 6 | Controlar el sistema | ESP32 | — | — |
| 7 | Sensar turbidez del agua | Sensor de turbidez 0–1000 NTU | Sensor nefelométrico | ... |
| 8 | Sensar pH del agua | Electrodo de vidrio | ISFET | Sonda de pH |
| 9 | Sensar nivel de agua | Sensor capacitivo XKC-Y25 | Boya/control eléctrico | Sensor vertical |
| 10 | Transportar agua turbia al reactor | Bomba de alimentación 12 V | — | — |
| 11 | Dosificar solución de quitosano | Bomba peristáltica | Bomba electromagnética | Bomba de jeringa |
| 12 | Posicionar agitador | NEMA 17 + Husillo T8 | Actuador lineal 12 V | Servomotor lineal |
| 13 | Controlar velocidad y tiempo del agitador | Motor DC + PWM | Relé + temporizador | Driver + encoder |
| 14 | Sujetar dispositivo/cajita | — | Brazo articulado | Brazo telescópico |
| 15 | Emitir señal sonora | Buzzer | Zumbador | Altavoz pequeño |
| 16 | Mostrar turbidez, pH y dosis | LCD 16×2 | OLED | TFT |
| 17 | Transmitir datos inalámbricamente | — | Bluetooth | WiFi + Bluetooth del ESP32 |
| 18 | Carcasa | PLA | ABS | PETG |
| 19 | Envase de mezcla de quitosano | — | HDPE/PP | Envase con sello hermético |

# Conceptos

| Concepto | Almacenar líquido | Procesamiento y control de señales | Medir/Detectar | Mecánica | Actuar | Control | Energía |
|----------|-------------------|------------------------------------|----------------|----------|--------|---------|---------|
| Concepto A (Base) | Botella plástica HDPE o PP | ESP32 | Sensores: Turbidez salida analógica, pH con electrodo | Bomba alimentación agua sucia + Bomba de jeringa automatizada | Driver de motor con retroalimentación de velocidad (encoder) | Interruptor deslizante | Fuente 5VDC |
| Concepto B | Botella plástica HDPE o PP | ESP32 | Sensores: Turbidez salida analógica, pH con sonda | Bomba alimentación agua sucia + Bomba peristáltica | Motor DC (CC) + PWM | ON/OFF | Fuente 5VDC |
| Concepto C | Envase con sello hermético | ESP32 | Sensores: Nefelométrico de turbidez, pH de estado sólido | Bomba dosificadora electromagnética | Relé + temporizador | ON/OFF | Fuente 5VDC |




---

# Criterios de evaluación

| **Criterio** | **Peso** |
|----------|------|
| Precisión de dosificación | 0.20 |
| Costo | 0.15 |
| Complejidad técnica | 0.15 |
| Control del proceso | 0.15 |
| Consumo energético | 0.10 |
| Mantenimiento e higiene | 0.10 |
| Confiabilidad | 0.10 |
| Portabilidad | 0.05 |
| Total | 1.00 |

---

# Puntajes (1-5)

| **Criterio** | **Solución 1 (A)** | **Solución 2 (B)** | **Solución 3 (C)** |
|----------|------------|-----------------|------------|
| Precisión de dosificación | 4 | 5 | 4 |
| Costo | 4 | 4 | 3 |
| Complejidad técnica | 4 | 4 | 3 |
| Control del proceso | 4 | 4 | 4 |
| Consumo energético | 4 | 4 | 3 |
| Mantenimiento e higiene | 4 | 4 | 3 |
| Confiabilidad | 4 | 5 | 3 |
| Portabilidad | 4 | 5 | 3 |

---

# Análisis de Pugh

Se toma el **Concepto A** como alternativa base de referencia.

| *Criterio** | **Base (A)** | **B vs A** | **C vs A** |
|----------|----------|--------|--------|
| Precisión de dosificación | 0 | +1 | 0 |
| Costo | 0 | 0 | -1 |
| Complejidad técnica | 0 | 0 | -1 |
| Control del proceso | 0 | 0 | 0 |
| Consumo energético | 0 | 0 | -1 |
| Mantenimiento e higiene | 0 | 0 | -1 |
| Confiabilidad | 0 | +1 | -1 |
| Portabilidad | 0 | +1 | -1 |
| Suma | — | +3 | -6 |

---

# Evaluación ponderada

| **Criterio** | **Peso** | **Concepto A (Base)** | **Concepto B** | **Concepto C** | **Peso x Concepto A (Base)** | **Peso x Concepto B** | **Peso x Concepto C** |
|----------|------|-------------------|------------|------------|-------------------------|-----------------|-----------------|
| Precisión de dosificación | 0.2 | 4 | 5 | 4 | 0.8 | 1 | 0.8 |
| Costo | 0.15 | 4 | 4 | 3 | 0.6 | 0.6 | 0.45 |
| Complejidad técnica | 0.15 | 4 | 4 | 3 | 0.6 | 0.6 | 0.45 |
| Control del proceso | 0.15 | 4 | 4 | 4 | 0.6 | 0.6 | 0.6 |
| Consumo energético | 0.1 | 4 | 4 | 3 | 0.4 | 0.4 | 0.3 |
| Mantenimiento e higiene | 0.1 | 4 | 4 | 3 | 0.4 | 0.4 | 0.3 |
| Confiabilidad | 0.1 | 4 | 5 | 3 | 0.4 | 0.5 | 0.3 |
| Portabilidad | 0.05 | 4 | 5 | 3 | 0.2 | 0.25 | 0.15 |
| Total | 1 | | | | 4 | 4.35 | 3.35 |
| TOTAL | | | | | 4 | 4.35 | 3.35 |

---

# Selección del concepto

De acuerdo con la evaluación ponderada, el **Concepto B** obtiene el mayor resultado con un puntaje de **4.35**, seguido del Concepto A con **4.00** y del Concepto C con **3.35**.

Por lo tanto, el **Concepto B se selecciona como concepto líder** para continuar con el desarrollo del prototipo de clarificación de agua mediante quitosano.

La selección deberá ser posteriormente validada mediante pruebas experimentales que permitan comprobar principalmente:

1. La precisión de dosificación de la mezcla de quitosano.
2. La respuesta del sensor de turbidez antes y después del proceso de clarificación.
3. La medición de pH del agua.
4. El correcto funcionamiento de la bomba de alimentación de agua sucia.
5. La capacidad del sistema de agitación para mantener condiciones reproducibles.
6. El consumo energético del dispositivo durante una operación completa.
7. La confiabilidad del sistema de control y comunicación.
