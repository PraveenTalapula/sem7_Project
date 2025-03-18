<pre>
 // Include the library files
#define BLYNK_PRINT Serial
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#include <Servo.h>  // Install this library  
#include <Wire.h>

// Initialize the Serial display
#define BLYNK_TEMPLATE_ID "TMPL3JmGtjjLn"
#define BLYNK_TEMPLATE_NAME "Smart Bridge Useing IOT"
#define BLYNK_AUTH_TOKEN "naP63XGG9892_o84KGjH9uNsY15dZtwo"

char auth[] = "naP63XGG9892_o84KGjH9uNsY15dZtwo";  // Enter your Blynk Auth token
char ssid[] = "iot";  // Enter your WIFI SSID
char pass[] = "123456789";  // Enter your WIFI Password

BlynkTimer timer;

int SoilSensor = D7;
int buttonState = 1;

const int trigPin1 = D6;
const int echoPin1 = D7;
unsigned long startMillis;
unsigned long currentMillis;
const unsigned long period = 10000;
long duration1;
int distance1;
Servo servo;

void setup() {
  Serial.begin(9600);
  Blynk.begin(auth, ssid, pass, "blynk.cloud", 80);
  servo.attach(D0);
  Wire.begin(D2, D1);
  Serial.clear();
  Serial.setCursor(0, 0);
  Serial.print("Initializing...");
  delay(500);
  Serial.clear();
  Serial.setCursor(0, 0);
  Serial.print("IoT Smart Bridge");
  Serial.setCursor(0, 1);
  Serial.print("Bridge Monitoring");
  delay(300);
  Serial.clear();
  pinMode(trigPin1, OUTPUT);
  pinMode(echoPin1, INPUT);
}

void loop() {
  ultrasonic();
  if (buttonState == HIGH) {
    Serial.println("Water level HIGH");
    Serial.clear();
    Serial.setCursor(0, 0);
    Serial.print("HIGH ALERT");
    Serial.setCursor(0, 1);
    Serial.print("Water level: ");
    Serial.setCursor(13, 1);
    Serial.print(distance1);
    servo.write(180);
    widgetLED LED(v1);
    LED.off();
    widgetLED LED(v2);
    LED.on();
  } else {
    Serial.println("Water level low");
    Serial.clear();
    Serial.setCursor(0, 0);
    Serial.print("LOW ALERT");
    Serial.setCursor(0, 1);
    Serial.print("Water level: ");
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
  distance1 = duration1 * 0.207 / 2;
  Serial.println(distance1);
  buttonState = digitalRead(SoilSensor);
  delay(300);

  Blynk.virtualWrite(V0, distance1);
}

</pre>
