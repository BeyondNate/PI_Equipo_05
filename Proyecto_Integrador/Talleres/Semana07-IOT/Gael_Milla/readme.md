# Taller de Internet de las Cosas (IoT) con ESP32

**Curso:** Proyectos de Ingeniería
**Universidad:** Universidad Peruana Cayetano Heredia (UPCH)

**Estudiante:** Gael Valentino Milla Fasabi
**Fecha:** 01/10/2026

---

## Introducción

En este taller se trabajó con una **ESP32 WROOM** para comprender el funcionamiento básico de un sistema IoT. Las actividades abarcaron la lectura de señales analógicas, la conexión a una red Wi-Fi, el envío de información a plataformas en la nube y el control remoto de un actuador.

Todo el trabajo se desarrolló utilizando el **Arduino IDE**.

## Materiales utilizados

- ESP32 WROOM (Dev Kit)
- Potenciómetro
- Sensor del kit Keystudio 48 en 1 _(indicar cuál: LDR, LM35 u otro)_
- LED y resistencia de 220 Ω
- Protoboard, cables y multímetro
- Cable USB con transmisión de datos
- Celular con hotspot en banda de 2.4 GHz

## Organización del repositorio

```
Taller_IoT_ESP32/
├── Actividad01_Potenciometro_Promedio/
├── Actividad02_WiFi_Hotspot/
├── Actividad03_ThingSpeak/
├── Actividad03_ArduinoCloud/
├── Actividad04_.../
├── Actividad05_LED_ArduinoCloud/
├── images/
└── README.md
```

> Las capturas se guardan en la carpeta `images/`. Debajo de cada actividad aparece el espacio para insertarlas, con el nombre de archivo sugerido.

---

## Actividad 01: Potenciómetro con promediado y conversión a voltaje

### Objetivo

Leer un potenciómetro conectado al ESP32, reducir las variaciones de la medición mediante un promedio y convertir el resultado del ADC a voltaje.

### Conexión

Conecté el potenciómetro con GND a tierra, VCC a los 3.3 V de la placa y la señal (SIG) al **GPIO34**. Elegí ese pin a propósito: pertenece al ADC1, que sigue funcionando con normalidad cuando el Wi-Fi está activo, algo que iba a necesitar en las actividades siguientes.

![Montaje del potenciómetro con el ESP32](images/act01_montaje.jpg)
*Figura 1. Montaje del potenciómetro en la protoboard, conectado al GPIO34.*

### Desarrollo

El ADC del ESP32 trabaja con una resolución de 12 bits, por lo que las lecturas se encuentran entre 0 y 4095. Para obtener una señal más estable se tomaron **32 muestras** y se calculó su promedio.

El voltaje se obtuvo mediante una conversión lineal y mediante `analogReadMilliVolts()`, que utiliza la calibración disponible en el ESP32.

### Código

**Actividad01_Potenciometro_Promedio.ino**

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

### Resultados

![Compilación exitosa de la Actividad 01](images/act01_compilacion.png)
*Figura 2. Resultado de la compilación en el Arduino IDE.*

![Monitor serie con las lecturas del potenciómetro](images/act01_monitor_serie.png)
*Figura 3. Monitor serie con el ADC promedio y el voltaje lineal y calibrado en distintas posiciones del potenciómetro.*

### Interpretación

La conexión se realizó correctamente y el ESP32 recibió una dirección IP dentro de la red creada por el celular. Los valores de máscara y puerta de enlace permiten identificar la configuración de la red local, mientras que el RSSI sirve para observar la intensidad de la señal. Con esta actividad se comprobó que el ESP32 puede integrarse a una red Wi-Fi y mantener la conexión mediante un mecanismo de reconexión.

### Observaciones

