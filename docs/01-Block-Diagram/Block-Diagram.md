---
title: Individal Block Diagram - Zane’s Subsystem - Smart Curtain Control
tags:
- tag1
- tag2
---

# Overview

This subsystem represents **Zane Brauer's** portion of **Team 206’s Smart Curtain Control** project.  
It focuses on detecting the room temperature using a temperature sensor through the **Microchip PIC18F57Q43 Curiosity Nano** microcontroller. 

---

# System Description

This system has a **Temperature sensor** that provides a analog input to the MCU through ADC pins. The system also has a **Button** for debugging or in otherwords recalibration.
The microcontroller takes the inputs from the **Temperature sensor** which outputs it to the **red LED indicator** and **Analog connector.** The connector leads back to the motor allowing 
the cutrain to open or close. The **red LED indicator** provides visual feedback on system status. The **Button** input goes into the Microcontroller, specifically the Digital I/O allowing for a recalibration to occur.

---

# Power Configuration

The design uses the voltage level of: **5V, 1.5A regulated supply** for logic and sensors.  

---

# Signal Connections

- **PWM Outputs (RA1, RC2):** Outputs Red LED Indicator and outputs temperature sensor input to analog connector 
- **Digital I/O Ports (RA1, RB2, RC4):** Routed through the 8-pin connector for inter-board communication and expansion  
- **Connector Pins:**
  - Pins **1–5:** Digital signals  
  - Pins **6–7:** Analog sensor data  
  - Pin **8:** Ground (GND)

Each connection is clearly labeled with its **direction** and **signal type** to maintain compatibility and simplify system integration.

---


## Block Diagram 
Showing an example of how to import a screenshot of the block diagram created outside of git and brought into a page.

![Example of Indivial Block diagram ](Block_Diagram.PNG)
