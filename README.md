# WRO-Vizdrive-2025
Documentation for Panamanian 🇵🇦 Team Vizdrive for WRO 2025
This file describes the full operation of an autonomous robot built for the **World Robotic Olympiad (WRO) 2025**, designed to navigate a closed circuit, detect and avoid obstacles using artificial vision and sensors, and mantaining a stable trajectory with gyroscopic correction.

## Project Overview
The robot automously drives a closed-loop circuit using the next components:
- **DC Motors** for propulsion (RWD)
- **Servo motor** for steering (front wheels)
- **PixyCam 2** for object recognition and color detection
- **Ultrasonic sensors** to avoid obstacles and maintain orientation
- **MPU gyroscope** with **PID control** to maintain a straight trajectory and stability
- **Color Sensors** for perceiving turns and directions
- **Wheel Encoder** measures the pulses per revolution PPR

---

## Hardware Components

| Component | Quantity | Description |
| ------------ | ------- | ------------------- |
| Arduino Mega 2560 | 1 | Main controller board |
| DC motor 1:48 | 1 | Motor for rear drive |
| Servo Motor SG90 | 1 | Front steering motors |
| L298N Motor Driver | 1 | Dual H-bridge motor driver |
| PixyCam 2 | 1 | Color and object recognition camera |
| TCS230 Color Sensor | 1 | I2C floor color recognition sensor |
| TCS34725 Color Sensor | 2 | Side color recognition sensor |
| DC motor 1:48 | 1 | Motor for rear drive |
| Servo Motor SG90 | 1 | Front steering motors |
| L298N Motor Driver | 1 | Dual H-bridge motor driver |
| PixyCam 2 | 1 | Color and object recognition camera |
| HC-SR04 Ultrasonic Sensors | 4 | Front and side distance sensors |
| MPU6050 | 1 | 3-axis gyroscope and accelerometer |
| 3.7V batteries | 2 | Power supply |
| Custom Chasis (RWD) | 1 | 3D designed robot frame and mount |

---

## Required Libraries

For all the functionalities, they are used the following libraries:

- `Servo` (standard)
- `NewPing` — for ultrasonic sensor management
- `MPU6050` — for gyroscopic data
- `Pixy2` — for PixyCam 2 integration

---

## Core Functionalities

### Motion System
- `driveForward()` – Moves the robot forward.
- `driveBackwards()` – Moves backward.
- `stopMotors()` – Stops all motors.
- `stopBrake()` – Stops all motors forcefully.
- `setSteeringAngle()` – Sets the angle for the servo motor steering.
- `keepCenter` – Mantains orientation and straight trajectory with PID control.

**Motor and Wheel Configuration:**
- Rear-wheel drive (DC motors)
- Front steering (servo-controlled angle)
- Base speed: `200` (PWM signal)
- Wheel Circumference: `10.0 cm`
- Pulses per revolution `32.0`

### Vision-Based Obstacle Evasion

- Uses **PixyCam 2** to detect colored objects.
- **Red objects → Evade to the right**
- **Green objects → Evade to the left**
- Size-based decision to determine proximity of objects.

### Distance Detection

- **Ultrasonic sensors** measure real-time distance to nearby walls or obstacles.
- Detects distance to wall when avoiding obstacles, and takes control priority in emergencies (overrides camera based decisions).

### Color Detection

- **TCS230 Color Sensor** is placed at the bottom, for detecting *blue* and *orange* signals that indicate the time to turn.
- Two TCS34725 Color Sensor are placed at both sides. For correcting obstacle color-based evasion and detecting the *magenta* signal for parking.

### Orientation Control

- Ultrasonic sensors detect the distance to both walls.
- MPU6050 gyroscope continously monitors the robot's real-time angle.
- PID control (Proportional and Derivative parameters) are used to determine the angle for the servo motor to maintain the center constantly and correct errors in orientation.

---

## Circuit Behavior

1. Vehicle leaves the parking lot with a calculated maneuver.
2. Starts advancing forward.
3. If an **object is detected** by Pixycam:
  - If it's **color-coded**:
    - Red → Turn right
    - Green → Turn left
4. After avoiding, use PID control to return to original heading.
5. The robot detects the color at the bottom, and performs turn when necessary.
6. The robot counts the turns, and detects the magenta signal for parking.

---

## Pin Configuration Summary

| Component | Arduino Pin |
|---------------|-------------|
| Wheel Encoder | 2 |
| DC Motor | INA: 3, INB: 4 |
| Servo Motor | 9 |
| Ultrasonic Front-Left | Trigger: 10, Echo: 11 |
| Ultrasonic Front-Right | Trigger: 12, Echo: 13 |
| Ultrasonic Left | Trigger: 14, Echo: 15 |
| Ultrasonic Right | Trigger: 16, Echo: 17 |
| PixyCam 2 | SPI interface (Default) |
| MPU6050 | I2C (SDA: 20, SCL: 21) |
| Bottom color-sensor | 22, 23, 24, 25, out: 26 |
| Left color-sensor | 22, 23, 24, 25, out: 26 |
| Left color-sensor | 22, 23, 24, 25, out: 26 |
