[MQTT](https://mqtt.org/) is a lightweight and flexible IoT message exchanging and data delivery protocol. It dedicates to implementing a balance of flexibility and hardware/network resources for IoT developers.

[NodeMCU](https://www.nodemcu.com/) is an open-source IoT platform. It uses the Lua language to program. This platform is based on eLua open-source projects, and its underlying layer uses the ESP8266 SDK 0.9.5.

In this project, we will implement remote control LED lights via NodeMCU(ESP8266) and the free [public MQTT broker](https://www.emqx.com/en/mqtt/public-mqtt5-broker) which is operated and maintained by [EMQX Cloud](https://www.emqx.com/en/cloud), and use the Arduino IDE to program NodeMCU ESP8266. EMQX Cloud is the [MQTT IoT Cloud Service Platform](https://www.emqx.com/en/cloud) launched by EMQ, which provides the **MQTT 5.0** access service with one-stop operations and maintenance managed and a unique isolated environment.

> Learn more: [ESP32 Connects to The Free Public MQTT Broker](https://www.emqx.com/en/blog/esp32-connects-to-the-free-public-mqtt-broker)

## Required Components

* NodeMCU
* Arduino IDE
* LED * 1，330 Ω resistor
* [MQTTX](https://mqttx.app/): Elegant cross-platform MQTT 5.0 client tool
* The free public MQTT broker
  - Broker:  **broker.emqx.io**
  - TCP Port:  **1883**
  - Websocket Port:  **8083**



## NodeMCU ESP8266 and LED Connection Diagram

![project](https://assets.emqx.com/images/esp8266_control_led.png)



## Code

1. First, we will import the **ESP8266WiFi** and **PubSubClient** libraries. The ESP8266WiFi library can connect the ESP8266 to the WiFi network, and the PubSubClient library allows us to connect to the MQTT broker and publish/subscribe to topics.

   ```c
   #include <ESP8266WiFi.h>
   #include <PubSubClient.h>
   ```

2. We will use the **D1** pin of NodeMCU ESP8266 to connect to the LED. Actually, the inside of this pin is connected to **GPIO5** of the ESP8266 module.

   ```c
   // GPIO 5 D1
   #define LED 5
   ```

3. Set the WIFI name and password as well as the MQTT Broker connection address and port.

   ```c
   // WiFi
   const char *ssid = "mousse"; // Enter your WiFi name
   const char *password = "qweqweqwe";  // Enter WiFi password
    
   // MQTT Broker
   const char *mqtt_broker = "broker.emqx.io";
   const char *topic = "esp8266/led";
   const char *mqtt_username = "emqx";
   const char *mqtt_password = "public";
   const int mqtt_port = 1883;
   ```

4. We have opened a serial connection to print the program's results and connect to the WiFi network.

   ```c
   // Set software serial baud to 115200;
   Serial.begin(115200);
   // connecting to a WiFi network
   WiFi.begin(ssid, password);
   while (WiFi.status() != WL_CONNECTED) {
       delay(500);
       Serial.println("Connecting to WiFi..");
   }
   ```

5. We will set the MQTT Broker and also print the connection information to the serial monitor.

   ```c
    //connecting to a MQTT Broker
   client.setServer(mqtt_broker, mqtt_port);
   client.setCallback(callback);
   while (!client.connected()) {
       String client_id = "esp8266-client-";
       client_id += String(WiFi.macAddress());
       Serial.println("Connecting to public emqx mqtt broker.....");
       if (client.connect(client_id, mqtt_username, mqtt_password)) {
           Serial.println("Public emqx mqtt broker connected");
       } else {
           Serial.print("failed with state ");
           Serial.print(client.state());
           delay(2000);
       }
   }
   ```

6. After successfully connecting to the MQTT Broker, ESP8266 will publish and subscribe to the MQTT Broker.

   ```c
   // publish and subscribe
   client.publish(topic, "hello emqx");
   client.subscribe(topic);
   ```

7. Writing a callback function to read the sending commands from the serial monitor and control the LED on and off.

   ```c
   void callback(char *topic, byte *payload, unsigned int length) {
       Serial.print("Message arrived in topic: ");
       Serial.println(topic);
       Serial.print("Message: ");
       String message;
       for (int i = 0; i < length; i++) {
           message += (char) payload[i];  // Convert *byte to string
       }
       Serial.print(message);
       if (message == "on" && !ledState) {
           digitalWrite(LED, HIGH);  // Turn on the LED
           ledState = true;
       }
       if (message == "off" && ledState) {
           digitalWrite(LED, LOW); // Turn off the LED
           ledState = false;
       }
       Serial.println();
       Serial.println("-----------------------");
   }
   ```

8. Complete code.

   ```c
   #include <ESP8266WiFi.h>
   #include <PubSubClient.h>
   
   // GPIO 5 D1
   #define LED 5
   
   // WiFi
   const char *ssid = "mousse"; // Enter your WiFi name
   const char *password = "qweqweqwe";  // Enter WiFi password
   
   // MQTT Broker
   const char *mqtt_broker = "broker.emqx.io";
   const char *topic = "esp8266/led";
   const char *mqtt_username = "emqx";
   const char *mqtt_password = "public";
   const int mqtt_port = 1883;
   
   bool ledState = false;
   
   WiFiClient espClient;
   PubSubClient client(espClient);
   
   void setup() {
       // Set software serial baud to 115200;
       Serial.begin(115200);
       delay(1000); // Delay for stability
   
       // Connecting to a WiFi network
       WiFi.begin(ssid, password);
       while (WiFi.status() != WL_CONNECTED) {
           delay(500);
           Serial.println("Connecting to WiFi...");
       }
       Serial.println("Connected to the WiFi network");
   
       // Setting LED pin as output
       pinMode(LED, OUTPUT);
       digitalWrite(LED, LOW);  // Turn off the LED initially
   
       // Connecting to an MQTT broker
       client.setServer(mqtt_broker, mqtt_port);
       client.setCallback(callback);
       while (!client.connected()) {
           String client_id = "esp8266-client-";
           client_id += String(WiFi.macAddress());
           Serial.printf("The client %s connects to the public MQTT broker\n", client_id.c_str());
           if (client.connect(client_id.c_str(), mqtt_username, mqtt_password)) {
               Serial.println("Public EMQX MQTT broker connected");
           } else {
               Serial.print("Failed with state ");
               Serial.print(client.state());
               delay(2000);
           }
       }
   
       // Publish and subscribe
       client.publish(topic, "hello emqx");
       client.subscribe(topic);
   }
   
   void callback(char *topic, byte *payload, unsigned int length) {
       Serial.print("Message arrived in topic: ");
       Serial.println(topic);
       Serial.print("Message: ");
       String message;
       for (int i = 0; i < length; i++) {
           message += (char) payload[i];  // Convert *byte to string
       }
       Serial.print(message);
       if (message == "on" && !ledState) {
           digitalWrite(LED, HIGH);  // Turn on the LED
           ledState = true;
       }
       if (message == "off" && ledState) {
           digitalWrite(LED, LOW); // Turn off the LED
           ledState = false;
       }
       Serial.println();
       Serial.println("-----------------------");
   }
   
   void loop() {
       client.loop();
       delay(100); // Delay for a short period in each loop iteration
   }
   ```



## Testing

1. Please use [Arduino IDE ](<https://www.arduino.cc/en/Main/Software>) to upload the complete code to ESP8266 and open the serial monitor.

   ![esp_con](https://assets.emqx.com/images/esp8266_connect_ssuccessful.png)

2. Establish the connection between the MQTTX client and MQTT Broker and send commands to the ESP8266.

   ![esp_con](https://assets.emqx.com/images/esp8266_control_led_publish.png)



## Summary

So far, we have successfully implemented remote control of the LED light using the NodeMCU ESP8266 and free public MQTT broker. This example only describes a simple scenario, while a more secure connection method and persistence of IoT data are needed in the actual projects.

Next, you can check out [The Easy-to-understand Guide to MQTT Protocol](https://www.emqx.com/en/mqtt-guide) series of articles provided by EMQ to learn about MQTT protocol features, explore more advanced applications of MQTT, and get started with MQTT application and service development.

## Resources

- [MQTT on ESP32: A Beginner's Guide](https://www.emqx.com/en/blog/esp32-connects-to-the-free-public-mqtt-broker)
- [How to Use MQTT on Raspberry Pi with Paho Python Client](https://www.emqx.com/en/blog/use-mqtt-with-raspberry-pi)
- [MicroPython MQTT Tutorial Based on Raspberry Pi](https://www.emqx.com/en/blog/micro-python-mqtt-tutorial-based-on-raspberry-pi)
- [ESP8266 Connects to MQTT Broker with Arduino](https://www.emqx.com/en/blog/esp8266-connects-to-the-public-mqtt-broker)
- [Upload Sensor Data to MQTT Cloud Service via NodeMCU (ESP8266)](https://www.emqx.com/en/blog/upload-sensor-data-to-mqtt-cloud-service-via-nodemcu-esp8266)



<section class="promotion">
    <div>
        Try EMQX Cloud for Free
        <div class="is-size-14 is-text-normal has-text-weight-normal">No credit card required</div>
    </div>
    <a href="https://accounts.emqx.com/signup?continue=https://cloud-intl.emqx.com/console/deployments/0?oper=new" class="button is-gradient px-5">Get Started →</a>
</section>
