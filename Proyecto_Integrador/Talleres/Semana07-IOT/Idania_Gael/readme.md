# Actividad desarrollada por Gael e Idania.

## Ejemplo 01: Lectura de un Potenciómetro con ESP32

### Objetivo

Leer los valores de un potenciómetro conectado al ESP32, mejorar la estabilidad de las mediciones mediante un promediado de muestras y convertir los valores obtenidos por el ADC a voltaje.

### Desarrollo

El potenciómetro se conecta al **GPIO 34**, utilizando el ADC del ESP32. La lectura tiene una resolución de **12 bits**, por lo que el valor obtenido puede variar entre **0 y 4095**.

Para reducir pequeñas variaciones en las mediciones, se toman **32 muestras** y se calcula su promedio. Además, el valor obtenido se convierte a voltaje utilizando dos métodos:

- **Conversión lineal:** utiliza el valor de referencia de 3.3 V y el máximo del ADC.
- **Conversión calibrada:** utiliza `analogReadMilliVolts()`, considerando la calibración del ESP32.

### Código utilizado

```cpp
const int   POT_PIN      = 34;
const int   NUM_MUESTRAS = 32;
const float VREF         = 3.3;
const int   ADC_MAX      = 4095;

void setup() {
  Serial.begin(115200);
  analogReadResolution(12);
  analogSetPinAttenuation(POT_PIN, ADC_11db);

  Serial.println("Actividad 01: Potenciometro con promediado");
}

void loop() {
  long sumaADC = 0;
  long sumaMv  = 0;

  for (int i = 0; i < NUM_MUESTRAS; i++) {
    sumaADC += analogRead(POT_PIN);
    sumaMv  += analogReadMilliVolts(POT_PIN);
    delay(2);
  }

  float adcProm = sumaADC / (float)NUM_MUESTRAS;
  float vLineal = adcProm * VREF / ADC_MAX;
  float vCalib  = (sumaMv / (float)NUM_MUESTRAS) / 1000.0;

  Serial.print("ADC promedio: ");
  Serial.print(adcProm, 1);

  Serial.print(" | V (lineal): ");
  Serial.print(vLineal, 3);
  Serial.print(" V");

  Serial.print(" | V (calibrado): ");
  Serial.print(vCalib, 3);
  Serial.println(" V");

  delay(500);
}
```
### Interpretación

El promediado de las 32 muestras permite obtener una lectura más estable del potenciómetro. Al girarlo, el valor promedio del ADC aumenta o disminuye dependiendo de la posición.

La conversión a voltaje permite interpretar la lectura de una forma más directa. La diferencia entre el voltaje lineal y el calibrado se debe a que el primero utiliza una conversión teórica, mientras que analogReadMilliVolts() considera la calibración del ESP32.

> Espacio para colocar captura del Monitor Serie
> Resultados Actividad 01

### Análisis

La actividad permitió comprobar la lectura de una señal analógica utilizando el ESP32. El uso del promedio reduce las variaciones de las lecturas y permite obtener valores más consistentes. Además, la conversión a voltaje facilita la interpretación de los datos obtenidos.

## Ejemplo 02: Conexión a un Hostpot mediante WiFi con ESP32

### Objetivo

Crear una red WiFi utilizando el hotspot de un smartphone y conectar el ESP32 a esta red, mostrando en el Monitor Serie la dirección IP y otros parámetros de conexión.

### Desarrollo

Para establecer la conexión se utiliza la biblioteca WiFi.h. El ESP32 se configura en modo **estación** (WIFI_STA), permitiendo que se conecte a una red WiFi existente.

Una vez establecida la conexión, el programa obtiene y muestra:

- Dirección IP: identifica al ESP32 dentro de la red.
- Puerta de enlace: permite la comunicación con otras redes.
- Máscara de subred: determina el rango de la red.
- Dirección MAC: identificador de la interfaz WiFi.
- RSSI: indica la intensidad de la señal recibida.

El programa también comprueba periódicamente si la conexión continúa activa y, en caso de perderse, intenta conectarse nuevamente.

```cpp
#include <WiFi.h>

const char* WIFI_SSID     = "ajam";
const char* WIFI_PASSWORD = "Idania123";

void conectarWiFi() {
  Serial.printf("\nConectando a: %s\n", WIFI_SSID);

  WiFi.mode(WIFI_STA);
  WiFi.begin(WIFI_SSID, WIFI_PASSWORD);

  unsigned long inicio = millis();

  while (WiFi.status() != WL_CONNECTED &&
         millis() - inicio < 20000) {
    delay(500);
    Serial.print(".");
  }

  if (WiFi.status() == WL_CONNECTED) {
    Serial.println("\nConexion exitosa");

    Serial.print("Direccion IP asignada: ");
    Serial.println(WiFi.localIP());

    Serial.print("Puerta de enlace: ");
    Serial.println(WiFi.gatewayIP());

    Serial.print("Mascara de subred: ");
    Serial.println(WiFi.subnetMask());

    Serial.print("Direccion MAC: ");
    Serial.println(WiFi.macAddress());

    Serial.print("Intensidad (RSSI): ");
    Serial.print(WiFi.RSSI());
    Serial.println(" dBm");
  } else {
    Serial.println("\nNo se pudo conectar.");
  }
}

void setup() {
  Serial.begin(115200);
  delay(1000);
  conectarWiFi();
}

void loop() {
  if (WiFi.status() != WL_CONNECTED) {
    Serial.println("Conexion perdida, reintentando...");
    conectarWiFi();
  }

  delay(5000);
}
```
### Resultados obtenidos

| Parámetro         | Resultado           |
| ----------------- | ------------------- |
| Estado            | Conexión exitosa    |
| Dirección IP      | `10.28.25.252`      |
| Puerta de enlace  | `10.28.25.37`       |
| Máscara de subred | `255.255.255.0`     |
| Dirección MAC     | `40:22:D8:60:B2:B0` |
| RSSI inicial      | `-53 dBm`           |
| RSSI posterior    | `-40 dBm`           |
| RSSI final        | `-38 dBm`           |

> Espacio para colocar captura del Monitor Serie
> Resultados Actividad 02

### Interpretación y análisis

El ESP32 logró conectarse correctamente al hotspot del smartphone y recibió la dirección IP **10.28.25.252.**

La máscara 255.255.255.0 corresponde a una red /24, mientras que la puerta de enlace fue 10.28.25.37.

La intensidad de señal pasó de **-53 dBm a -38 dBm**. Como los valores de RSSI más cercanos a 0 representan una señal más fuerte, se observa una mejora en la intensidad de la conexión durante las mediciones.

El programa también permite detectar una posible pérdida de conexión y realizar nuevamente el proceso de conexión.

### Observación

Al iniciar el ESP32 aparecen algunos mensajes correspondientes al proceso de arranque y un mensaje relacionado con **Core dump data check failed**. Sin embargo, la ejecución continuó normalmente y el ESP32 logró conectarse al hotspot correctamente.
