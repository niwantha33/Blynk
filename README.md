# Blynk
Sample codes for Blynk virtual pins 


# Code Descriptions 

This is to initilizing of Blynk connection with the Blynk server 

define BLYNK_PRINT Serial        // Comment this out to disable prints and save space
include <ESP8266WiFi.h>
include <BlynkSimpleEsp8266.h>

# Setting up of DHT sensor

You can use any DHT sensor, library avaialble at github ( Thanks ) 


include "DHT.h"
define DHTPIN 5     // pin D1 in Original Board 
define DHTTYPE DHT11   // DHT 11
DHT dht(DHTPIN, DHTTYPE);

# Global data setting

const int LED_pin = 16;                           //Inbuilt LED ( Only available in Original Board) 
float h =0.0;
float t =0.0;

# Wifi Connection details 

const char* ssid     = "TP";
const char* password = "12345678";

# You have to get this code from Blynk ( It is better to email it)

char auth[] = "xxxxxxxxxxxxxxxxxxxxxxxxxxx"; // Blynk authorization key

# To improve and speed up of compiling 

void setup();
void loop();


void setup()
{
  Serial.begin(11520);                                //baud rate should be set as 74880
  Serial.println("Start");
  pinMode(LED_pin, OUTPUT);  
  Blynk.begin(auth, ssid, password);
  delay(1000);
  dht.begin();
  delay(1000); // to stable the DHT 
  
  // LED blynking to indicate debugs
  digitalWrite(LED_pin, HIGH);                                // turn the LED on 
  delay(250);                                                // wait for a 250ms
  digitalWrite(LED_pin, LOW);                                 // turn the LED off 
  delay(250);
}
//:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

# Please Note: You can create any number of virtual pins to do anythings 

BLYNK_READ(V0){ // You have to make virtual pin in Mobile phone > Blynk 
   h = dht.readHumidity();
  Blynk.virtualWrite(V0, h);
}

BLYNK_READ(V1){
  t = dht.readTemperature();
  Blynk.virtualWrite(V1, t);
}

BLYNK_READ(V2){
  float hic = dht.computeHeatIndex(t, h, false);
  Blynk.virtualWrite(V2, hic);
}

//:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

void loop() {
  Blynk.run();        // Main function to run Blynk app
}