- Al iniciar el ESP32 aparecen algunos mensajes del proceso de arranque, entre ellos uno que dice _"Core dump data check failed"_. Aun así, la ejecución continuó con normalidad y la placa se conectó al hotspot sin inconvenientes. Por lo que entiendo, ese aviso es informativo y suele aparecer cuando no hay ningún registro de fallos guardado en la memoria de la placa.
- Al subir el código me apareció el error _"Wrong boot mode detected (0x13)"_. Lo solucioné manteniendo presionado el botón **BOOT** mientras el IDE mostraba "Connecting..." y soltándolo cuando comenzó la escritura.

---

## Actividad 03: Envío de datos del potenciómetro a la nube

### Objetivo

Enviar a la nube los valores obtenidos del potenciómetro y observar su variación mediante **ThingSpeak** y **Arduino Cloud**.

### 3.1 ThingSpeak

Se creó un canal con dos campos: el voltaje (Field 1) y el valor del ADC (Field 2). El ESP32 se conecta al Wi-Fi y, cada 20 segundos, arma una petición HTTP con la *Write API Key* del canal y los valores medidos. El intervalo no es casual: la cuenta gratuita de ThingSpeak exige al menos 15 segundos entre envíos, así que dejé un pequeño margen de seguridad.

![Configuración del canal en ThingSpeak](images/act03_ts_canal.png)
*Figura 6. Canal de ThingSpeak con sus campos configurados.*

![Monitor serie enviando datos a ThingSpeak](images/act03_ts_serial.png)
*Figura 7. Monitor serie confirmando cada envío (código HTTP 200 y número de entrada).*

![Gráficas en tiempo real en ThingSpeak](images/act03_ts_graficas.png)
*Figura 8. Gráficas del voltaje y del ADC en el canal.*

**Código**

**Actividad03_ThingSpeak.ino**

```cpp
/*
  Actividad 03 (Potenciometro) - ThingSpeak (HTTP)
  1) Cree un canal en thingspeak.com con Field1 = Voltaje (V) y Field2 = ADC.
  2) Copie la "Write API Key" del canal.
  Nota: la cuenta gratuita permite 1 dato cada 15 s como minimo.
*/
#include <WiFi.h>
#include <HTTPClient.h>

const char* WIFI_SSID     = "NOMBRE_DE_SU_RED";
const char* WIFI_PASSWORD = "CLAVE_DE_SU_RED";
const char* WRITE_API_KEY = "SU_WRITE_API_KEY";

const unsigned long INTERVALO_MS = 20000;
unsigned long ultimoEnvio = 0;

// ---------- Lectura del potenciometro (GPIO34, ADC1) ----------
const int POT_PIN      = 34;
const int NUM_MUESTRAS = 32;

void iniciarSensor() {
  analogReadResolution(12);
  analogSetPinAttenuation(POT_PIN, ADC_11db);
}

// Devuelve el voltaje promedio en voltios y el ADC promedio (por referencia)
float leerValor(int &adcProm) {
  long sumaADC = 0, sumaMv = 0;
  for (int i = 0; i < NUM_MUESTRAS; i++) {
    sumaADC += analogRead(POT_PIN);
    sumaMv  += analogReadMilliVolts(POT_PIN);
    delay(2);
  }
  adcProm = sumaADC / NUM_MUESTRAS;
  return (sumaMv / (float)NUM_MUESTRAS) / 1000.0;
}
const char* NOMBRE_VAR = "voltaje";

void conectarWiFi() {
  WiFi.mode(WIFI_STA);
  WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
  Serial.print("Conectando a Wi-Fi");
  while (WiFi.status() != WL_CONNECTED) { delay(500); Serial.print("."); }
  Serial.print("\nIP: "); Serial.println(WiFi.localIP());
}

void setup() {
  Serial.begin(115200);
  iniciarSensor();
  conectarWiFi();
}

void loop() {
  if (WiFi.status() != WL_CONNECTED) conectarWiFi();

  if (millis() - ultimoEnvio >= INTERVALO_MS) {
    ultimoEnvio = millis();
    int adc;
    float valor = leerValor(adc);

    String url = "http://api.thingspeak.com/update?api_key=" + String(WRITE_API_KEY) +
                 "&field1=" + String(valor, 3) + "&field2=" + String(adc);

    HTTPClient http;
    http.begin(url);
    int codigo = http.GET();   // ThingSpeak responde con el numero de entrada (0 = error)
    if (codigo > 0) {
      Serial.printf("Enviado -> valor: %.3f | ADC: %d | HTTP: %d | entrada #%s\n",
                    valor, adc, codigo, http.getString().c_str());
    } else {
      Serial.printf("Error HTTP: %s\n", http.errorToString(codigo).c_str());
    }
    http.end();
  }
}
```

