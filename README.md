# PRACTICA-10
ENCENDER UN LED CON NODE-RED
## Introducción

Este repositorio muestra como podemos programar una ESP32  que se conecte mediante wi-fi a un servidor de **Node-RED**y asi encender un **LED**



### Descripción

La ```Esp32``` se conecta mediante wi-fi a un servidor y puede realizar diferentes actividades deseadas por el usuario ; Cabe aclarar que en  esta practica se usara un simulador llamado [WOKWI](https://https://wokwi.com/).


## Material Necesario

Para realizar esta practica necesitas lo siguiente

- [WOKWI](https://https://wokwi.com/)
- Tarjeta ESP 32
- Node-RED
- LED

## Instrucciones
Instalar el software Node-RED
instalar paqueterias 


## PAQUETERIAS
![](https://github.com/WilberVD/PRACTICA8/blob/main/librerias.jpg)

### Instrucciones de preparación de entorno 

1. Abrir la terminal de programación y colocar la siguente programación:

```
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqttServer = "54.146.113.169";
const int mqttPort = 1883;
const char* mqttUser = "diegojm";
const char* mqttPassword = "1234";
const char* mqttTopic = "wilberLED";

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

2. Hacer la conexion del  **LED**  con la **ESP32** y y tambien crear los grupos en el servidor **Node-RED** y configurar los parametros.

![](https://github.com/WilberVD/PRACTICA-10/blob/main/nodred.jpg)


### Instrucciónes de operación

1. Iniciar simulador.
2. Visualizar los datos en el monitor serial.
3. ENCENDER EL **SWITCH** 


Cuando haya funcionado, verás los valores dentro del monitor serial como se muestra en la siguente imagen.

![](https://github.com/WilberVD/PRACTICA-10/blob/main/corriendo.jpg)
![](https://github.com/WilberVD/PRACTICA-10/blob/main/corr2.jpg)

# Créditos

Desarrollado por Wilber Valladares Diaz

- [GitHub](WilberVD (github.com))
- [LINK DEL PROGRAMA EN WOKI](https://wokwi.com/projects/387863774841582593)
