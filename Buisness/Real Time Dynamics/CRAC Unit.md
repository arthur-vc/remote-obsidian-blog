# Documentation
This CRAC unit controller currently runs on an Arduino Nano ESP32. Using the digital pins convention rather than GPIO to define pins. 
> **Arduino Nano ESP32 Documentation**
> https://docs.arduino.cc/hardware/nano-esp32/

## Code
This is the current code running on the Arduino Nano ESP32, on version 2.3.8 of Arduino IDE. Note dependencies/libraries included:
```csharp title:CRAC-Unit-Controller-V5-2026.cpp
/*
TO BE USED WITH ARDUINO NANO ESP32
WRITTEN BY TAL CARMEL
FEW CRUCIAL ADDITIONS BY ARTHUR CARROLL

/*
BUTTONS:
POWER
TEMPDECREASE
TEMPINCREASE
FANSPEED
*/
// BUILT OFF : VERSION 4 - MAJOR REVAMP BY: TAL C.
// NOTES:
// - Buttons are now activated through a struct, meaning to create a new Button is as simple as defining
//   a new variable i.e: Button Power // sets a new button called Power, then to assign it a pin go to
//   the SetButton() function on line 275
// - To create new Button variables look at line 44-47 it's very simple
// - To test MQTT open the RasPi (Power for monitor is on the left side, give it a min to turn on)
// - Open 2 terminals, in one use command:
//    --> mosquitto_sub -t "test" // This will start the server, currently the bool usePassword on line 68 is false
//    --> mosquitto_pub -t "test" -m "Power" // This will send an MQTT message, available commands can be seen on line 90-115
// - Might have to change network variables around, they're on lines 53-63
// - To add more MQTT commands just follow the "else if" formatting I got in function
#include <IPAddress.h>
#include <ArduinoMqttClient.h>
#include <WiFi.h>
#include <IPAddress.h>
#include <ArduinoJson.h>
#include <ArduinoJson.h>
char payload[500] = {};

bool debug = true;

// PINS - USING DIGITAL DEFINITIONS FROM DATASHEET - NOT GPIO
#define PIN_POWER D5         
#define PIN_TEMP_DOWN A1    
#define PIN_TEMP_UP D7       
#define PIN_FAN D2           
#define PIN_LIGHT_SENSOR A0  // THANK YOU FOR PROPERLY NAMING THIS ONE ARTHUR I LOVE YOU (i still renamed it)


int analogValue;
bool fan_high;

// BUTTONS
typedef struct Button {
 int pin;
 void Toggle() {
   digitalWrite(pin, HIGH);
   delay(100);
   digitalWrite(pin, LOW);
 };
};

// To assign a button a pin put it in the function SetButtons() on line 275
// i.e. Power.pin = 5
Button Power;
Button TempDecrease;
Button TempIncrease;
Button FanSpeed;

//
//// NETWORKING START
//

IPAddress deviceIP(192, 168, 1, 172);
byte deviceMac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };
IPAddress gatewayIP(127, 0, 0, 1);

IPAddress mqttServerIP(192, 168, 1, 71);
int mqttServerPort = 1883;

// WIFI CREDS
String ssid = "TeraDC";
String pass = "T3r@DC_20#7";
int status = WL_IDLE_STATUS;

// MQTT CREDS
String mqttUser = "prototype1";
String mqttPass = "ta_prototype_one";
bool usePassword = false;
String topic = "server/crac";

WiFiClient wifiClient;
MqttClient client(wifiClient);

//
// NETWORK VARIABLE END
// NETWORK FUNCTIONS START
//

bool StringIncludes(String stringToCheck, String subString) {
 if (stringToCheck.indexOf(subString) >= 0)
   return true;
 else
   return false;
}

void MessageHandler(String message) {
 Serial.print("Handling MQTT Message: ");
 Serial.println(message);

 if (StringIncludes(message, "EXEC_POWER")) {
   Power.Toggle();
   Serial.println("Switching POWER ON/OFF");
   SendMessage("CRAC - Switching POWER ON/OFF");
 }

 else if (StringIncludes(message, "EXEC_TEMP_UP")) {
   TempIncrease.Toggle();
   Serial.println("Increased TEMP");
   SendMessage("CRAC - Increased TEMP");
 }

 else if (StringIncludes(message, "EXEC_TEMP_DOWN")) {
   TempDecrease.Toggle();
   Serial.println("Decreased TEMP");
   SendMessage("CRAC - Decreased TEMP");
 }

 else if (StringIncludes(message, "EXEC_FAN_SPEED")) {
   FanSpeed.Toggle();
   Serial.println("Switching FAN SPEED");
   SendMessage(String("CRAC - Switching FAN SPEED - FAN Status : ") + (fan_high ? "true" : "false"));
 }


 else if (StringIncludes(message, "EXEC_BASELINE")) {
   Baseline();
   Serial.println("Called Baseline()");
   SendMessage("CRAC - Called Baseline");

 } else {
   Serial.println("!! Message has no handling options !!");
   // running this creates infinte feedback loop... so dont run it: SendMessage("CRAC - Message has no handling options!");
 }
}

void OnNewMessage(int messageData) {
 Serial.println("\n============\n<< NEW MQTT MESSAGE >>");
 String message = "";
 while (client.available()) {
   message = message + ((char)client.read());
 }
 if (message != "") {
   if (debug) {
     Serial.println("");
     Serial.print("Received a message with topic '");
     Serial.print(topic);  // idk why this no work as it should display topic -> (client.messageTopic());
     Serial.print("', length ");
     Serial.print(messageData);
     Serial.print(" bytes:\n>>> ");
     Serial.println(message);
     Serial.println();
   }
   MessageHandler(message);
 } else {
   Serial.println("Null message");
 }
 Serial.println("============");
}

//
// NETWORKING FUNCTIONS
//

void SetupConnections() {
 SetupWifi();
 SetupMqtt();
}

void MaintainConnections() {
 client.poll();
 // Maintain WiFi
 while (WiFi.status() != WL_CONNECTED) {
   Serial.println("WiFi Disconnected! Attempting to Reconnect");
   SetupWifi();
 }

 // Maintain MQTT
 if (!client.connected()) {
   Serial.println("MQTT Disconnected! Attempting to Reconnect");
   SetupMqtt();
 }
}

void SetupWifi() {
 while (WiFi.status() != WL_CONNECTED) {

   Serial.println("Attempting to connect to WPA SSID: ");

   Serial.println(ssid);

   // Connect to WPA/WPA2 network:

   status = WiFi.begin(ssid, pass);

   // wait 10 seconds for connection:

   delay(10000);
 }
 Serial.println("Wifi is connected!");
}

void SetupMqtt() {
 //client.setClient(wifiClient);
 client.connect(mqttServerIP, mqttServerPort);
 if (usePassword) {
   client.setUsernamePassword(mqttUser, mqttPass);
 }
 while (!client.connect(mqttServerIP, mqttServerPort)) {
   Serial.println("MQTT connection failed! Error code = ");
   Serial.println(client.connectError());
   delay(1000);
 }
 client.onMessage(OnNewMessage);
 client.subscribe(topic);

 if (debug) {
   Serial.print("connected to ");
   Serial.print(mqttServerIP);
   Serial.print(":");
   Serial.println(mqttServerPort);

   Serial.print("MQTT topic set to: ");
   Serial.println(topic);
   Serial.println('\n');
 }
}
// SEND MQTT MESSAGE
void SendMessage(String msg) {
   JsonDocument doc;
 doc["crac_response"] = msg;
 serializeJson(doc, payload);
 client.beginMessage(topic);
 client.print(payload);
 client.print("");
 client.endMessage();

 Serial.print("SENT MESSAGE: ");
 Serial.println(payload);
}


//
//// NETWORKING END
//

// FOR DEBUGGING PURPOSES ONLY
void SerialManagerInput() {
 // parse the IDE serial for a number - if corresponding to a pinout, send a current which will activate the transistor
 int x = Serial.parseInt();
 switch (x) {
   case 1:
     Power.Toggle();
     break;
   case 2:
     TempDecrease.Toggle();
     break;
   case 3:
     TempIncrease.Toggle();
     break;
   case 4:
     FanSpeed.Toggle();
     break;
   case 5:
     Baseline();
     break;
 }
}

void setup() {
 Serial.begin(9600);
 SetPinMode();        // SET THE PINS FOR OUTPUT
 SetButtons();        // SET PINS FOR THE BUTTONS
 SetupConnections();  // NETWORKING
}

void loop() {
 MaintainConnections();  // NETWORKING

 SerialManagerInput();  // GET INPUT FROM SERIAL MANAGER (FOR TESTING)

 LedCheck();  // CHECK LED - ONLY FOR DEBUGGING ATM
}

//FUNCTIONS

void SetPinMode()  // SETS PINS TO OUTPUT
{
 pinMode(PIN_POWER, OUTPUT);
 pinMode(PIN_TEMP_DOWN, OUTPUT);
 pinMode(PIN_TEMP_UP, OUTPUT);
 pinMode(PIN_FAN, OUTPUT);
}

void SetButtons() { // SETS UP BUTTON CLASS
 Power.pin = PIN_POWER;
 TempDecrease.pin = PIN_TEMP_DOWN;
 TempIncrease.pin = PIN_TEMP_UP;
 FanSpeed.pin = PIN_FAN;
}


// BASELINES THE CRAC UNIT - REDUCES TEMP AS LOW AS IT CAN GO - SETS FAN SPEED TO LOW
void Baseline() {
 for (int x = 0; x <= 21; x++) {
   TempDecrease.Toggle();
   delay(50);
 }
 LedCheck();
 if (fan_high == true) {
   FanSpeed.Toggle();
   delay(100);
 }
}
//15 - 5 up to go to 21

void LedCheck() {
 analogValue = analogRead(PIN_LIGHT_SENSOR);
 //Serial.println(analogValue);
 delay(1000);
 if (analogValue < 300 && analogValue >= 0) {
   Serial.println("LED is OFF");
   fan_high = false;
   Serial.println(analogValue);
 } else if (analogValue >= 300 && analogValue <= 1000) {
   Serial.println("LED is ON");
   fan_high = true;
   Serial.println(analogValue);
 } else {
   Serial.println("ERROR unkown range - returning last value");
   Serial.println(analogValue);
 }
}
```
Check the gitea for a more up to date version if it comes through.
## Notes
It's currently important to refrain from spamming commands after the CRAC unit powers up, so we've tried to add a small 1 second delay whenever you turn on or off the CRAC to protect other status's from desyncing. Its important to note that any attempt made to read volatge across LEDs on the CRAC PCB is impossible, as shortcircuiting the LED activates some other function, which implies the PCB monitors the LED voltage for some implementation, and any current leak will affect this reading. 