> Reemplazar la red Wi-Fi y la `WRITE_API_KEY` por los datos propios.

**Interpretación.** Los envíos realizados desde el ESP32 fueron registrados por ThingSpeak y se reflejaron en las gráficas del canal. Al modificar la posición del potenciómetro, los valores de voltaje y ADC también cambiaron. Debido al intervalo entre envíos, las gráficas representan mediciones separadas y no todos los cambios rápidos del potenciómetro quedan registrados.

### 3.2 Arduino Cloud

Aquí el proceso es bastante distinto. Primero se registró el ESP32 como *dispositivo de terceros* (ESP32 Dev Module), lo que entrega un Device ID y un Secret Key. Después se creó un *Thing* con dos variables de solo lectura, `voltaje` (float) y `valorADC` (entero), y se armó un dashboard con un indicador y un gráfico. En el código, las variables se actualizan en cada vuelta del `loop()` y la biblioteca `ArduinoIoTCloud` se encarga de sincronizarlas con la nube.

![Dispositivo y Thing en Arduino Cloud](images/act03_ac_thing.png)
*Figura 9. Thing con las variables `voltaje` y `valorADC`, asociado al ESP32.*

![Dashboard en Arduino Cloud](images/act03_ac_dashboard.png)
*Figura 10. Dashboard mostrando la variación del potenciómetro.*

**Código**

El sketch de Arduino Cloud se reparte en tres archivos que van como pestañas dentro de la misma carpeta.

**Actividad03_ArduinoCloud.ino**

```cpp
/*
  Actividad 03 (Potenciometro) - Arduino Cloud
  Librerias: ArduinoIoTCloud y Arduino_ConnectionHandler.
  Pasos en app.arduino.cc: Devices > Add > Third party device > ESP32 > "ESP32 Dev Module"
  (guarde el Device ID y el Secret Key) > cree un Thing con las variables indicadas en
  thingProperties.h > asocie el dispositivo > cree un Dashboard con widgets.
*/
#include "arduino_secrets.h"
#include "thingProperties.h"

// ---------- Lectura del potenciometro (GPIO34, ADC1) ----------
const int POT_PIN      = 34;
const int NUM_MUESTRAS = 32;

void iniciarSensor() {
  analogReadResolution(12);
  analogSetPinAttenuation(POT_PIN, ADC_11db);
}

// Devuelve el voltaje promedio en voltios y el ADC promedio (por referencia)
float leerValor(int &adcProm) {
  long sumaADC = 0, sumaMv = 0;
  for (int i = 0; i < NUM_MUESTRAS; i++) {
    sumaADC += analogRead(POT_PIN);
    sumaMv  += analogReadMilliVolts(POT_PIN);
    delay(2);
  }
  adcProm = sumaADC / NUM_MUESTRAS;
  return (sumaMv / (float)NUM_MUESTRAS) / 1000.0;
}
const char* NOMBRE_VAR = "voltaje";

void setup() {
  Serial.begin(115200);
  delay(1500);
  iniciarSensor();

  initProperties();
  ArduinoCloud.begin(ArduinoIoTPreferredConnection);
  setDebugMessageLevel(2);
  ArduinoCloud.printDebugInfo();
}

void loop() {
  ArduinoCloud.update();
  int adc;
  voltaje  = leerValor(adc);
  valorADC = adc;
  delay(200);
}
```

