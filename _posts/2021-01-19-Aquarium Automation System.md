---
title: "Aquarium Automation System"
permalink: /projects/aquarium-automation-system/
excerpt: "Aquarium Automation System is Automated management and monitoring systems that consisted of Arduino and mobile application."
tags: [arduino, app-inventor, aquarium-automation]
use_math: true
toc: true
---

# **Overview**

* Team project that participated in CP-CoP(Creative Project-Community of Practice) competition
* Embedded System that perform monitoring and actuating all-based on sensing, linked with mobile application
* If you want to know more information about source code, visit my **[Github](https://github.com/KeunJuSong/Aquarium-Automation-System)**

# **Idea**

* Simple form of IoT(Internet of Things) architecture
* Programming sensor value with additional operations to express in unit (ex. pH, Celsius)
* Function that communicate with mobile application 

# **Description**

## **System Architecture**

* System is consisted of H/W and Application
* **H/W**
  * Arduino UNO Board (Leonardo is also allowed)
  * Arduino Sensor Shield
    * Extended board that can add more sensors
  * Tempreature sensor
  * pH sensor
  * Photo resistor (Light sensor)
  * Real-Time Clock (RTC) module
  * 2ch-Relay module
    *  For actuate auto-feeder and LED
  * LED (Compatible with Arduino)
  * Auto-Feeder (Model: **AF-2003**)

* **Mobile Application**
  * Monitoring temperature and pH value
  * Control On/Off LED and auto-feeder
  * Notify when temperature or pH value is not up to requirement of water quality

## **System Function**

* System have 3 main functions to maintain required water quality
  * **Auto-Feed**
    * Feeding itself in specific time, by setting RTC and Relay module 
  * **Auto-Control pH**
    * Control pH of water itself by sensing pH value (from pH sensor) in real-time
  * **Auto-LED On/Off**
    * Control LED On/Off itself by sensing light value from photo resistor
    * Setting value to compare with sensing value → Turn on LED when sensing value is less than setting value 

## **Result**

* **Pamphlet**

  <figure>
    <img src="{{ '/assets/images/[CP-CoP]_Auqarium-Automation-System-pamphlet.png' | relative_url }}" alt="Aquarium Automation System Pamphlet">
    <figcaption>Pamphlet of Auqarium Automation System</figcaption>
  </figure>

* **Final Model (in CP-CoP exhibition)**

  <figure>
    <img src="{{ '/assets/images/[CP-CoP]_Auqarium-Automation-System-model.jpg' | relative_url }}" alt="Aquarium Automation System Final Model">
    <figcaption>Auqarium Automation System in CP-CoP exhibition</figcaption>
  </figure>

# **Development Environment**

* Arduino IDE
* App Inventor 2.0

# **Reference**

* [Aquarium Control System DIY blog](https://slowknight.tistory.com/6)
* [Hackster.io](https://www.hackster.io/)
