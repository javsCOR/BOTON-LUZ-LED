# Practica ESP32 CON NODE RED LUZ LED
Este repositorio muestra como podemos programar una ESP32 con  programa node-red.


## Introducción

### Descripción

La ```Esp32``` la utilizamos en un entorno de adquision de datos, lo cual en esta practica ocuparemos un sensor de RELAY ; Cabe aclarar que esta practica se usara un simulador llamado [WOKWI](https://https://wokwi.com/)y NODE-red (http://localhost:1880/#flow/214928d358894902).


## Material Necesario

Para realizar esta practica necesitas lo siguiente

- [WOKWI](https://https://wokwi.com/)
- Tarjeta ESP 32
- NODE-red (http://localhost:1880/#flow/214928d358894902).
- sensor RELAY





## Instrucciones

### Requisitos previos

Para poder usar este repositorio necesitas entrar a la plataforma [WOKWI](https://https://wokwi.com/).


### Instrucciones de preparación de entorno 

1. Abrir la terminal de programación y colocar la siguente programación:

```
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqttServer = "44.195.202.69";
const int mqttPort = 1883;
const char* mqttUser = "javierCR";
const char* mqttPassword = "987654321";
const char* mqttTopic = "javierled";

WiFiClient espClient;
PubSubClient client(espClient);

int ledPin = 13; // Pin del LED

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqttServer, mqttPort);
  client.setCallback(callback);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();
}

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a: ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("Conectado a la red WiFi");
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Intentando conexión MQTT...");
    if (client.connect("ESP32Client", mqttUser, mqttPassword)) {
      Serial.println("Conectado");
      client.subscribe(mqttTopic);
    } else {
      Serial.print("Error de conexión, rc=");
      Serial.print(client.state());
      Serial.println(" Intentando de nuevo en 5 segundos");
      delay(5000);
    }
  }
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Mensaje recibido: [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();

  if (strcmp(topic, mqttTopic) == 0) {
    if ((char)payload[0] == '1') {
      digitalWrite(ledPin, HIGH);
    } else {
      digitalWrite(ledPin, LOW);
    }
  }
}

```
2. Instalamos  librerias "ARDUINOJSON" "PUBSUBCLIENT" "DHT sensor library for ESPx"


3. Hacer la conexion  con una direccion IP para enlazar worwi con node-red



### Instrucciónes de operación

1. Iniciar simulador.
2. Visualizar los datos en el monitor serial.



![](![Alt text](image.png))

![](https://github.com/javsCOR/BOTON-LUZ-LED/blob/main/DIAGRAMA%202.png?raw=true)

## Resultados cuando enlazamos el programam worwi una vez funcionando debemos armar un diagrama como la siguiente imagen. 

![](https://github.com/javsCOR/BOTON-LUZ-LED/blob/main/DIAGRAMA%204.png?raw=true)



Cuando haya funcionado, verás  EN EL PROGRAMA NODE RED  como se muestra EL BOTON PARA MINIPULAR LA ACCION  en la siguente imagen.

![](https://github.com/javsCOR/BOTON-LUZ-LED/blob/main/DIAGRAMA%203.png?raw=true)







# Créditos

creado por ING javier cortes rojas
