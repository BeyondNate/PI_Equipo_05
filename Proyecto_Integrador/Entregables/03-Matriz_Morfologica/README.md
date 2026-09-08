## Matriz Morfológica

<img src="https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Entregables/03-Matriz_Morfologica/FOTOS/mf01_v2.png"/>

<img src="https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Entregables/03-Matriz_Morfologica/FOTOS/mf02_v2.png"/>

<img src="https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Entregables/03-Matriz_Morfologica/FOTOS/mf03_v2.png"/>

### Solución preliminar 1 — Económica y sencilla

| Función                                | Elección                                        |
| -------------------------------------- | ----------------------------------------------- |
| **T1. Fuente de energía**              | **T1.2 – Batería de litio 3.7/5 V 5000 mAh**    |
| **T2. Encendido**                      | **T2.1 – Interruptor deslizante**               |
| **T3. Acondicionar energía eléctrica** | **T3.3 – Convertidor DC-DC tipo buck**          |
| **T4. Energía para sensores**          | **T4.1 – Regulador LDO 3.3 V**                  |
| **T5. Energía para comunicación**      | **T5.1 – Regulador 3.3 V dedicado**             |
| **T6. Controlar sistema**              | **T6.1 – ESP32**                                |
| **T7. Detectar turbidez**              | **T7.1 – Sensor de turbidez 0–1000 NTU**        |
| **T8. Detectar pH**                    | **T8.1 – Sensor de pH con electrodo de vidrio** |
| **T9. Medir nivel del agua**           | **T9.3 – Sensor de nivel de agua vertical**     |
| **T10. Bombear quitosano**             | **T10.1 – Bomba peristáltica**                  |
| **T11. Controlar agitador**            | **T11.1 – Control por PWM**                     |
| **T12. Sujetar cajita de sensores**    | **T12.1 – Brazo fijo**                          |
| **T13. Señal visual**                  | **T13.1 – LED simple indicador**                |
| **T14. Señal sonora**                  | **T14.1 – Buzzer**                              |
| **T15. Mostrar información**           | **T15.1 – Pantalla LCD 16×2**                   |
| **T16. Transmitir datos**              | **T16.4 – WiFi**                                |
| **T17. Carcasa**                       | **T17.1 – PLA**                                 |
| **T18. Envase de quitosano**           | **T18.2 – Plástico HDPE o PP**                  |

#### ¿Cómo sería?

Un sistema con **ESP32**, alimentado por una batería de litio, que mide el **pH y la turbidez** mediante los sensores ubicados en la cajita. A partir de estas mediciones, calcula la dosis de quitosano, acciona una **bomba peristáltica** y controla el **agitador mediante PWM**. La información se mostraría mediante una **pantalla LCD**, LED y buzzer.

* **Ventaja:** Es una propuesta económica y relativamente sencilla de construir.
* **Desventaja:** El brazo fijo ofrece poca flexibilidad para ajustar la posición de los sensores.

### Solución preliminar 2 — Equilibrada ⭐

| Función                                | Elección                                             |
| -------------------------------------- | ---------------------------------------------------- |
| **T1. Fuente de energía**              | **T1.3 – Conexión directa**                          |
| **T2. Encendido**                      | **T2.2 – Interruptor basculante (rocker)**           |
| **T3. Acondicionar energía eléctrica** | **T3.3 – Convertidor DC-DC tipo buck**               |
| **T4. Energía para sensores**          | **T4.2 – Módulo step-down regulado**                 |
| **T5. Energía para comunicación**      | **T5.1 – Regulador 3.3 V dedicado**                  |
| **T6. Controlar sistema**              | **T6.1 – ESP32**                                     |
| **T7. Detectar turbidez**              | **T7.1 – Sensor de turbidez 0–1000 NTU**             |
| **T8. Detectar pH**                    | **T8.3 – Sensor de pH con sonda**                    |
| **T9. Medir nivel del agua**           | **T9.1 – Control eléctrico de nivel de agua / boya** |
| **T10. Bombear quitosano**             | **T10.1 – Bomba peristáltica**                       |
| **T11. Controlar agitador**            | **T11.1 – Control por PWM**                          |
| **T12. Sujetar cajita de sensores**    | **T12.2 – Brazo articulado**                         |
| **T13. Señal visual**                  | **T13.2 – LED RGB multicolor**                       |
| **T14. Señal sonora**                  | **T14.1 – Buzzer**                                   |
| **T15. Mostrar información**           | **T15.1 – Pantalla LCD 16×2**                        |
| **T16. Transmitir datos**              | **T16.4 – WiFi**                                     |
| **T17. Carcasa**                       | **T17.1 – PLA**                                      |
| **T18. Envase de quitosano**           | **T18.2 – Plástico HDPE o PP**                       |

