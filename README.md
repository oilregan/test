# CODE

// Microgear

#include <AuthClient.h>

#include <debug.h>

#include <MicroGear.h>

#include <MQTTClient.h>

#include <PubSubClient.h>

#include <SHA1.h>


//node

#include <Arduino.h>

#include <ESP8266WiFi.h>

#include <ESP8266WebServer.h>

#include <ESP8266mDNS.h>

#include <EEPROM.h>


#include <DHT.h>

#include <DHT_U.h>


const char* ssid     = "iPhoneJT";

const char* password = "jak123456";


#define APPID   "Teat1"

#define KEY     "1fBtuelf2lcsuq9"

#define SECRET  "4aTlIwAkwNEkUcNf8ocR96bVX"


#define ALIAS   "NodeMCU1"

#define TargetWeb "HTML_web"


DHT dht(4, DHT11);

int moistureA0 = A0;


int moisture = 0;

int timer = 0;


WiFiClient client;
MicroGear microgear(client);


void onMsghandler(char *topic, uint8_t* msg, unsigned int msglen) 
{
    Serial.print("Incoming message --> ");
    msg[msglen] = '\0';
    Serial.println((char *)msg);
}


void onConnected(char *attribute, uint8_t* msg, unsigned int msglen) 
{
    Serial.println("Connected to NETPIE...");
    microgear.setName("moisture");
}

void setup() 
{

     /* Event listener */
    microgear.on(MESSAGE,onMsghandler);
    microgear.on(CONNECTED,onConnected);

    dht.begin();
    Serial.begin(115200);
    Serial.println("Starting...");

    WiFi.begin(ssid, password);
    while (WiFi.status() != WL_CONNECTED) 
    {
       delay(250);
       Serial.print(".");
    }

    Serial.println("WiFi connected");  
    Serial.println("IP address: ");
    Serial.println(WiFi.localIP());

    microgear.init(KEY,SECRET,ALIAS);
    microgear.connect(APPID);
}

void loop() 
{
  
    if (microgear.connected())
    {
       microgear.loop();
       timer = 0;

       Serial.println("connected");

       microgear.loop();
       moisture = analogRead( moistureAO );

       int tmp_moi = (int)((moisture - 424) / 6);
    if (tmp_moi > 100) {
      tmp_moi = 100;
    }

    if (tmp_moi < 0) {
      tmp_moi = 0;
    }

    tmp_moi = 100 - tmp_moi;
  
   /*
       int Humidity = dht.readHumidity();
       int Temp = dht.readTemperature();  // Read temperature as Celsius (the default)
        Serial.print(" Temperature: ");
        Serial.print(Temp);
        Serial.print(" C ");
        Serial.print(" Humidity: ");
        Serial.print(Humidity);
        Serial.print(" RH ");

        int soil= analogRead(S);
        // for Arduino
        //Checksoil = map(Checksoil,0,710,100,0);
        // for NodeWIFI
        soil = map(soil,0,1024,100,0);
        Serial.print("Moisture: ");
        Serial.print(soil);
        Serial.println(" %");
        */
        char msg[128];
    sprintf(msg,"%d,%d,%d,moisture", (int)Temp , (int)Humidity , (int)soil );
       /*String data = "/" + String(Humidity) + "/" + String(Temp);
       char msg[128];
       data.toCharArray(msg,data.length());*/
       Serial.println(msg);    

       microgear.chat(TargetWeb , msg);
    }
   else 
   {
    Serial.println("connection lost, reconnect...");
    microgear.connect(APPID);
    delay(timer);
    timer += 500;
   }
    delay(1500);
}



// Microgear 
#include <AuthClient.h>
#include <debug.h>
#include <MicroGear.h>
#include <MQTTClient.h>
#include <PubSubClient.h>
#include <SHA1.h>

//node
#include <Arduino.h>
#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>
#include <ESP8266mDNS.h>
#include <EEPROM.h>

#include <DHT.h>
#include <DHT_U.h>

const char* ssid     = "iPhoneJT";
const char* password = "jak123456";

#define APPID   "Teat1"
#define KEY     "1fBtuelf2lcsuq9"
#define SECRET  "4aTlIwAkwNEkUcNf8ocR96bVX"

#define ALIAS   "NodeMCU1"
#define TargetWeb "HTML_web"

DHT dht(4, DHT11);

int moistureA0 = A0;

int moisture = 0;
int timer = 0;

WiFiClient client;
MicroGear microgear(client);

void onMsghandler(char *topic, uint8_t* msg, unsigned int msglen) 
{
    Serial.print("Incoming message --> ");
    msg[msglen] = '\0';
    Serial.println((char *)msg);
}


void onConnected(char *attribute, uint8_t* msg, unsigned int msglen) 
{
    Serial.println("Connected to NETPIE...");
    microgear.setName("moisture");
}

void setup() 
{

     /* Event listener */
    microgear.on(MESSAGE,onMsghandler);
    microgear.on(CONNECTED,onConnected);

    dht.begin();
    Serial.begin(115200);
    Serial.println("Starting...");

    WiFi.begin(ssid, password);
    while (WiFi.status() != WL_CONNECTED) 
    {
       delay(250);
       Serial.print(".");
    }

    Serial.println("WiFi connected");  
    Serial.println("IP address: ");
    Serial.println(WiFi.localIP());

    microgear.init(KEY,SECRET,ALIAS);
    microgear.connect(APPID);
}

void loop() 
{
  
    if (microgear.connected())
    {
       microgear.loop();
       timer = 0;

       Serial.println("connected");

       microgear.loop();
       moisture = analogRead( moistureAO );

       int tmp_moi = (int)((moisture - 424) / 6);
    if (tmp_moi > 100) {
      tmp_moi = 100;
    }

    if (tmp_moi < 0) {
      tmp_moi = 0;
    }

    tmp_moi = 100 - tmp_moi;
  
   /*
       int Humidity = dht.readHumidity();
       int Temp = dht.readTemperature();  // Read temperature as Celsius (the default)
        Serial.print(" Temperature: ");
        Serial.print(Temp);
        Serial.print(" C ");
        Serial.print(" Humidity: ");
        Serial.print(Humidity);
        Serial.print(" RH ");

        int soil= analogRead(S);
        // for Arduino
        //Checksoil = map(Checksoil,0,710,100,0);
        // for NodeWIFI
        soil = map(soil,0,1024,100,0);
        Serial.print("Moisture: ");
        Serial.print(soil);
        Serial.println(" %");
        */
        char msg[128];
    sprintf(msg,"%d,%d,%d,moisture", (int)Temp , (int)Humidity , (int)soil );
       /*String data = "/" + String(Humidity) + "/" + String(Temp);
       char msg[128];
       data.toCharArray(msg,data.length());*/
       Serial.println(msg);    

       microgear.chat(TargetWeb , msg);
    }
   else 
   {
    Serial.println("connection lost, reconnect...");
    microgear.connect(APPID);
    delay(timer);
    timer += 500;
   }
    delay(1500);
}


