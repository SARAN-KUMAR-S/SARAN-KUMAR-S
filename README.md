// Pin Definitions
const int ledPin = 18;            // LED pin
const int ldrPin = 32;            // LDR pin (digital)
const int trigPin = 5;            // Ultrasonic Trigger pin
const int echoPin = 19;           // Ultrasonic Echo pin

// Variables
int ldrState;                     // Store the state of the LDR
long duration;                    // Time for ultrasonic signal to return
int distance;                     // Distance measured by ultrasonic
const int lowIntensity = 50;      // Low intensity for LED (dim)
const int highIntensity = 255;    // High intensity for LED (bright)

#define BLYNK_PRINT Serial  // Enable serial printing for debugging
#define BLYNK_TEMPLATE_ID "TMPL30Lh45hTH"  // Blynk template ID
#define BLYNK_TEMPLATE_NAME "Led Control"  // Blynk template name
#define BLYNK_AUTH_TOKEN "D4IpwHsifGq4M_3wWPvyyYeOmoBsc-Un"  // Blynk authentication token

#include <WiFi.h>  // WiFi library for ESP32
#include <BlynkSimpleEsp32.h>  // Blynk library for ESP32

// Blynk authentication token and WiFi credentials
char auth[] = BLYNK_AUTH_TOKEN;
char ssid[] = "SRN";  // WiFi SSID
char pass[] = "01010101";  // WiFi password

// This function is triggered when virtual pin V0 in the Blynk app is updated
BLYNK_WRITE(V0) {
  int value = param.asInt();  // Get the value sent from the app (0 or 1)
  digitalWrite(18, value);  // Set the state of GPIO 2 (connected to the LED)
}


void setup() {
  // Setup Serial Monitor
  Serial.begin(115200);

  // Initialize the pins
  pinMode(ldrPin, INPUT);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(ledPin, OUTPUT); // Set LED pin as output

  Serial.println("Street Light System Ready");

  pinMode(18, OUTPUT);  // Set GPIO 2 as an output pin (for LED control)
  
  // Connect to WiFi and Blynk cloud server
  Blynk.begin(auth, ssid, pass, "blynk.cloud", 80);  // Correct syntax for port number
}

void loop() {
  ldrState = digitalRead(ldrPin);  // Read the state of LDR (0 = day, 1 = night)

  if (ldrState == HIGH) {  // Night time (LDR is HIGH)
    distance = measureDistance();  // Measure distance from ultrasonic sensor

    if (distance <= 10) {  // Object detected within 10 cm
      Serial.println("Object detected! Full brightness.");
      analogWrite(ledPin, highIntensity);  // Full brightness
    } else {
      Serial.println("No object. Low brightness.");
      analogWrite(ledPin, lowIntensity);   // Dim brightness
    }
  } else {
    // Day time, turn off the LED
    Serial.println("Daytime. LED off.");
    analogWrite(ledPin, 0);  // LED off
  }

  delay(10);  // Small delay to stabilize readings

  Blynk.run();  // Keep the Blynk connection alive

  
}

// Function to measure distance using ultrasonic sensor
int measureDistance() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  duration = pulseIn(echoPin, HIGH);
  return duration * 0.034 / 2;  // Convert time into distance
}
