# Autonomous Robot for WRO Future Engineers

This repository contains the complete solution for an autonomous robot designed for the **WRO Future Engineers** competition. The robot uses a **Raspberry Pi 5** with a **12MP Arducam camera (IMX708)** for computer vision and an **ESP32** for motor and servo control. The system relies on color-based decision-making and wall detection to navigate the track.

## System Overview

- **Raspberry Pi 5**
  - Handles camera input and image processing with Picamera2 and OpenCV
  - Sends movement commands via serial to the ESP32

- **ESP32**
  - Controls one DC motor using an L298N driver
  - Controls one steering servo
  - Handles start/stop button logic

- **Sensors & Actuators**
  - DC motor connected to L298N (IN1 = GPIO 18, IN2 = GPIO 19, ENA = GPIO 21)
  - Servo connected to GPIO 25
  - Button connected to GPIO D5 (active low)
  - USB serial communication between Raspberry Pi and ESP32

## Navigation Logic

- The track is completely white with black walls (10 cm high).
- At corners, the robot detects a **blue** or **orange** line:
  - If **blue** appears first, the robot turns **left**
  - If **orange** appears first, it turns **right**
- After each turn, the robot moves forward and re-centers using wall detection.

## Start/Stop Control

- The robot waits for the user to press the button (connected to D5 on ESP32).
- Upon first press: the robot starts the logic and motion
- On second press: the robot stops all operations

## Installation

### On Raspberry Pi

1. Install dependencies:

   ```bash
   sudo apt update
   sudo apt install python3-opencv python3-picamera2
   ```

2. Enable the camera:

   ```bash
   sudo raspi-config
   ```

3. Create a systemd service to auto-start the script:

   ```bash
   sudo nano /etc/systemd/system/robot.service
   ```

   Add:

   ```ini
   [Unit]
   Description=Robot Vision Script
   After=network.target

   [Service]
   ExecStart=/usr/bin/python3 /home/pi/Desktop/ESTOSSI/lineas.py
   WorkingDirectory=/home/pi/Desktop/ESTOSSI
   StandardOutput=inherit
   StandardError=inherit
   Restart=always
   User=pi

   [Install]
   WantedBy=multi-user.target
   ```

   Then enable it:

   ```bash
   sudo systemctl daemon-reexec
   sudo systemctl daemon-reload
   sudo systemctl enable robot.service
   sudo systemctl start robot.service
   ```

### On ESP32

1. Flash the firmware using Arduino IDE.
2. Required pins:
   - Motor: IN1 = GPIO 18, IN2 = GPIO 19, ENA = GPIO 21
   - Servo: GPIO 25
   - Start/Stop Button: GPIO D5
3. Commands received via serial:
   - `'A'` = Move forward
   - `'S'` = Stop
   - `'L'` = Turn left
   - `'R'` = Turn right
   - `'C'` = Center servo

## Project Files

- `lineas.py`: Main computer vision script
- `functions.py`: Custom image-processing and control functions
- `masks.py`: HSV masks for color detection (blue and orange)
- `esp32_control.ino`: ESP32 firmware

## Requirements

- Raspberry Pi OS 64-bit
- Python 3.9 or higher
- OpenCV 4+
- Picamera2
- Arduino IDE 2.x

## Track Description

- White floor
- 10 cm tall black side walls
- Colored corner lines (blue = left turn, orange = right turn)