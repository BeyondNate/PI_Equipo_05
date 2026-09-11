## Matriz Morfológica

![Mi imagen](../FOTOS/mf03_v4.png)

## 📁 Link al CANVA:

[Canva_Matriz](https://canva.link/7j7wfllv1xer6x5)

### Solución preliminar 1 — Económica y sencilla

| Función                                | Elección                                     |
| -------------------------------------- | -------------------------------------------- |
| **T1. Fuente de energía**              | **Batería de litio 3.7/5 V 5000 mAh**        |
| **T2. Encendido**                      | **Interruptor deslizante**                   |
| **T3. Acondicionar energía eléctrica** | **Fuente conmutada (switching)**             |
| **T4. Energía para sensores**          | **Regulador LDO 5 V**                        |
| **T5. Energía para comunicación**      | **Regulador 5 V dedicado**                   |
| **T6. Controlar sistema**              | **Microcontrolador ESP32**                   |
| **T7. Sensar turbidez**                | **Sensor de turbidez 0–1000 NTU**            |
| **T8. Sensar pH**                      | **Sensor de pH con electrodo de vidrio**     |
| **T9. Sensar nivel de agua**           | **Sensor capacitivo sin contacto XKC-Y25**   |
| **T10. Transportar agua turbia**       | **Bomba de alimentación de agua sucia 12 V** |
| **T11. Dosificar quitosano**           | **Bomba peristáltica**                       |
| **T12. Posicionar agitador**           | **Motor NEMA 17 + Husillo T8**               |
| **T13. Controlar agitador**            | **Motor DC + PWM**                           |
| **T14. Sujetar dispositivo/cajita**    | **Brazo articulado**                         |
| **T15. Emitir señal sonora**           | **Buzzer**                                   |
| **T16. Mostrar información**           | **Pantalla LCD 16×2**                        |
| **T17. Transmitir datos**              | **Módulo de Bluetooth**                      |
| **T18. Carcasa**                       | **PLA**                                      |
| **T19. Envase de quitosano**           | **Plástico HDPE o PP**                       |

#### ¿Cómo sería?

Un sistema con **ESP32** que funciona con una batería de litio y permite medir la **turbidez, el pH y el nivel del agua**. Una bomba transportaría el agua hacia el reactor y una **bomba peristáltica** dosificaría el quitosano. El agitador utilizaría un motor controlado mediante **PWM**, mientras que la información se mostraría en una pantalla LCD y se transmitiría mediante Bluetooth.

* **Ventaja:** Es una propuesta relativamente sencilla y utiliza componentes accesibles.
* **Desventaja:** Tiene menos posibilidades de control y comunicación que las otras alternativas.

### Solución preliminar 2 — Equilibrada ⭐

| Función                                | Elección                                     |
| -------------------------------------- | -------------------------------------------- |
| **T1. Fuente de energía**              | **Conexión directa**                         |
| **T2. Encendido**                      | **Interruptor basculante (rocker)**          |
| **T3. Acondicionar energía eléctrica** | **Convertidor DC-DC tipo buck**              |
| **T4. Energía para sensores**          | **Módulo step-down regulado**                |
| **T5. Energía para comunicación**      | **Convertidor DC-DC aislado**                |
| **T6. Controlar sistema**              | **Microcontrolador ESP32**                   |
| **T7. Sensar turbidez**                | **Sensor nefelométrico de turbidez**         |
| **T8. Sensar pH**                      | **Sensor de pH con sonda**                   |
| **T9. Sensar nivel de agua**           | **Control eléctrico de nivel de agua/Boya**  |
| **T10. Transportar agua turbia**       | **Bomba de alimentación de agua sucia 12 V** |
| **T11. Dosificar quitosano**           | **Bomba dosificadora electromagnética**      |
| **T12. Posicionar agitador**           | **Actuador lineal 12 V DC**                  |
| **T13. Controlar agitador**            | **Relé + temporizador**                      |
| **T14. Sujetar dispositivo/cajita**    | **Brazo articulado**                         |
| **T15. Emitir señal sonora**           | **Zumbador**                                 |
| **T16. Mostrar información**           | **Pantalla OLED**                            |
| **T17. Transmitir datos**              | **WiFi + Bluetooth integrado en el ESP32**   |
| **T18. Carcasa**                       | **ABS**                                      |
| **T19. Envase de quitosano**           | **Plástico HDPE o PP**                       |

#### ¿Cómo sería?

Un sistema con **ESP32** que mide la **turbidez, el pH y el nivel del agua**. Una bomba transporta el agua hacia el reactor y otra se encarga de dosificar el quitosano. El **actuador lineal** permite posicionar el agitador y su funcionamiento se controla mediante un relé y temporizador. La información se muestra en una **pantalla OLED** y puede transmitirse mediante **WiFi o Bluetooth**.

* **Ventaja:** Ofrece un buen equilibrio entre funcionalidad, costo y complejidad, además de permitir un mejor control del sistema.
* **Desventaja:** Requiere más componentes y una implementación un poco más compleja que la primera propuesta.

### Solución preliminar 3 — Más robusta

| Función                                | Elección                                     |
| -------------------------------------- | -------------------------------------------- |
| **T1. Fuente de energía**              | **Conexión directa**                         |
| **T2. Encendido**                      | **Interruptor basculante**                   |
| **T3. Acondicionar energía eléctrica** | **Convertidor DC-DC tipo buck**              |
| **T4. Energía para sensores**          | **Módulo step-down regulado**                |
| **T5. Energía para comunicación**      | **Convertidor DC-DC aislado**                |
| **T6. Controlar sistema**              | **Microcontrolador ESP32**                   |
| **T7. Sensar turbidez**                | **Sensor nefelométrico de turbidez**         |
| **T8. Sensar pH**                      | **Sensor de pH de estado sólido (ISFET)**    |
| **T9. Sensar nivel de agua**           | **Sensor de nivel de agua vertical**         |
| **T10. Transportar agua turbia**       | **Bomba de alimentación de agua sucia 12 V** |
| **T11. Dosificar quitosano**           | **Bomba de jeringa automatizada**            |
| **T12. Posicionar agitador**           | **Servomotor lineal**                        |
| **T13. Controlar agitador**            | **Driver de motor con encoder**              |
| **T14. Sujetar dispositivo/cajita**    | **Brazo telescópico**                        |
| **T15. Emitir señal sonora**           | **Altavoz pequeño**                          |
| **T16. Mostrar información**           | **Pantalla TFT a color**                     |
| **T17. Transmitir datos**              | **WiFi + Bluetooth integrado en el ESP32**   |
| **T18. Carcasa**                       | **PETG**                                     |
| **T19. Envase de quitosano**           | **Envase con sello hermético**               |

#### ¿Cómo sería?

Una propuesta más robusta que utiliza sensores y componentes con mayores posibilidades de control. El sistema mediría **turbidez, pH y nivel de agua**, transportaría el agua al reactor y dosificaría el quitosano mediante una **bomba de jeringa automatizada**. El agitador podría posicionarse mediante un **servomotor lineal** y controlar su velocidad mediante un encoder. Además, tendría una **pantalla TFT** y un sistema de comunicación inalámbrica.

* **Ventaja:** Permite un mayor control y ofrece más posibilidades de monitoreo.
* **Desventaja:** Es la alternativa más compleja y costosa de implementar.

### Selección de la mejor solución 🏆

Después de comparar las tres propuestas, consideramos que la **Solución preliminar 2** es la más adecuada para nuestro proyecto. Esta alternativa presenta un buen equilibrio entre **funcionalidad, costo y facilidad de implementación**.

La Solución 2 permite integrar la medición de **turbidez, pH y nivel de agua**, además de controlar el transporte del agua, la dosificación del quitosano y el posicionamiento del agitador. También permite mostrar la información mediante una **pantalla OLED** y utilizar **WiFi o Bluetooth** para la comunicación.

A diferencia de la primera propuesta, ofrece mayores posibilidades de control y monitoreo; mientras que, frente a la tercera, evita utilizar demasiados componentes que aumentarían el costo y la complejidad.

Por ello, la **Solución preliminar 2** representa la alternativa más equilibrada y viable para desarrollar nuestro sistema automatizado de clarificación de agua. 💧⚙️