#### ¿Cómo sería?

Un sistema con **ESP32** que mide pH, turbidez y nivel del agua, calcula la dosis de quitosano y controla una **bomba peristáltica** y un **agitador mediante PWM**. La cajita de sensores estaría sostenida por un **brazo articulado**, permitiendo ajustar su posición dentro del recipiente. Los resultados se mostrarían mediante una **pantalla LCD**, LED RGB y buzzer, mientras que los datos podrían transmitirse mediante **WiFi**.

* **Ventaja:** Presenta el mejor equilibrio entre precisión, costo, funcionalidad, confiabilidad y portabilidad.
* **Desventaja:** Requiere algunos componentes adicionales y una configuración ligeramente más compleja que la primera alternativa.

### Solución preliminar 3 — Más robusta

| Función                                | Elección                                         |
| -------------------------------------- | ------------------------------------------------ |
| **T1. Fuente de energía**              | **T1.3 – Conexión directa**                      |
| **T2. Encendido**                      | **T2.2 – Interruptor basculante**                |
| **T3. Acondicionar energía eléctrica** | **T3.1 – Fuente conmutada (switching)**          |
| **T4. Energía para sensores**          | **T4.2 – Módulo step-down regulado**             |
| **T5. Energía para comunicación**      | **T5.3 – Convertidor DC-DC aislado**             |
| **T6. Controlar sistema**              | **T6.1 – ESP32**                                 |
| **T7. Detectar turbidez**              | **T7.2 – Sensor nefelométrico de turbidez**      |
| **T8. Detectar pH**                    | **T8.2 – Sensor de pH de estado sólido (ISFET)** |
| **T9. Medir nivel del agua**           | **T9.2 – Sensor de nivel**                       |
| **T10. Bombear quitosano**             | **T10.2 – Bomba dosificadora electromagnética**  |
| **T11. Controlar agitador**            | **T11.3 – Driver de motor con encoder**          |
| **T12. Sujetar cajita de sensores**    | **T12.3 – Brazo telescópico**                    |
| **T13. Señal visual**                  | **T13.3 – Indicador luminoso tipo baliza**       |
| **T14. Señal sonora**                  | **T14.3 – Altavoz pequeño**                      |
| **T15. Mostrar información**           | **T15.3 – Pantalla TFT a color**                 |
| **T16. Transmitir datos**              | **T16.3 – Módulo GSM**                           |
| **T17. Carcasa**                       | **T17.2 – ABS**                                  |
| **T18. Envase de quitosano**           | **T18.3 – Envase con sello hermético**           |

#### ¿Cómo sería?

Una propuesta más robusta que utiliza un **ESP32**, sensores de mayor precisión y una **bomba dosificadora electromagnética**. El agitador contaría con un **driver con encoder** para mejorar su control, mientras que la cajita de sensores utilizaría un **brazo telescópico**. También incluiría una **pantalla TFT, baliza luminosa, altavoz y comunicación GSM**, junto con una carcasa de ABS.

* **Ventaja:** Ofrece mayor control, precisión y posibilidades de monitoreo.
* **Desventaja:** Es la alternativa más costosa y compleja de implementar.

### Selección de la mejor solución 🏆

Después de comparar las tres propuestas, consideramos que la **Solución preliminar 2** es la más adecuada para nuestro proyecto. La evaluación muestra que esta alternativa obtiene el **mayor puntaje**, destacando por su equilibrio entre **precisión, costo, funcionalidad, confiabilidad y portabilidad**.

Por ello, la **Solución preliminar 2** será la alternativa seleccionada para continuar con el desarrollo del sistema automatizado de dosificación de quitosano. 💧⚙️

