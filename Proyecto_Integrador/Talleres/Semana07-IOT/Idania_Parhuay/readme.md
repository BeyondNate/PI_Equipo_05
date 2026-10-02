# Taller de Internet de las Cosas (IoT) con ESP32

**Curso:** Proyectos de Ingeniería
**Universidad:** Universidad Peruana Cayetano Heredia (UPCH)

**Estudiante:** Idania Parhuay Meza

---

## Introducción

Este taller me ayudó a llevar la teoría de IoT a la práctica y ver cómo funciona todo en la realidad. Usamos una placa ESP32 para tomar datos del entorno, conectarla a internet y subir la información a la nube para verla en tiempo real. Las cinco actividades siguen una progresión natural. Primero se entiende cómo el microcontrolador transforma una señal analógica en un número; luego se le da conectividad; y al final se envían datos a plataformas en línea y se controla un componente de forma remota.

Todo el trabajo se desarrolló con una **ESP32 WROOM** programada desde el **Arduino IDE**.

## Materiales utilizados

- ESP32 WROOM (Dev Kit)
- Potenciómetro
- Sensor del kit Keystudio 48 en 1 (LM35)
- LED y resistencia de 220 Ω
- Protoboard, cables y multímetro
- Cable USB con transmisión de datos
- Celular con hotspot en banda de 2.4 GHz

## Resumen de actividades
 
| Actividad | Tema | Pin / Plataforma |
|---|---|---|
| 01 | Potenciómetro con promediado y conversión a voltaje | GPIO34 |
| 02 | Conexión Wi-Fi mediante hotspot | Hotspot del celular |
| 03 | Potenciómetro enviado a la nube | GPIO34 · ThingSpeak (HTTP) |
| 04 | Sensor LM35 enviado a la nube | GPIO35 · ThingSpeak (HTTP) |
| 05 | Control de un LED desde la nube | GPIO2 · MQTT (broker público EMQX) |
 
---

## Actividad 01: Potenciómetro con promediado y conversión a voltaje

### Objetivo

Leer los valores de un potenciómetro conectado al ESP32, estabilizar las mediciones promediando varias muestras y convertir lo que entrega el ADC a voltaje.

### Conexión

Conecté el potenciómetro con GND a tierra, VCC a los 3.3 V de la placa y la señal (SIG) al **GPIO34**. Elegí ese pin a propósito: pertenece al ADC1, que sigue funcionando con normalidad cuando el Wi-Fi está activo, algo que iba a necesitar en las actividades siguientes.

<p align="center">
  <img src="https://github.com/user-attachments/assets/8421c653-a486-4b3f-9ee8-981cd5ad93a9" width="700" alt="Montaje del potenciómetro"><br>
  <em>Figura 1. Montaje del potenciómetro en la protoboard.</em>
</p>

### Desarrollo

La lectura del ADC tiene una resolución de 12 bits, así que el valor se mueve entre 0 y 4095. El detalle es que, incluso con el potenciómetro quieto, ese número oscila un poco de una lectura a otra. Para suavizarlo, el código toma 32 muestras consecutivas y trabaja con su promedio.
 
Con ese valor promedio calculé el voltaje de dos maneras:
 
- **Conversión lineal:** usa la referencia de 3.3 V y el máximo del ADC (4095), o sea, la relación teórica entre ambos.
- **Conversión calibrada:** usa `analogReadMilliVolts()`, que tiene en cuenta la calibración que el fabricante grabó en el ESP32.

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

<p align="center">
  <img src="https://github.com/user-attachments/assets/30829e6e-f250-40f1-949f-64d961b0dcdc" width="700" alt="Monitor serie de la Actividad 01"><br>
  <em>Figura 2. Monitor serie con el ADC promedio y el voltaje lineal y calibrado en distintas posiciones del potenciómetro.</em>
</p>

### Interpretación

Promediar las 32 muestras dio lecturas bastante más estables que la lectura directa del ADC. Al girar el potenciómetro, el valor promedio sube o baja según la posición, y el voltaje lo acompaña de forma proporcional, lo que vuelve los datos mucho más fáciles de interpretar que un simple número entre 0 y 4095.
 
