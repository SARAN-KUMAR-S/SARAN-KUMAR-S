
# Smart Street Light System

An ESP32-based intelligent street lighting system that automatically adjusts brightness based on ambient light conditions and motion detection. Features IoT control through Blynk.

## Features
- Automatic day/night detection using LDR
- Motion detection using ultrasonic sensor
- Automatic brightness control
- Remote control via Blynk app
- Energy-efficient operation

## Hardware Requirements
- ESP32 development board
- LED
- LDR (Light Dependent Resistor)
- HC-SR04 Ultrasonic Sensor
- Resistors and wiring

## Software Requirements
- Arduino IDE
- Libraries:
  - WiFi
  - BlynkSimpleEsp32

## Setup
1. Install required libraries in Arduino IDE
2. Configure WiFi credentials in code
3. Set up Blynk account and update authentication token
4. Upload code to ESP32

## Pin Configuration
- LED: GPIO 18
- LDR: GPIO 32
- Ultrasonic Trigger: GPIO 5
- Ultrasonic Echo: GPIO 19
