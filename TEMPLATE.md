---
title: Vegetation Protection Unit
date: 2024-12-02
authors:
  - name: Vivian Tran
  - name: Akhil Kalakota
  - name: Amdadul Haque
---

![relevant graphic or workshop logo](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f7/Squirrel%2C_Manyara_National_Park%2C_Tanzania_%282010%29.jpg/330px-Squirrel%2C_Manyara_National_Park%2C_Tanzania_%282010%29.jpg)


## Introduction
This tutorial aims to demonstrate how to use an ESP board to create a responsive system that triggers strobe deterrents based on real-time animal detection through machine learning. It provides step-by-step instructions on integrating the ESP board with a machine learning model to classify animals and activate adaptive deterrents effectively. 

The motivation for this project stems from a group member's experience with wildlife damaging their garden, leading to frustration and the need for a solution. This inspired the development of a system to safely and effectively prevent wildlife from ruining gardens, ensuring that plants and vegetation are protected without causing harm to the animals. The goal is to provide a practical tool for others facing similar challenges, combining technology and ethical practices to safeguard gardens from persistent wildlife interference. By the end of the tutorial, readers will have the knowledge and tools to create their own wildlife deterrent system to protect gardens effectively.


### Learning Objectives

- Learn how to set up wireless communication (e.g., Wi-Fi or MQTT) on the ESP board for remote monitoring and control.
- Understand how to trigger outputs like strobe lights, buzzers, or water sprinklers using the ESP board in response to detection events.
- Learn how to write and organize code for running multiple tasks simultaneously (e.g., data collection, classification, and output triggering).

*This project will not include a Rasberry Pi AI camera, that was orginally used in the full project, which was supported by a Rasbery Pi 4. This camera is the sensor to send information to the machine learning which activates the deterents. This tutorial is only on the ESP32 board that control the deterents and how wireless connects to a ccomputer or laptop.*

### Background Information

This project demonstrates how to build a responsive wildlife deterrent system using an ESP32 board. The system:
 - Activates adaptive deterrents,  water sprinklers, to safely discourage animals from damaging gardens or property.
 - Supports remote monitoring and control through Wi-Fi or MQTT for enhanced user interaction.

We are using the ESP32 board because of its integrated Wi-Fi and Bluetooth capabilities, enabling seamless wireless connectivity for remote monitoring and control. Additionally, its versatile hardware features built-in GPIO pins, making it ideal for interfacing with sensors and outputs such as DC motors and LEDs. 
Some other things that are commonly used is an Ardiuno, or a Rasberry Pi. 

Using an Ardiuno:
 - Pros: Simple to program, low bar of entry, and widely supported by a community
 - Cons: No Bluetooth or WiFi; therefore would need additional hardware for the wirless connection
   
Using a Rasberry Pi:
 - Pros: More powerful, capable of running full machine model learning, and can support advance camera
 - Cons: Higher cost, less sutitable for low-power

To understand and implement this system, it’s crucial to grasp the following concepts:
 - Connecting and Powering an ESP32: How to safely power the ESP32 using batteries (e.g., Li-ion or LiPo) while ensuring compatibility with connected hardware like motors and LEDs.
 - Convolution and Responsive Design: Programming event-driven responses (e.g., strobe lights) based on sensor inputs or ML predictions.
 - Wireless Communication (Wi-Fi or MQTT): Setting up remote control and monitoring to receive alerts or adjust system behavior.

## Getting Started
Software Prerequisites:
- Ardiuno ID, The Arduino IDE is a user-friendly platform for writing, uploading, and managing code on microcontroller boards like the ESP32. It includes libraries and tools to simplify development. It’s used to write and upload the code to the ESP32 for controlling hardware like motors and sensors.
- MQTT, MQTT is a lightweight messaging protocol for IoT devices, enabling communication between the ESP32 and a remote server or client. We are using MQTT this is a way to send alerts or control the system remotely using Wi-Fi, MQTT facilitates reliable data exchange.