Entre las dos columnas de voltaje aparece una pequeña diferencia, y tiene una explicación sencilla: la conversión lineal parte de una relación teórica, mientras que `analogReadMilliVolts()` incorpora la calibración propia del ESP32, que corrige parte de la no linealidad del conversor. En conjunto, la actividad me sirvió para comprobar cómo se lee una señal analógica con el ESP32 y para ver que unas pocas líneas de código (promediar y convertir) mejoran bastante la calidad de la medición.

---

## Actividad 02: Conexión Wi-Fi mediante hotspot

### Objetivo

Crear una red Wi-Fi con el hotspot de mi smartphone, conectar el ESP32 a ella y mostrar en el Monitor Serie la dirección IP y otros parámetros de la conexión.

### Desarrollo

Para conectarme usé la biblioteca `WiFi.h`, con el ESP32 configurado en modo estación (`WIFI_STA`), es decir, como un cliente que se une a una red que ya existe. El hotspot del celular lo dejé en la banda de **2.4 GHz**, porque el ESP32 no reconoce redes de 5 GHz.

Cuando la conexión se establece, el programa imprime:

- **Dirección IP:** identifica al ESP32 dentro de la red.
- **Puerta de enlace:** el punto por el que se comunica con otras redes.
- **Máscara de subred:** define el rango de direcciones de la red.
- **Dirección MAC:** identificador único de la interfaz Wi-Fi.
- **RSSI:** la intensidad de la señal recibida.

Además, revisa cada cierto tiempo si la conexión sigue activa y, si se pierde, intenta reconectarse.

### Código

**Actividad02_WiFi_Hotspot.ino**

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

### Resultados

| Parámetro | Resultado |
|---|---|
| Estado | Conexión exitosa |
| Dirección IP | 10.28.25.252 |
| Puerta de enlace | 10.28.25.37 |
| Máscara de subred | 255.255.255.0 |
| Dirección MAC | 40:22:D8:60:B2:B0 |
| RSSI inicial | -53 dBm |
| RSSI posterior | -40 dBm |
| RSSI final | -38 dBm |

<p align="center">
  <img src="https://github.com/user-attachments/assets/696f954d-e72b-4559-8726-c00411d9ee1f" width="700" alt="Monitor serie de la Actividad 02"><br>
  <em>Figura 3. Monitor serie con la conexión exitosa y los parámetros de red del ESP32.</em>
</p>

### Interpretación

El ESP32 se conectó sin problemas al hotspot y recibió la dirección IP 10.28.25.252. La máscara 255.255.255.0 corresponde a una red /24, y la puerta de enlace fue 10.28.25.37, que es el propio celular haciendo de router. Con esos datos queda claro que la placa ya forma parte de una red local con salida a internet, que es justo lo que necesitan las actividades que siguen.

Lo que más me llamó la atención fue la evolución del RSSI: pasó de -53 dBm a -40 dBm y terminó en -38 dBm. Como los valores más cercanos a 0 indican una señal más fuerte, la conexión fue mejorando a lo largo de las mediciones; lo más probable es que se deba a un cambio en la distancia o en la posición entre el celular y la placa. El programa, además, contempla la posibilidad de perder la conexión y vuelve a ejecutar el proceso de conexión cuando eso ocurre.

### Observaciones

- Al iniciar el ESP32 aparecen algunos mensajes del proceso de arranque, entre ellos uno que dice _"Core dump data check failed"_. Aun así, la ejecución continuó con normalidad y la placa se conectó al hotspot sin inconvenientes. Por lo que entiendo, ese aviso es informativo y suele aparecer cuando no hay ningún registro de fallos guardado en la memoria de la placa.
- Al subir el código me apareció el error _"Wrong boot mode detected (0x13)"_. Lo solucioné manteniendo presionado el botón **BOOT** mientras el IDE mostraba "Connecting..." y soltándolo cuando comenzó la escritura.

---

## Actividad 03: Envío de datos del potenciómetro a la nube

### Objetivo

Mostrar en tiempo real la variación del potenciómetro en plataformas IoT. Se trabajó con **ThingSpeak** como plataforma principal y se intentó además con **Arduino Cloud**.

### 3.1 ThingSpeak

Se creó un canal con dos campos: el voltaje (Field 1) y el valor del ADC (Field 2). El ESP32 se conecta al Wi-Fi y, cada 20 segundos, arma una petición HTTP con la *Write API Key* del canal y los valores medidos. El intervalo no es casual: la cuenta gratuita de ThingSpeak exige al menos 15 segundos entre envíos, así que dejé un pequeño margen de seguridad. La lectura del potenciómetro es la misma de la Actividad 1, con promedio de 32 muestras, para enviar un dato limpio.

