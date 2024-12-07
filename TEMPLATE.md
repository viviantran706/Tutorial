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

`brew install mosquitto`

Linux: Use your package manager:


`sudo apt install mosquitto`

Start the Broker:
Open a terminal and type:


`mosquitto`


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
|Soldering Station | Required for permanent connections, such as attaching wires to components or headers to the ESP32.|
|Screwdriver Set | For assembling or securing hardware components, such as mounting the ESP32 or motors. |
| Multimeter  | To measure voltage, current, and continuity for debugging electrical connections. |
| Wire Stripper/Cutter | Used for preparing jumper wires or connecting power leads. |

| Optional Tools | Purpose |
|3D Printer |For creating custom enclosures or mounting brackets. | 
| Hot Glue Gun | To secure components to the breadboard or enclosures. |


## Part 01: Name

### Introduction

Briefly introduce what  you are teaching in this section.

### Objective

- List the learning objectives of this section

### Background Information

Give a brief explanation of the technical skills learned/needed
in this challenge. There is no need to go into detail as a
separation document should be prepared to explain more in depth
about the technical skills

### Components

- List the components needed in this challenge

### Instructional

Teach the contents of this section

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
