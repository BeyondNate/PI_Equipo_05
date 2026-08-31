## Caja Negra

<img src="https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Entregables/02-Caja_negra_Esquema_funciones_Matriz_morfologica/Fotos/CAJA%20NEGRA%20v1.jpg"/>

La **Caja Negra** representa el sistema automatizado para la clarificación de agua mediante dosificación de quitosano. En ella se identifican las principales **entradas**, como el agua turbia, la mezcla de quitosano, la energía eléctrica, los litros a tratar y las señales de pH y turbidez. Como resultado, el sistema genera **agua clarificada, lodos sedimentados y datos sobre el proceso**, como las mediciones finales y las notificaciones mediante WiFi.

## Esquema de Funciones

<img src="https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Entregables/02-Caja_negra_Esquema_funciones_Matriz_morfologica/Fotos/ESQUEMA%20DE%20FUNCIONES%20v1.jpg"/>

El **Esquema de Funciones** descompone el sistema en las principales funciones que permiten realizar el proceso de forma automatizada. Incluye la **adquisición de las señales de pH y turbidez, el procesamiento de los datos, el cálculo del volumen de quitosano, el control de la bomba peristáltica y del agitador magnético**, además de la visualización y transmisión de los resultados. De esta manera, se muestra cómo interactúan las diferentes partes del sistema para lograr la clarificación del agua.

## Matriz Morfológica

<img src="https://github.com/BeyondNate/PI_Equipo_05/blob/main/Proyecto_Integrador/Entregables/02-Caja_negra_Esquema_funciones_Matriz_morfologica/Fotos/PI%20MATRIZ%20MORFOL%C3%93GICA%20v1.png"/>

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
| **T9. Bombear quitosano**              | **T9.1 – Bomba peristáltica**                   |
| **T10. Controlar agitador**            | **T10.1 – Control por PWM**                     |
| **T11. Sujetar cajita de sensores**    | **T11.1 – Brazo fijo**                          |
| **T12. Señal visual**                  | **T12.1 – LED simple indicador**                |
| **T13. Señal sonora**                  | **T13.1 – Buzzer**                              |
| **T14. Mostrar información**           | **T14.1 – Pantalla LCD 16×2**                   |
| **T15. Transmitir datos**              | **T15.1 – WiFi**                                |
| **T16. Carcasa**                       | **T16.1 – PLA**                                 |
| **T17. Envase de quitosano**           | **T17.2 – Plástico HDPE o PP**                  |

#### ¿Cómo sería?

Un sistema con **ESP32**, alimentado por una batería de litio, que mide el **pH y la turbidez** mediante las sondas ubicadas en la cajita de sensores. A partir de estas mediciones calcula la dosis de quitosano, acciona una **bomba peristáltica** y controla el **agitador mediante PWM**. La cajita estaría sostenida por un **brazo fijo**, mientras que el sistema mostraría la información mediante una **pantalla LCD**, LED y buzzer. La carcasa podría fabricarse en **PLA**.

- **Ventaja:** Es una propuesta económica y relativamente sencilla de construir.
- **Desventaja:** El brazo fijo ofrece poca flexibilidad para ajustar la posición de las sondas.

### Solución Preliminar 2 - Equilibrada