**Código**

**Actividad03_ThingSpeak.ino**

```cpp
#include <WiFi.h>
#include <HTTPClient.h>

const char* WIFI_SSID     = "ajam";
const char* WIFI_PASSWORD = "Idania123";
const char* WRITE_API_KEY = "U8EKFOD3X40ZAD8X";

const unsigned long INTERVALO_MS = 20000;
unsigned long ultimoEnvio = 0;

// ---------- Lectura del potenciometro (GPIO34, ADC1) ----------
const int POT_PIN      = 34;
const int NUM_MUESTRAS = 32;

void iniciarSensor() {
  analogReadResolution(12);
  analogSetPinAttenuation(POT_PIN, ADC_11db);
}

// Devuelve el voltaje promedio (V) y el ADC promedio (por referencia)
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
const char* NOMBRE_VAR = "Voltaje";

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
    int codigo = http.GET();   // ThingSpeak responde con el n.° de entrada (0 = error)
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

### Resultados
 
<p align="center">
  <img src="https://github.com/user-attachments/assets/031dc061-b8df-4372-b4c2-a264fc9989af" width="700" alt="Canal de ThingSpeak"><br>
  <em>Figura 4. Canal de ThingSpeak con sus campos configurados.</em>
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/199e9f2d-9067-44dc-8389-461ba343414d" width="700" alt="Monitor serie de la Actividad 03"><br>
  <em>Figura 5. Monitor serie confirmando cada envío (código HTTP 200 y número de entrada).</em>
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/ed89e769-dc04-4fc5-a314-4c6731143d3d" width="700" alt="Gráficas de ThingSpeak"><br>
  <em>Figura 6. Gráficas del voltaje y del ADC en el canal.</em>
</p>

### Interpretación
 
En el monitor serie, cada envío devuelve el código HTTP 200 y un número de entrada que va creciendo, señal de que ThingSpeak está recibiendo y guardando los datos. En las gráficas se aprecia cómo el voltaje sube y baja según lo que hice con el potenciómetro, y las dos curvas tienen la misma forma porque el voltaje no es más que el ADC escalado.
 
También se nota una limitación de la plataforma: con un dato cada 20 segundos, los movimientos rápidos se pierden y el resultado se parece más a una serie de puntos que a una señal continua. Para registrar tendencias es suficiente, pero no para observar cambios veloces.


> **Nota:** Desarrollé únicamente la parte de ThingSpeak; Arduino Cloud y Ubidots no se incluyeron en este taller.

---

## Actividad 04: Envío de datos del sensor LM35 a la nube

### Objetivo

Repetir el envío de datos a la nube, pero ahora con un sensor real del kit Keystudio, el LM35, en lugar del potenciómetro.

### Desarrollo

Se utilizó el sensor de temperatura **LM35**, conectado al **GPIO35**, otro pin del ADC1. Lo alimenté con los 5 V del pin VIN en lugar de los 3.3 V, porque el LM35 necesita alrededor de 4 V o más para funcionar bien; su salida, en cambio, es de apenas unas décimas de voltio, así que no representa riesgo para el ADC del ESP32.
 
La lectura sigue la misma lógica de la Actividad 1, con promedio de 32 muestras, pero ahora el voltaje se interpreta como temperatura: el LM35 entrega 10 mV por cada grado Celsius, por lo que basta dividir los milivoltios entre 10. Los datos se enviaron a un canal propio de ThingSpeak, con la temperatura en el Field 1 y el valor del ADC en el Field 2, una vez cada 20 segundos.

<p align="center">
  <img src="https://github.com/user-attachments/assets/12c1b26d-a474-462d-85eb-223e2426aa19" width="700" ><br>
  <em>Figura 7. Figura de montaje.</em>
</p>

### Código

**Actividad04_ThingSpeak.ino**

```cpp
/*
  Actividad 04 - Sensor LM35 + ThingSpeak

  Conexiones:
  LM35 VCC  -> ESP32 VIN (5 V)
  LM35 OUT  -> ESP32 GPIO35
  LM35 GND  -> ESP32 GND

  ThingSpeak:
  Field 1 = Temperatura (°C)
  Field 2 = ADC

  El ESP32 envía una medición cada 20 segundos.
*/