**thingProperties.h**

```cpp
// thingProperties.h - equivalente al archivo que genera Arduino Cloud
#include <ArduinoIoTCloud.h>
#include <Arduino_ConnectionHandler.h>

const char DEVICE_LOGIN_NAME[] = "PEGUE_AQUI_EL_DEVICE_ID";

const char SSID[]     = SECRET_SSID;
const char PASS[]     = SECRET_OPTIONAL_PASS;
const char DEVICE_KEY[] = SECRET_DEVICE_KEY;

float voltaje;
int valorADC;

void initProperties() {
  ArduinoCloud.setBoardId(DEVICE_LOGIN_NAME);
  ArduinoCloud.setSecretDeviceKey(DEVICE_KEY);
  ArduinoCloud.addProperty(voltaje,  READ, 1 * SECONDS, NULL);
  ArduinoCloud.addProperty(valorADC, READ, 1 * SECONDS, NULL);
}

WiFiConnectionHandler ArduinoIoTPreferredConnection(SSID, PASS);
```

**arduino_secrets.h**

```cpp
#define SECRET_SSID "NOMBRE_DE_SU_RED"
#define SECRET_OPTIONAL_PASS "CLAVE_DE_SU_RED"
#define SECRET_DEVICE_KEY "PEGUE_AQUI_EL_SECRET_KEY"
```

> Los valores de `DEVICE_LOGIN_NAME`, `SECRET_DEVICE_KEY` y la red Wi-Fi se completan con los datos propios de Arduino Cloud.

**Interpretación.** Arduino Cloud permite visualizar las variables del ESP32 mediante un dashboard después de configurar el dispositivo, el Thing y sus credenciales. En comparación con el envío manual mediante HTTP utilizado en ThingSpeak, aquí gran parte de la comunicación es gestionada por la plataforma y su biblioteca.

> **Nota:** la parte de Ubidots que plantea el enunciado no se desarrolló en este informe.

---

## Actividad 04: Envío de datos de un sensor del kit Keystudio

### Objetivo

Repetir el envío de datos a la nube, pero ahora con un sensor real del kit Keystudio en lugar del potenciómetro.

### Desarrollo

Se utilizó un sensor del kit Keystudio conectado al **GPIO35**, otro pin del ADC1. La lectura conserva el promedio de muestras utilizado anteriormente, pero ahora el valor obtenido representa una variable del entorno. Según el sensor utilizado, la medición puede corresponder a temperatura (LM35) o nivel de luz (LDR), y posteriormente se envía a la plataforma IoT.

![Montaje del sensor con el ESP32](images/act04_montaje.jpg)
*Figura 11. Conexión del sensor al ESP32.*

![Datos del sensor en la plataforma](images/act04_plataforma.png)
*Figura 12. Visualización de la medición en la plataforma IoT.*

### Código

Esta es la versión para ThingSpeak. Con una sola línea `#define` se elige el sensor (LDR o LM35).

**Actividad04_ThingSpeak.ino**

