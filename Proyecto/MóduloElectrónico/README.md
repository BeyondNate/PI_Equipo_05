# ⚙️ Esquemático General del Sistema – ChitoSan

Este esquemático representa la distribución de los principales componentes electrónicos de **ChitoSan**, un sistema automatizado para la clarificación de agua mediante el uso de pectina de maracuyá y quitosano.

El diseño de la PCB principal está organizado en módulos independientes para facilitar la identificación de las conexiones, la alimentación y el control de los diferentes componentes.

### 🔌 Módulos del circuito

- **Módulo de Alimentación:** Recibe los 12 V de entrada y proporciona una salida regulada de 5 V para alimentar el ESP32 y los componentes electrónicos.
- **Control Principal:** Utiliza un ESP32 como unidad central, encargado de procesar las señales de los sensores y controlar los actuadores.
- **Módulo de Sensores:** Integra los módulos de medición de pH y turbidez para obtener información sobre las características del agua.
- **Interfaz de Usuario:** Incorpora una pantalla LCD 1602 mediante comunicación I2C y un buzzer para mostrar información y emitir alertas.
- **Control de Motor NEMA 17:** Emplea un driver TMC2209 para controlar el movimiento del motor paso a paso.
- **Bombas y Actuadores:** Incluye dos módulos de control para las bombas de agua mediante MOSFET IRLZ44N, resistencias y diodos de protección.
- **Soportes:** Contiene los puntos de montaje mecánico para la instalación de la PCB.

### 🛠️ Componentes externos a la PCB principal

Por motivos de espacio y optimización del diseño, algunos componentes se implementarán en una galleta (protoboard) independiente:

- **Bomba dosificadora peristáltica G328:** Encargada de suministrar el quitosano durante el proceso de tratamiento.
- **Módulo sensor de nivel de agua:** Permite detectar el nivel del líquido en el recipiente de medición y dosificación.

Estos elementos formarán parte del sistema general, pero no estarán integrados físicamente en la PCB principal.

### 🎯 Propósito del diseño

El objetivo es centralizar el control electrónico del sistema mediante el ESP32, integrando la alimentación, las mediciones y el accionamiento de los actuadores en una PCB organizada, mientras que los componentes externos se conectan de manera independiente.

> 💧 **ChitoSan – Tecnología, automatización y sostenibilidad al servicio del tratamiento del agua.**
