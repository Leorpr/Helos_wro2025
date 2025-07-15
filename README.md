Engineering materials
====

This repository contains engineering materials of a self-driven vehicle's model participating in the WRO Future Engineers competition in the season 2022.

## Content

* `t-photos` contains 2 photos of the team (an official one and one funny photo with all team members)
* `v-photos` contains 6 photos of the vehicle (from every side, from top and bottom)
* `video` contains the video.md file with the link to a video where driving demonstration exists
* `schemes` contains one or several schematic diagrams in form of JPEG, PNG or PDF of the electromechanical components illustrating all the elements (electronic components and motors) used in the vehicle and how they connect to each other.
* `src` contains code of control software for all components which were programmed to participate in the competition
* `models` is for the files for models used by 3D printers, laser cutting machines and CNC machines to produce the vehicle elements. If there is nothing to add to this location, the directory can be removed.
* `other` is for other files which can be used to understand how to prepare the vehicle for the competition. It may include documentation how to connect to a SBC/SBM and upload files there, datasets, hardware specifications, communication protocols descriptions etc. If there is nothing to add to this location, the directory can be removed.

## Introduction

This project is structured around two core components: a Raspberry Pi 5 for computer vision and decision-making, and an ESP32 microcontroller for motor and steering control. The system is designed for autonomous navigation tasks, such as wall detection and directional correction, ideal for competitions like WRO 2025.

The Raspberry Pi is equipped with a USB camera and runs a Python script using OpenCV. Each video frame is analyzed by dividing it into three zones (left, center, right) and applying a binary threshold to detect obstacles such as walls. Based on the visual analysis, the Raspberry Pi sends specific single-character commands to the ESP32 over a USB-serial connection. The commands used are:
	•	A — Advance (move forward)
	•	S — Stop
	•	B — Backward (reverse)
	•	R — Turn right
	•	L — Turn left
	•	C — Center the servo

The ESP32 receives these commands and directly controls:
	•	A DC motor, driven via an L298N motor driver module, for forward and backward movement.
	•	A servo motor, used for steering, which adjusts within a limited angular range to ensure precise turns.

The system is powered by two separate sources: a power bank connected to the Raspberry Pi and a LiPo battery supplying the motor driver. This split power strategy avoids interference and ensures consistent operation of both control and processing units.

Code is uploaded to the ESP32 using the Arduino IDE. The Raspberry Pi script is run through Python using Thonny or the terminal. The architecture cleanly separates high-level visual processing from low-level motor control, enabling responsive and intelligent movement based on real-time camera input.