#include <WiFi.h>
#include <HTTPClient.h>

// ---------- Datos de conexión ----------
const char* WIFI_SSID     = "ajam";
const char* WIFI_PASSWORD = "Idania123";
const char* WRITE_API_KEY = "78IAZ1G9HXTYS73H";

// ---------- Intervalo de envío ----------
const unsigned long INTERVALO_MS = 20000;
unsigned long ultimoEnvio = 0;

// ---------- Sensor LM35 ----------
const int SENSOR_PIN   = 35;
const int NUM_MUESTRAS = 32;

// Configuración del ADC
void iniciarSensor() {
  analogReadResolution(12);
  analogSetPinAttenuation(SENSOR_PIN, ADC_11db);
}

// Lee el LM35 y devuelve la temperatura en °C
// adcProm recibe el valor ADC promedio
float leerTemperatura(int &adcProm) {
  long sumaADC = 0;
  long sumaMv  = 0;

  // Tomar varias muestras para estabilizar la lectura
  for (int i = 0; i < NUM_MUESTRAS; i++) {
    sumaADC += analogRead(SENSOR_PIN);
    sumaMv  += analogReadMilliVolts(SENSOR_PIN);
    delay(2);
  }

  adcProm = sumaADC / NUM_MUESTRAS;
  float mv = sumaMv / (float)NUM_MUESTRAS;

  // LM35: 10 mV = 1 °C
  return mv / 10.0;
}

// ---------- Conexión Wi-Fi (máximo 20 intentos) ----------
void conectarWiFi() {
  WiFi.mode(WIFI_STA);
  WiFi.begin(WIFI_SSID, WIFI_PASSWORD);

  Serial.print("Conectando a Wi-Fi");

  int intentos = 0;
  while (WiFi.status() != WL_CONNECTED && intentos < 20) {
    delay(500);
    Serial.print(".");
    intentos++;
  }

  if (WiFi.status() == WL_CONNECTED) {
    Serial.println("\nConectado exitosamente");
    Serial.print("IP: ");
    Serial.println(WiFi.localIP());
  } else {
    Serial.println("\nNo se pudo conectar. Verifique SSID, clave y banda 2.4 GHz.");
  }
}

// ---------- Configuración inicial ----------
void setup() {
  Serial.begin(115200);
  delay(1000);

  iniciarSensor();
  conectarWiFi();

  ultimoEnvio = millis() - INTERVALO_MS;   // Primer envío inmediato
}

