# Practica-No.10-Encender-LED
Dentro de este repositorio se muestra la manera de programar una ESP32 para que, mediante el programa **Node-RED**, se pueda encender un **LED**. 
## Introducción
### Descripción
La **ESP32** se utiliza en un entorno de adquisición de datos. Mediante un *switch* ubicado en la interfaz de *Node-RED*, se mandará la señal que activará o desactivará el *LED*. Para esta practica se hace uso del programa **Node-RED** y de un simulador llamado [WOKWI](https://wokwi.com/projects/new/esp32).
## Material necesario
Para realizar esta práctica necesitas lo siguiente

- [WOKWI](https://wokwi.com/projects/new/esp32)
- Tarjeta ESP32
- LED
- Programa *Node-RED*

## Instrucciones
### Requisitos previos
Para poder hacer uso de este repositorio se requiere entrar a **Node-RED** y a la plataforma de [WOKWI](https://wokwi.com/projects/new/esp32).
### Instrucciones de preparación del entorno
1. Abrir la terminal de programación y colocar el siguiente código:

```
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqttServer = "18.193.219.109";
const int mqttPort = 1883;
const char* mqttUser = "diegojm";
const char* mqttPassword = "1234";
const char* mqttTopic = "AbrahamLED";

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
2. Instalar la libreria de **ArduinoJson** y **PubSubClient** como se muestra en la siguiente imagen.
![](https://github.com/AbrahamCH1/Practica-No.8-Node-RED-con-DHT22/blob/main/Captura%20de%20pantalla%20(322).png?raw=true)

3. Hacer la conexion del **LED** como se muestra en la siguente imagen.
![](https://github.com/AbrahamCH1/Practica-No.8-Node-RED-con-DHT22/blob/main/Captura%20de%20pantalla%20(323).png?raw=true)

4. Abrir **Node-RED** y hacer la conexión de las herramientas como se muestra en la imagen.
![](https://github.com/AbrahamCH1/Practica-No.8-Node-RED-con-DHT22/blob/main/Captura%20de%20pantalla%20(324).png?raw=true)

5. Colocar el servidor y el topico dando *doble click* en el recuadro de *mqtt out**.
![](https://github.com/AbrahamCH1/Practica-No.8-Node-RED-con-DHT22/blob/main/Captura%20de%20pantalla%20(325).png?raw=true)

6. Dar *click* en el botón *deploy* y abrir la interfaz para observar los resultados.

### Instrucciones de operación
1. Iniciar simulador.
2. En la interfaz de *Node-RED* accionar el *switch*.
3. Observar en el simulador que el *LED* haya encendido.

## Resultados
Cuando haya funcionado, se podrán observar los valores en la interfaz de Node-RED.
![](https://github.com/AbrahamCH1/Practica-No.8-Node-RED-con-DHT22/blob/main/Captura%20de%20pantalla%20(328).png?raw=true)


# Créditos
Desarrollado por Ing. Abraham Contreras Herrera
[GITHUB](https://github.com/AbrahamCH1)