Hardware Prerequisites:
| Component Name | Quanitity | 
| -------------- | --------- | 
|  ESP32 Board |1||
|  Motor Sheild |1| 
|  DC Motor |1| 
|  9v Battery  |2| 
|  Battery Connector  |2| 
|  Bread Board  |1| 
|  Wires (FF, FM, MM)  |10| 
|  Bread Board Power Module  |1| 


![GPIO Guide ESP32 Board](https://www.electronicwings.com/storage/PlatformSection/TopicContent/424/icon/ESP32%20GPIO%20Banner%20Image.jpg)
![relevant graphic or workshop logo](https://ece-196.github.io/docs/assignments/spinning-and-blinking/arduino/images/motion-pinout.png)
![References Sheet for DC Motor](https://www.baldor.com/mvc/DownloadCenter/Files/9AKK107331)

![References Guide for Bread Board Power Module](https://handsontec.com/dataspecs/mb102-ps.pdf)

### Required Downloads and Installations
#### Arduino IDE

Download: Visit the official Arduino website to download the IDE: 
![Arduino IDE Download](https://www.bing.com/search?pglt=425&q=arduino+ide+download&cvid=c3fd347b12954317b5f4acd093cdf87d&gs_lcrp=EgRlZGdlKgYIABAAGEAyBggAEAAYQDIGCAEQABhAMgYIAhAAGEAyBggDEAAYQDIGCAQQABhAMgYIBRAAGEAyBggGEAAYQDIGCAcQABhAMgYICBAAGEDSAQc5ODlqMGoxqAIAsAIA&FORM=ANNTA1&PC=ASTS)

Installation Tutorial:

Windows: Run the downloaded installer and follow the prompts. Be sure to allow USB driver installation.
macOS: Drag the Arduino IDE file to the Applications folder.
Linux: Extract the downloaded file and execute the install script via terminal (sudo ./install.sh).
Setup ESP32 Board Library:
Open Arduino IDE and navigate to File > Preferences.
Paste the following URL into Additional Board Manager URLs:
arduino
Copy code
<ins>https://dl.espressif.com/dl/package_esp32_index.json</ins>
Go to Tools > Board > Board Manager, search for "ESP32," and click Install.




#### MQTT Broker
DownloadInstall 
![Mosquitto](https://mosquitto.org/)
a popular MQTT broker: Mosquitto Downloads.

Installation Tutorial:

Windows: Download the installer, run it, and follow the instructions.

macOS: Install using Homebrew:

```brew install mosquitto```

Linux: Use your package manager:


```sudo apt install mosquitto```

Start the Broker:
Open a terminal and type:

```mosquitto```


#### ESP32 Board Libraries:

What is it?
The ESP32 board library provides the necessary files for programming and uploading code to the ESP32 using the Arduino IDE.
Why do you need it?
This ensures the Arduino IDE recognizes the ESP32 and provides the correct tools for compilation and uploading.
Installation: Add the URL **https://dl.espressif.com/dl/package_esp32_index.json** in the Arduino IDE's Board Manager settings, then search for and install the ESP32 library.



### Required Tools and Equipment

| Component Name | Purpose | 
| -------------- | --------- | 
| Computer (Windows, macOS, or Linux)| Used for writing, uploading, and debugging code on the ESP32 board. |
| Soldering Station | Required for permanent connections, such as attaching wires to components or headers to the ESP32.|
| Screwdriver Set | For assembling or securing hardware components, such as mounting the ESP32 or motors. |
| Multimeter  | To measure voltage, current, and continuity for debugging electrical connections. |
| Wire Stripper/Cutter | Used for preparing jumper wires or connecting power leads. |

| Optional Tools | Purpose |
|----------------|---------|
|3D Printer |For creating custom enclosures or mounting brackets. | 
| Hot Glue Gun | To secure components to the breadboard or enclosures. |


## Part 01: DC Moter (Water Deterent)

### Introduction

In this section, we will guide you through the process of building and programming a water-based wildlife deterrent using a DC motor and the ESP32 board. You’ll learn how to set up the motor to drive a mechanical device (like a rotating water sprayer) and how to control it in response to real-time animal detection. This system combines hardware assembly and programming, resulting in a responsive and humane method to protect your garden or property.

### Objective
By the end of this section, you will be able to:

Understand DC Motor Basics:

 - Learn how a DC motor works and its applications in deterrent systems.
Understand the role of motor drivers in controlling speed and direction.
Assemble a Motor Control Circuit:
 - Connect a DC motor to the ESP32 board using an L298N motor driver.
Properly wire power sources, motors, and the control board.
Program Motor Control:
 - Write code to control the motor's speed and direction using the ESP32.
Use PWM (Pulse Width Modulation) for speed control and GPIO pins for direction.
Integrate the Motor into the System:
 - Link motor activation to animal detection events.
Simulate the system to test responsiveness and adjust parameters for optimal performance.
Troubleshoot Common Issues:
 - Learn how to diagnose and resolve hardware and software issues, such as motor stalling or incorrect connections.

### Background Information

In this challenge, you will develop and apply the following technical skills:

 - Circuit Assembly: Learn how to connect a DC motor, motor driver, and ESP32 board into a functional circuit.
 - Motor Control Programming: Use PWM (Pulse Width Modulation) and GPIO pins to control motor speed and direction through code.
 - Integration and Testing: Link motor activation to system events (e.g., animal detection) and simulate operations to ensure functionality.
 - Debugging and Troubleshooting: Diagnose common issues like wiring errors, incorrect motor driver connections, or software misconfigurations.
 - These skills are fundamental for creating systems that respond to environmental triggers using physical components like motors.

### Components

| Component Name | Description | 
| -------------- | --------- | 
| ESP32 Board | Microcontroller to control the motor based on detection events. |
| DC Motor | Powers the water sprayer or other mechanical deterrents.|
| DC Motor Sheild | Interfaces between the ESP32 and the DC motor, providing speed and direction control. |
| 9v Batteries | Provides power to the motor and ESP32. |
| Breadboard | Allows for easy prototyping of the circuit without soldering. |
| 3D Waterproof Enclosure | 	(Optional) Protects the components from water damage during operation |


### Instructional

1. Assemble the Circuit
   
Set up the motor driver:

 - Connect the DC motor terminals to the motor output pins on the L298N driver (OUT1 and OUT2).
 - Connect the motor driver’s power input (VCC and GND) to the battery terminals. Ensure the motor driver can handle the motor's voltage.
 - Attach the motor driver's control pins (e.g., IN1 and IN2) to GPIO pins on the ESP32.

Power the ESP32:

 - Connect the ESP32 to the battery or an external power source via USB. Ensure the voltage matches the ESP32's requirements (typically 3.3V or 5V).
   
Wiring the Breadboard:

 - Use jumper wires to link the motor driver, ESP32, and motor through the breadboard for easy adjustments.

   
2. Write and Upload the Code
Set up the Arduino IDE:

Ensure the ESP32 board is added to the IDE. Install necessary libraries for PWM control.
Define GPIO pins for the motor driver control (e.g., IN1 and IN2).
Write the code:

Use PWM to control the motor speed:
```
#define LED 2          // Define the pin for the LED (optional)
#define MTR_HI 5       // Pin to control the high side of the motor
#define MTR_LO 18      // Pin to control the low side of the motor

int base_value = 0;                 // PWM value starts at 0
int step_value_for_base_value = 10; // Faster ramp-up
int time_for_delay = 5;             // Reduced delay for faster acceleration


void setup() {
// Initialize pins for motor and led
  pinMode(LED, OUTPUT);
  pinMode(MTR_HI, OUTPUT);
  pinMode(MTR_LO, OUTPUT);

  analogWrite(MTR_HI, 0);
  analogWrite(MTR_LO, 0);
}

void loop() {
// LED Blinking
  digitalWrite(LED, HIGH); // Turn the LED on
  delay(150);
  digitalWrite(LED, LOW);  // Turn the LED off
  delay(150);

 // Run the motor at full speed forward to squeeze the bottle
  analogWrite(MTR_HI, 255); // Immediate full speed
  analogWrite(MTR_LO, 0);   // Ensure motor spins in one direction
  delay(1000);              // Keep squeezing for 1 second

  // Stop the motor to let it reset
  analogWrite(MTR_HI, 0);
  analogWrite(MTR_LO, 0);
  delay(1000);

 // Smooth acceleration to full speed
  while (base_value <= 255) {
    analogWrite(MTR_HI, base_value);
    analogWrite(MTR_LO, 0);
    base_value += step_value_for_base_value;
    delay(time_for_delay);
  }

  // Smooth deceleration to stop
  while (base_value >= 0) {
    analogWrite(MTR_HI, base_value);
    analogWrite(MTR_LO, 0);
    base_value -= step_value_for_base_value;
    delay(time_for_delay);
  }

}
```
Upload the code:
 - Connect the ESP32 to your computer, select the correct port, and upload the code via the Arduino IDE.
![image](https://github.com/user-attachments/assets/95855d85-f18e-4c8e-94de-9d25779158b5)
*TIP: Make sure it is the apporiate board!!!*

   
3. Test the System
Power the setup using the battery.
Observe the motor's behavior:
Verify the motor turns on and off as programmed.
Adjust the speed in the code (modify analogWrite() values) if needed.

5. Link to Detection Events
   
Modify the code to integrate with the detection logic. Replace the manual motor control logic with a condition triggered by the detection system:

```
void sprayWater() {
    digitalWrite(LED_PIN, HIGH); // Turn on LED during spray
    analogWrite(MTR_HI, 255);    // Activate motor for water spray
    analogWrite(MTR_LO, 0);
    delay(5000);                 // Spray for 5 seconds
    analogWrite(MTR_HI, 0);      // Stop motor
    analogWrite(MTR_LO, 0);
    digitalWrite(LED_PIN, LOW);  // Turn off LED after spray
```
In Void Loop:
```
while (squirrelDetected) {
  playTone(20000, 5000);
  delay(2000);
  sprayWater();
  delay(2000);
}
```

## Part 02: MQTT Wireless Setup Between Computer and ESP32

### Introduction

In this section, we will teach you how to set up MQTT (Message Queuing Telemetry Transport) for wireless communication between an ESP32 and a computer. You’ll learn how to configure an MQTT broker, program the ESP32 to publish and subscribe to messages, and use an MQTT client on your computer to send and receive data. This forms the backbone for remote monitoring and control of IoT devices like the wildlife deterrent system.


### Objective

By the end of this section, you will be able to:

Understand MQTT Basics:

 - Learn the role of MQTT in IoT systems, including brokers, publishers, and subscribers.
   
Set Up an MQTT Broker:

- Install and configure an MQTT broker (e.g., Mosquitto) on your computer to manage communication between devices.
  
Program the ESP32 for MQTT:

 - Write code to enable the ESP32 to publish and subscribe to topics.
   
Test the Setup:

 - Use an MQTT client to simulate data exchange and verify communication.

Integrate MQTT with System Events:

 - Link the ESP32's responses (e.g., deterrent activation) to commands or messages received via MQTT.


### Background Information

In this challenge, participants will develop the following technical skills:

 - Wireless Communication: Understanding how MQTT enables lightweight, efficient communication between devices.
 - ESP32 Networking: Configuring the ESP32 for Wi-Fi and connecting it to an MQTT broker.
 - Message Handling: Learning how to publish sensor data and subscribe to commands.
 - Debugging Tools: Using MQTT clients (e.g., MQTT Explorer) to test and troubleshoot the system.
These skills are essential for building scalable IoT systems where devices need to communicate in real time.

### Components

| Component Name | Description | 
| -------------- | --------- | 
| ESP32 Board | Microcontroller with Wi-Fi for MQTT communication. |
| Computer (Windows, macOS, or Linux) | Runs the MQTT broker and client for monitoring and testing. |
| Wi-Fi Network | Required for connecting the ESP32 and computer to the same network. |

### Instructional

1. Set Up the MQTT Broker
   
Install Mosquitto Broker:

Download Mosquitto from here.
Follow the installation guide for your operating system:
Windows: Run the installer and ensure the Mosquitto service is enabled.
macOS: Use Homebrew to install:
```
brew install mosquitto
```
Linux: Use your package manager:

```sudo apt install mosquitto```

Start the Broker:

Open a terminal or command prompt and start Mosquitto:
```mosquitto```

By default, the broker will run on localhost (127.0.0.1) and port 1883.

2. Install an MQTT Client on Your Computer
Use an MQTT client like MQTT Explorer or the command-line tool mosquitto_sub and mosquitto_pub.
Test the broker by publishing and subscribing to messages:

Subscribe to a topic:

```mosquitto_sub -h localhost -t test/topic```
Publish a message:
```
mosquitto_pub -h localhost -t test/topic -m "Hello, MQTT!"
```
3. Configure the ESP32 for MQTT
Install the PubSubClient Library:

Open the Arduino IDE and go to Sketch > Include Library > Manage Libraries.
Search for "PubSubClient" and install it.
Write the ESP32 Code:

Use the following example to connect the ESP32 to the MQTT broker and publish/subscribe to messages:
```
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "YourWiFiSSID";
const char* password = "YourWiFiPassword";
const char* mqttServer = "192.168.1.100"; // Replace with your broker's IP
const int mqttPort = 1883;
const char* mqttTopic = "test/topic";

WiFiClient espClient;
PubSubClient client(espClient);

void setup() {
    Serial.begin(115200);
    WiFi.begin(ssid, password);
    while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.println("Connecting to WiFi...");
    }
    Serial.println("Connected to WiFi");

    client.setServer(mqttServer, mqttPort);
    while (!client.connected()) {
        if (client.connect("ESP32Client")) {
            Serial.println("Connected to MQTT Broker!");
        } else {
            Serial.print("Failed with state ");
            Serial.println(client.state());
            delay(2000);
        }
    }
    client.subscribe(mqttTopic);
}

void loop() {
    client.loop();  
    if (client.connected()) { //This is where we replace this code for the deterrent but for now we leave it like this to test if the connection works
        client.publish(mqttTopic, "ESP32 says hello!");
    }
    delay(5000);
}
```
Upload the Code:
 - Connect the ESP32 to your computer and upload the code using the Arduino IDE.

4. Test the System
   
- Open the MQTT client on your computer and subscribe to test/topic.
  
 - Observe messages sent by the ESP32.
   
 - Publish a message from the client and observe the ESP32's response.
   
6. Integrate with Detection Logic
Modify the ESP32 code to publish and subscribe to topics related to detection events. For example:

Publish: When an animal is detected:

```client.publish("deterrent/alert", "Animal detected!");```
 - Subscribe: To control the deterrent system remotely:

``` client.subscribe("deterrent/control");``` 
This section sets up the foundation for wireless communication, enabling participants to build a responsive system controlled remotely via MQTT.


## Example

### Introduction

Introduce the example that you are showing here.

### Example

Present the example here. Include visuals to help better understanding

### Analysis

Explain how the example used your tutorial topic. Give in-depth analysis of each part and show your understanding of the tutorial topic

## Additional Resources

### Useful links

List any sources you used, documentation, helpful examples, similar projects etc.