// ---------- Programa principal ----------
void loop() {
  // Si se cayó el Wi-Fi, reintentar
  if (WiFi.status() != WL_CONNECTED) {
    conectarWiFi();
  }

  // Esperar hasta cumplir el intervalo de envío
  if (millis() - ultimoEnvio >= INTERVALO_MS) {
    ultimoEnvio = millis();

    int adc;
    float temperatura = leerTemperatura(adc);

    if (WiFi.status() == WL_CONNECTED) {
      String url = "http://api.thingspeak.com/update?api_key=" +
                   String(WRITE_API_KEY) +
                   "&field1=" + String(temperatura, 3) +
                   "&field2=" + String(adc);

      HTTPClient http;
      http.begin(url);
      int codigo = http.GET();

      if (codigo > 0) {
        Serial.printf("Enviado -> Temperatura: %.3f °C | ADC: %d | HTTP: %d | entrada #%s\n",
                      temperatura, adc, codigo, http.getString().c_str());
      } else {
        Serial.printf("Error HTTP: %s\n", http.errorToString(codigo).c_str());
      }
      http.end();
    } else {
      Serial.printf("Lectura local -> Temperatura: %.3f °C | ADC: %d (sin Wi-Fi)\n", temperatura, adc);
    }
  }
}
```

### Resultados
 
<p align="center">
  <img src="https://github.com/user-attachments/assets/07795bcc-4170-4605-b971-04b6941e1b05" width="700" alt="Canal de ThingSpeak para el LM35"><br>
  <em>Figura 8. Canal de ThingSpeak configurado para la temperatura y el ADC.</em>
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/2b042b73-bf60-4508-90fd-a79629ca3066" width="700" alt="Monitor serie de la Actividad 04"><br>
  <em>Figura 9. Monitor serie con los envíos de temperatura y ADC.</em>
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/42ca3c71-5f30-437d-8837-b92d0a3a8379" width="700" alt="Gráficas de ThingSpeak para el LM35"><br>
  <em>Figura 10. Visualización de la temperatura medida en la plataforma IoT.</em>
</p>

### Interpretación

Para comprobar que el sensor respondía, lo calenté sosteniéndolo entre los dedos, y en la gráfica de ThingSpeak se ve cómo la temperatura sube y luego vuelve poco a poco hacia el valor ambiente. Esa lentitud al bajar es normal, porque el sensor tarda en disipar el calor que recibió.
 
Lo interesante de esta actividad es que ya no se trata de una señal que yo controlo girando una perilla, sino de una variable del entorno que el sistema mide y reporta por su cuenta. También noté que las lecturas del LM35 con el ESP32 pueden variar un poco de una medición a otra: la señal que entrega es muy pequeña (unos 250 mV a 25 °C) y justo en ese rango el ADC es menos preciso, por lo que el promedio de muestras resultó especialmente útil.

---

## Actividad 05: Control de un LED desde la nube

### Objetivo
 
Recorrer el camino inverso al de las actividades anteriores: en lugar de mandar datos hacia la nube, recibir una orden desde internet para encender y apagar un LED conectado al ESP32.

### Desarrollo
 
Para esta actividad se usó el protocolo **MQTT**, que funciona con un esquema de publicación y suscripción. El ESP32 se conecta primero al Wi-Fi del hotspot y luego al broker público **broker.emqx.io** (puerto 1883), donde se suscribe al topic `esp32/led`. Cualquier mensaje publicado en ese topic llega a la placa: si el texto es `ON`, la función `callback()` enciende el LED, y si es `OFF`, lo apaga. Para la prueba monté un **LED externo** en la protoboard, con una resistencia limitadora en serie, conectado al **GPIO2** y con su otro extremo a GND. Las órdenes las envié desde una aplicación cliente MQTT instalada en el celular.
 
Dos detalles del código vale la pena destacar. Por un lado, el identificador de cliente se genera de forma aleatoria en cada conexión, para evitar choques con otros usuarios del broker público. Por otro, si la conexión con el broker se interrumpe, el programa se reconecta y vuelve a suscribirse por su cuenta.
 
<p align="center">
  <img src="https://github.com/user-attachments/assets/2830c55d-b177-49ee-abc0-28648e3a9dec" width="700" alt="Montaje de la Actividad 05"><br>
  <em>Figura 11. Montaje de la Actividad 05: ESP32 y LED externo con su resistencia en la protoboard (LED resaltado en rojo).</em>
</p>


### Código

**Actividad05_LED_ArduinoCloud.ino**

```cpp
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "ZTE Blade A56 Pro";
const char* password = "702047779448";

const char* mqtt_server = "broker.emqx.io";

const int LED_PIN = 2;

WiFiClient espClient;
PubSubClient client(espClient);

void callback(char* topic, byte* payload, unsigned int length) {

  String mensaje = "";

  for (unsigned int i = 0; i < length; i++) {
    mensaje += (char)payload[i];
  }

  Serial.print("Mensaje recibido: ");
  Serial.println(mensaje);

  if (mensaje == "ON") {
    digitalWrite(LED_PIN, HIGH);
    Serial.println("LED ENCENDIDO");
  }

  if (mensaje == "OFF") {
    digitalWrite(LED_PIN, LOW);
    Serial.println("LED APAGADO");
  }
}

void conectarMQTT() {

  while (!client.connected()) {

    Serial.println("Intentando conectar a MQTT...");

    String clientId = "ESP32-";
    clientId += String(random(0xffff), HEX);

    if (client.connect(clientId.c_str())) {

      Serial.println("MQTT CONECTADO");

      client.subscribe("esp32/led");

      Serial.println("Suscrito a: esp32/led");

    } else {

      Serial.print("Fallo MQTT. Estado: ");
      Serial.println(client.state());

      delay(3000);
    }
  }
}

void setup() {

  Serial.begin(115200);

  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, LOW);

  WiFi.mode(WIFI_STA);
  WiFi.disconnect();
  delay(1000);

  WiFi.begin(ssid, password);

  Serial.print("Conectando al WiFi");

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println();
  Serial.println("WiFi conectado");

  Serial.print("IP: ");
  Serial.println(WiFi.localIP());

  client.setServer(mqtt_server, 1883);
  client.setCallback(callback);

  conectarMQTT();
}