```cpp
/*
  Actividad 04 (Sensor Keystudio) - ThingSpeak (HTTP)
  1) Cree un canal en thingspeak.com con Field1 = Medicion (°C o % luz) y Field2 = ADC.
  2) Copie la "Write API Key" del canal.
  Nota: la cuenta gratuita permite 1 dato cada 15 s como minimo.
*/
#include <WiFi.h>
#include <HTTPClient.h>

const char* WIFI_SSID     = "NOMBRE_DE_SU_RED";
const char* WIFI_PASSWORD = "CLAVE_DE_SU_RED";
const char* WRITE_API_KEY = "SU_WRITE_API_KEY";

const unsigned long INTERVALO_MS = 20000;
unsigned long ultimoEnvio = 0;

// ---------- Lectura del sensor Keystudio (GPIO35, ADC1) ----------
// Deje ACTIVA solo UNA de las dos lineas siguientes:
#define SENSOR_LDR
//#define SENSOR_LM35

const int SENSOR_PIN   = 35;
const int NUM_MUESTRAS = 32;

void iniciarSensor() {
  analogReadResolution(12);
  analogSetPinAttenuation(SENSOR_PIN, ADC_11db);
}

// Devuelve la medicion (°C para LM35, % de luz para LDR); adcProm por referencia
float leerValor(int &adcProm) {
  long sumaADC = 0, sumaMv = 0;
  for (int i = 0; i < NUM_MUESTRAS; i++) {
    sumaADC += analogRead(SENSOR_PIN);
    sumaMv  += analogReadMilliVolts(SENSOR_PIN);
    delay(2);
  }
  adcProm = sumaADC / NUM_MUESTRAS;
  float mv = sumaMv / (float)NUM_MUESTRAS;
#ifdef SENSOR_LM35
  return mv / 10.0;            // LM35: 10 mV por °C
#else
  return mv * 100.0 / 3300.0;  // LDR: porcentaje respecto a 3.3 V
#endif
}
#ifdef SENSOR_LM35
const char* NOMBRE_VAR = "temperatura";
#else
const char* NOMBRE_VAR = "luz";
#endif

void conectarWiFi() {
  WiFi.mode(WIFI_STA);
  WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
  Serial.print("Conectando a Wi-Fi");
  while (WiFi.status() != WL_CONNECTED) { delay(500); Serial.print("."); }
  Serial.print("\nIP: "); Serial.println(WiFi.localIP());
}

void setup() {
  Serial.begin(115200);
  iniciarSensor();
  conectarWiFi();
}

void loop() {
  if (WiFi.status() != WL_CONNECTED) conectarWiFi();

  if (millis() - ultimoEnvio >= INTERVALO_MS) {
    ultimoEnvio = millis();
    int adc;
    float valor = leerValor(adc);

    String url = "http://api.thingspeak.com/update?api_key=" + String(WRITE_API_KEY) +
                 "&field1=" + String(valor, 3) + "&field2=" + String(adc);

    HTTPClient http;
    http.begin(url);
    int codigo = http.GET();   // ThingSpeak responde con el numero de entrada (0 = error)
    if (codigo > 0) {
      Serial.printf("Enviado -> valor: %.3f | ADC: %d | HTTP: %d | entrada #%s\n",
                    valor, adc, codigo, http.getString().c_str());
    } else {
      Serial.printf("Error HTTP: %s\n", http.errorToString(codigo).c_str());
    }
    http.end();
  }
}
```

### Interpretación

La respuesta de la gráfica depende de los cambios aplicados sobre el sensor. En el caso del LDR, una variación en la cantidad de luz produce cambios en la lectura; con el LM35, el valor aumenta cuando el sensor recibe calor. Esta actividad permitió comprobar que el ESP32 puede adquirir información del entorno y transmitirla a una plataforma IoT sin depender de una entrada controlada manualmente.

---

## Actividad 05: Control de un LED desde la nube

### Objetivo

Recorrer el camino inverso: en lugar de mandar datos hacia la nube, recibir una orden desde una plataforma web para encender y apagar un LED conectado al ESP32.

### Desarrollo

El LED se conectó al **GPIO26** mediante una resistencia de 220 Ω que limita la corriente. En Arduino Cloud se creó la variable `led`, de tipo booleano y con permiso de lectura y escritura, y en el dashboard se agregó un *switch*. En el código, la función `onLedChange()` se ejecuta automáticamente cada vez que el interruptor cambia de estado, y es ahí donde se enciende o se apaga el pin.

![Montaje del LED en la protoboard](images/act05_montaje.jpg)
*Figura 13. LED con su resistencia conectado al GPIO26.*

![Switch en el dashboard](images/act05_dashboard.png)
*Figura 14. Dashboard con el interruptor que controla el LED.*

![LED encendido y apagado](images/act05_led.jpg)
*Figura 15. Estado del LED al activar y desactivar el switch.*

