'''
//Include the library files
#define BLYNK_PRINT Serial
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#include <Servo.h> //Install this library  
#include <Wire.h> 

//Initialize the Serial display

#define BLYNK_PRINT Serial
#define BLYNK_TEMPLATE_ID           "TMPL3qzDoMAT_"
#define BLYNK_TEMPLATE_NAME         "implementation of smart bridge using IOT"
#define BLYNK_AUTH_TOKEN            "e-lhp4dDQKQX1_jHP-mI2FcSzyp3HZyr"
 

char auth[] = "e-lhp4dDQKQX1_jHP-mI2FcSzyp3HZyr";  //Enter your Blynk Auth token
char ssid[] = "iot ";  //Enter your WIFI SSID
char pass[] = "123456789";  //Enter your WIFI Password

BlynkTimer timer;


int SoilSensor  = D7;
int buttonState = 1;

const int trigPin1 =D6;
const int echoPin1 =D7;
unsigned  long startMillis;
unsigned  long  currentMillis;
const unsigned long period = 10000;
long duation1;
int distance1;
Servo servo;

void setup() {

  Serial.begin(9600);
  Serial.begin(9600);
  Serial.print("AT");
  Blynk.begin(auth, ssid, pass, "blynk.cloud", 80);
  servo.attach(D0);
  Wire.begin(D2,D1);
  Serial.begin();
  Serial.backlight();
   Serial.setCursor(0, 0);
   Serial.print("  Initializing  ");
   delay(500);
   Serial.clear();
    Serial.setCursor(0, 0);
   Serial.print("  iot  ");
   Serial.setCursor(0, 1);
    Serial.print("  bridge monitoring  ");
   delay(300);
   Serial.clear();
   pinMode(trigPin1,OUTPUT);
   pinMode(echoPin1,INPUT);
}

void loop(){
  ultrasonic();
  if (buttonState ==HIGH){
    Serial.printin("water level HIGH");
    Serial.clear();
    Serial.setCursor(0, 0);
    Serial.print(" HIGH ALERTS");
    Serial.setCursor(0, 1);
    Serial.print(" water level");
    Serial.setCursor(13, 1);
    Serial.print(distance1);
    servo.write(180);
    widgetLED LED(v1);
    LED.off();
    widgetLED LED(v2);
    LED.on();

  } else{
    Serial.printin("water level low");
    Serial.clear();
     Serial.setCursor(0, 0);
    Serial.print(" LOW ALERTS");
    Serial.setCursor(0, 1);
    Serial.print(" water level");
    Serial.setCursor(13, 1);
    Serial.print(distance1);
    servo.write(0);
    widgetLED LED(v1);
    LED.on();
    widgetLED LED(v2);
    LED.off();
  }
  }
  void ultrasonic() {
  digitalWrite(trigPin1, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin1, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin1, LOW);
  duration1 = pulseIn(echoPin1, HIGH);
  distnce1  = duration1*0.207 / 2;
  Serial.printin(distance1);
  buttonState = digitalRead(Soilsensor);
  delay(300);

  Blynk.virtualWrite(V0, distance1);

 
}
'''