void loop() {

  if (!client.connected()) {
    conectarMQTT();
  }

  client.loop();
}
```

### Resultados
 
<p align="center">
  <img src="https://github.com/user-attachments/assets/79d5f316-fbca-4d7b-a82b-7b0969f91d38" width="700" alt="Captura en el computador"><br>
  <em>Figura 12. Captura realizada en el computador durante la prueba.</em>
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/c719a8bc-d0aa-4876-a7c8-50feef4bf23b" width="200" alt="Captura en el celular 1">
  <img src="https://github.com/user-attachments/assets/d085c998-668a-4d16-bc36-ca40752f7377" width="200" alt="Captura en el celular 2">
  <img src="https://github.com/user-attachments/assets/4eabf3b3-2ef0-4863-a0dc-b09be273ba18" width="200" alt="Captura en el celular 3"><br>
  <em>Figuras 13 a 15. Capturas realizadas en el celular durante la prueba.</em>
</p>
<p align="center">
  <img src="https://github.com/user-attachments/assets/c450a1e0-d9dc-4733-b720-335ddcb95448" width="260" alt="LED de la placa">
  <img src="https://github.com/user-attachments/assets/b63de407-809d-42ac-81f7-af8c47cb5053" width="260" alt="LED de la placa"><br>
  <em>Figuras 16 y 17. Estado del LED de la placa durante la prueba.</em>
</p>

### Interpretación
 
La prueba confirmó que el ESP32 puede recibir órdenes desde internet sin necesidad de montar un servidor propio. Desde la aplicación del celular se conectó al mismo broker (Figura 12) y se publicó mensajes en el topic `esp32/led` (Figuras 13 y 14): al enviar `ON`, el broker reenvía el mensaje a la placa, que está suscrita, y el LED externo se enciende; con `OFF` se apaga. Los mensajes se enviaron con calidad de servicio 0, es decir, con entrega como máximo una vez, algo más que suficiente para un LED, donde perder un mensaje no tiene consecuencias graves. A diferencia de las actividades anteriores, aquí la placa no consulta ni envía nada por iniciativa propia: se queda a la escucha, y el broker le entrega el mensaje apenas llega.
 
Esto ilustra la comunicación bidireccional propia de un sistema IoT y explica por qué MQTT se usa tanto en este campo: los mensajes son pequeños, la conexión es liviana y un mismo dato puede llegar a varios dispositivos a la vez. También deja ver una limitación importante. El broker utilizado es público y no pide usuario ni contraseña (en la Figura 12 los campos de credenciales quedaron vacíos), así que cualquiera que conozca el topic podría enviar órdenes a la placa. En una aplicación real convendría un broker con autenticación.

---

## Conclusiones
 
- El ADC del ESP32 entrega valores con algo de ruido y cierta no linealidad. Conviene promediar las lecturas y, cuando se necesita precisión en voltios, recurrir a la lectura calibrada.
- Para trabajar con Wi-Fi hay que usar pines del ADC1 y redes de 2.4 GHz. Son detalles que pasan desapercibidos hasta que algo deja de funcionar.
- ThingSpeak, con peticiones HTTP, resulta práctico para registrar y graficar datos, pero su límite de un envío cada 15 segundos lo hace poco adecuado para el control en tiempo real. MQTT, en cambio, ofrece una comunicación liviana e inmediata en ambos sentidos, aunque exige cuidar la seguridad del broker.
- Un sistema IoT se entiende mejor como una cadena completa (sensor, microcontrolador, red y plataforma), donde la falla de cualquier eslabón se traduce en datos que no llegan.
- Lo aprendido se puede trasladar al proyecto integrador. Por ejemplo, en un sistema automatizado de clarificación de agua con quitosano, un ESP32 podría registrar variables del proceso en la nube y recibir órdenes remotas, como activar o detener la dosificación.

## Herramientas y bibliotecas
 
- Arduino IDE con el paquete de placas **esp32** (placa "ESP32 Dev Module")
- `WiFi.h` y `HTTPClient.h` (incluidas en el paquete de la ESP32)
- `PubSubClient` (cliente MQTT)
- ThingSpeak como plataforma IoT y EMQX como broker MQTT público


## Referencias
 
- Presentación del taller de IoT, Proyectos de Ingeniería, UPCH.
