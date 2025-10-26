---
title: Individal Block Diagram - Zane’s Subsystem - Smart Curtain Control
tags:
- tag1
- tag2
---

# Overview

This subsystem represents **Zane Brauer's** portion of **Team 206’s Smart Curtain Control** project.  
It focuses on detecting the room temperature using temperature sensing through the **Microchip PIC18F57Q43 Curiosity Nano** microcontroller.

---

# System Description

The system integrates **light and motion sensors** that provide analog inputs to the MCU through ADC pins, while a **digital button** offers manual control.  
The microcontroller processes these inputs and outputs control signals to the **H-Bridge driver**, which powers the **DC motor** responsible for opening and closing the curtain.  
A **red LED indicator** provides visual feedback on system status.

---

# Power Configuration

The design uses two voltage levels:
- **5V, 1.5A regulated supply** for logic and sensors  
- **9V, 3A unregulated source** for the H-Bridge and motor

---

# Signal Connections

- **PWM Outputs (RB1, RB2, RD0):** Control motor speed and direction  
- **Digital I/O Ports (RF5, RC4, RD1, RD2, RD3):** Routed through the 8-pin connector for inter-board communication and expansion  
- **Connector Pins:**
  - Pins **1–5:** Digital signals  
  - Pins **6–7:** Analog sensor data  
  - Pin **8:** Ground (GND)

Each connection is clearly labeled with its **direction** and **signal type** to maintain compatibility and simplify system integration.

---


## Example Block Diagram 
Showing an example of how to import a screenshot of the block diagram created outside of git and brought into a page.

![Example of Indivial Block diagram ](Block_Diagram.png)