### Código

**Actividad05_LED_ArduinoCloud.ino**

```cpp
/*
  Actividad 05 - Control de un LED desde Arduino Cloud
  Conexion: GPIO26 -> resistencia 220 ohm -> LED (anodo) ; catodo -> GND
  (Alternativa sin componentes: use LED_PIN = 2, LED integrado de la placa)
  En Arduino Cloud cree la variable "led" (Boolean, Read & Write) y un widget Switch.
*/
#include "arduino_secrets.h"
#include "thingProperties.h"

const int LED_PIN = 26;

void setup() {
  Serial.begin(115200);
  delay(1500);
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LOW);

  initProperties();
  ArduinoCloud.begin(ArduinoIoTPreferredConnection);
  setDebugMessageLevel(2);
  ArduinoCloud.printDebugInfo();
}

void loop() {
  ArduinoCloud.update();
}

// Se ejecuta cada vez que el Switch del dashboard cambia
void onLedChange() {
  digitalWrite(LED_PIN, led ? HIGH : LOW);
  Serial.println(led ? "LED encendido" : "LED apagado");
}
```

**thingProperties.h**

```cpp
// thingProperties.h - equivalente al archivo que genera Arduino Cloud
#include <ArduinoIoTCloud.h>
#include <Arduino_ConnectionHandler.h>

const char DEVICE_LOGIN_NAME[] = "PEGUE_AQUI_EL_DEVICE_ID";

const char SSID[]     = SECRET_SSID;
const char PASS[]     = SECRET_OPTIONAL_PASS;
const char DEVICE_KEY[] = SECRET_DEVICE_KEY;

void onLedChange();
bool led;

void initProperties() {
  ArduinoCloud.setBoardId(DEVICE_LOGIN_NAME);
  ArduinoCloud.setSecretDeviceKey(DEVICE_KEY);
  ArduinoCloud.addProperty(led, READWRITE, ON_CHANGE, onLedChange);
}

WiFiConnectionHandler ArduinoIoTPreferredConnection(SSID, PASS);
```

**arduino_secrets.h**

```cpp
#define SECRET_SSID "NOMBRE_DE_SU_RED"
#define SECRET_OPTIONAL_PASS "CLAVE_DE_SU_RED"
#define SECRET_DEVICE_KEY "PEGUE_AQUI_EL_SECRET_KEY"
```

### Interpretación

El cambio del switch en el dashboard produjo el encendido o apagado del LED conectado al ESP32. De esta manera se comprobó la comunicación en sentido inverso a las actividades anteriores: la información no solo se envía desde el dispositivo hacia la nube, sino que también puede utilizarse para generar una acción física sobre el dispositivo.

---

## Conclusiones

- El uso del promedio de muestras permitió obtener lecturas analógicas más estables en el ESP32.
- La conexión mediante Wi-Fi fue necesaria para integrar la placa con servicios IoT y permitió comprobar parámetros básicos de una red.
- ThingSpeak facilitó el registro y visualización de datos, mientras que Arduino Cloud permitió trabajar tanto con variables de monitoreo como con acciones de control.
- Las actividades mostraron el flujo completo de un sistema IoT: **captura de datos, procesamiento, comunicación y actuación**.
- El control remoto del LED permitió observar de forma práctica cómo una orden enviada desde la nube puede producir una respuesta física en el ESP32.

## Herramientas y bibliotecas

- Arduino IDE con el paquete de placas **esp32** (placa "ESP32 Dev Module")
- `WiFi.h` y `HTTPClient.h` (incluidas en el paquete de la ESP32)
- `ArduinoIoTCloud` y `Arduino_ConnectionHandler` (Arduino Cloud)
- ThingSpeak y Arduino Cloud como plataformas IoT


> **Seguridad:** las credenciales de Wi-Fi, las API Keys y las claves de Arduino Cloud deben mantenerse privadas y reemplazarse por los datos correspondientes al dispositivo.