| Función                                | Elección                                     |
| -------------------------------------- | -------------------------------------------- |
| **T1. Fuente de energía**              | **T1.2 – Batería de litio 3.7/5 V 5000 mAh** |
| **T2. Encendido**                      | **T2.2 – Interruptor basculante (rocker)**   |
| **T3. Acondicionar energía eléctrica** | **T3.3 – Convertidor DC-DC tipo buck**       |
| **T4. Energía para sensores**          | **T4.2 – Módulo step-down regulado**         |
| **T5. Energía para comunicación**      | **T5.1 – Regulador 3.3 V dedicado**          |
| **T6. Controlar sistema**              | **T6.1 – ESP32**                             |
| **T7. Detectar turbidez**              | **T7.1 – Sensor de turbidez 0–1000 NTU**     |
| **T8. Detectar pH**                    | **T8.3 – Sensor de pH con sonda**            |
| **T9. Bombear quitosano**              | **T9.1 – Bomba peristáltica**                |
| **T10. Controlar agitador**            | **T10.1 – Control por PWM**                  |
| **T11. Sujetar cajita de sensores**    | **T11.2 – Brazo articulado**                 |
| **T12. Señal visual**                  | **T12.2 – LED RGB multicolor**               |
| **T13. Señal sonora**                  | **T13.1 – Buzzer**                           |
| **T14. Mostrar información**           | **T14.2 – Pantalla OLED**                    |
| **T15. Transmitir datos**              | **T15.1 – WiFi**                             |
| **T16. Carcasa**                       | **T16.3 – PETG**                             |
| **T17. Envase de quitosano**           | **T17.2 – Plástico HDPE o PP**               |

#### ¿Cómo sería?

Un sistema con **ESP32** y batería de litio que mide pH y turbidez, calcula la dosis de quitosano y controla una **bomba peristáltica** y un **agitador mediante PWM**. La cajita de sensores estaría sostenida por un **brazo articulado**, permitiendo ajustar mejor su posición dentro del recipiente. Los resultados se mostrarían mediante una **pantalla OLED**, LED RGB y buzzer, además de poder transmitirse mediante **WiFi**. La carcasa podría fabricarse en **PETG**.

- **Ventaja:** Presenta un buen equilibrio entre costo, funcionalidad y facilidad de construcción.
- **Desventaja:** El brazo articulado y algunos componentes adicionales aumentan ligeramente la complejidad del sistema.

### Solución Preliminar 3 - Más robusta

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
| **T9. Bombear quitosano**              | **T9.2 – Bomba dosificadora electromagnética**   |
| **T10. Controlar agitador**            | **T10.3 – Driver de motor con encoder**          |
| **T11. Sujetar cajita de sensores**    | **T11.3 – Brazo telescópico**                    |
| **T12. Señal visual**                  | **T12.3 – Indicador luminoso tipo baliza**       |
| **T13. Señal sonora**                  | **T13.3 – Altavoz pequeño**                      |
| **T14. Mostrar información**           | **T14.3 – Pantalla TFT a color**                 |
| **T15. Transmitir datos**              | **T15.3 – Módulo GSM**                           |
| **T16. Carcasa**                       | **T16.2 – ABS**                                  |
| **T17. Envase de quitosano**           | **T17.3 – Envase con sello hermético**           |


#### ¿Cómo sería?

Una propuesta más robusta que utiliza un **ESP32**, un sensor nefelométrico de turbidez y un sensor de pH de estado sólido. La dosificación se realizaría mediante una **bomba dosificadora electromagnética** y el agitador tendría un **driver con encoder** para un mayor control. La cajita de sensores estaría sostenida por un **brazo telescópico**, permitiendo regular su posición. También incluiría una **pantalla TFT, baliza luminosa, altavoz y comunicación GSM**, con una carcasa de **ABS**.

- **Ventaja:** Ofrece mayor control, precisión y posibilidades de monitoreo.
- **Desventaja:** Es la alternativa más costosa y compleja de implementar.

### Selección de la mejor solución 🏆

Después de comparar las tres propuestas, consideramos que la **Solución Preliminar 2** es la más adecuada para nuestro proyecto. Esta alternativa logra un buen equilibrio entre **costo, funcionalidad y facilidad de construcción**, utilizando componentes accesibles como el ESP32, sensores de pH y turbidez, una bomba peristáltica y un agitador controlado por PWM. Además, el **brazo articulado** permite ajustar mejor la posición de la cajita de sensores dentro del recipiente, haciendo que el sistema sea más práctico. ⚙️

A diferencia de la primera propuesta, ofrece mayor flexibilidad; y frente a la tercera, evita una complejidad y costo excesivos. Por ello, la Solución 2 representa la opción más **viable para construir y llevar a la realidad** nuestro sistema de dosificación automática de quitosano. 💧🌱
