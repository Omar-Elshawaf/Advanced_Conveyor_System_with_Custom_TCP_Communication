
# Advanced Conveyor System with Custom TCP/IP Communication, MATLAB GUI, and WebSocket Monitoring & Control

## Overview
The **Advanced Conveyor System** integrates automated control with real-time telemetry and fault detection through a custom TCP/IP communication protocol. This system leverages **ESP32** as a central controller, communicating with a **MATLAB GUI** for operator interaction and a **WebSocket** interface for scalable remote monitoring. The project aims to enhance the automation and control of conveyor systems, utilizing advanced networking and fault-handling mechanisms for industrial applications.

## Features
- **Custom TCP/IP Communication**: Custom protocol implemented on both the ESP32 and the MATLAB GUI, supporting real-time telemetry and remote control.
- **MATLAB GUI**: Interactive control panel for operators to manage the conveyor system, including commands for start, stop, speed control (PWM), and item tracking.
- **WebSocket Monitoring**: Optional WebSocket-based dashboard for real-time telemetry visualization and fault monitoring.
- **Fault Detection & Reporting**: Proactive fault handling mechanisms that report errors such as motor stalls, sensor faults, and ACK timeouts.
- **Real-Time Telemetry**: Continuous updates of system parameters like speed, item count, sensor states, and operational status, sent over TCP/IP.

## System Architecture
The system consists of the following key components:
1. **ESP32**: Acts as the central controller for the conveyor system, handling motor control, sensor data acquisition, and communication over TCP/IP.
2. **DC Motor & Encoder**: Drives the conveyor and provides speed and distance measurements.
3. **IR Sensor**: Detects and counts items on the conveyor.
4. **Limit Switch**: Provides a homing reference to the conveyor system.
5. **Jetson Device**: Functions as a relay between the ESP32 and MATLAB, also simulating network congestion for testing.
6. **MATLAB GUI**: Provides a user interface for control and monitoring of the conveyor system.
7. **WebSocket Interface**: (Optional) A real-time dashboard for monitoring system parameters remotely.

## Network Communication
The system uses a custom TCP/IP protocol for communication between the **ESP32**, **Jetson**, and **MATLAB GUI**. Key messages include:
- **STATUS**: Periodically broadcasted by ESP32 to report system telemetry (speed, item count, error flags).
- **CMD**: Commands sent from MATLAB GUI to ESP32 (e.g., START, STOP, PWM control).
- **ACK**: Acknowledgments sent from Jetson to ESP32, ensuring reliable communication.
- **FAULT**: Error messages sent by ESP32 in case of faults like motor stalls or sensor issues.

### Message Example:
```json
{
  "type": "STATUS",
  "ts_ms": 120452,
  "seq": 31,
  "speed_rpm": -1,
  "encoder": 0,
  "items": 0,
  "limit": 1,
  "ir": 1,
  "err": 0,
  "state": "WAIT_HOME",
  "distance_cm": 0.00,
  "interval": 500,
  "congestion": 0,
  "rtt": 200
}
```

## Key Functionalities
### 1. **Automated Conveyor Control**:
   - The ESP32 controls the conveyor’s DC motor using **PWM**, based on the commands received from the **MATLAB GUI**.
   - **IR sensors** detect items and report counts, while the **encoder** provides speed and distance feedback.
   - The system can return to the home position using a **limit switch**.

### 2. **Fault Handling**:
   - **Motor Stall Detection**: If the motor is commanded to move but the encoder does not change for a set time, a "motor stall" fault is triggered.
   - **IR Sensor Fault**: If the IR sensor detects a stuck object, an error is reported.
   - **ACK Timeout**: If no acknowledgment is received within a specified time, an "ACK TIMEOUT" fault is triggered.

### 3. **Real-Time Monitoring**:
   - The **MATLAB GUI** displays real-time telemetry data, allowing operators to track system performance and detect faults.
   - The **WebSocket Interface** provides a remote dashboard for monitoring the system’s operational state in real-time.

## Network and Congestion Control
To ensure reliable communication and mitigate the effects of network congestion, the system:
- Monitors the **Round-Trip Time (RTT)** for each command.
- **Adjusts the transmission interval** dynamically based on the RTT, increasing when the RTT is high and reducing it when RTT is low.
- Flags **congestion** when transmission intervals exceed a threshold, providing a visible indication of network issues.

## Hardware Components
- **ESP32**: The central microcontroller that handles all automation logic, communication, and fault detection.
- **DC Motor Driver**: Controls the motor using PWM signals.
- **IR Sensor**: Detects items on the conveyor.
- **Encoder**: Provides feedback on motor speed and movement.
- **Limit Switch**: Used for homing the conveyor system.

## Software Components
- **ESP32 Firmware (C++)**: Handles the motor control, sensor data acquisition, and custom TCP/IP protocol implementation.
- **Jetson Python Script**: Acts as a TCP client for the ESP32 and a relay to MATLAB. Also simulates network congestion.
- **MATLAB GUI**: Provides an interface for system control, including sending commands and viewing live telemetry.
- **WebSocket Interface**: (Optional) Streams telemetry data for remote monitoring.

## Operational Flow
1. The operator opens the **MATLAB GUI** and connects to the **Jetson**.
2. The **Jetson** connects to the **ESP32**, initiating the conveyor system.
3. The **ESP32** starts sending periodic **STATUS** messages with telemetry data.
4. The **MATLAB GUI** can send commands (START, STOP, PWM, TARGET) to control the conveyor.
5. If a fault occurs, the **ESP32** proactively sends a **FAULT** message to the GUI.
6. The **WebSocket Interface** streams real-time telemetry data for remote monitoring.

## Operational Flow
Output Video 



https://github.com/user-attachments/assets/cb4f90c3-a3cc-43e6-8c79-b43a550d5958




## Future Enhancements
- **Adaptive Timeout Handling**: Implement more dynamic handling of timeouts based on network conditions.
- **Security Features**: Add encryption and authentication for secure communication.
- **Persistent Logging**: Introduce logging for fault detection, performance tracking, and historical analysis.
- **Machine Learning Integration**: Implement predictive maintenance by analyzing system behavior and detecting potential failures before they happen.

## Installation & Setup
### Hardware Setup:
1. Connect the **ESP32** to the DC motor, encoder, IR sensor, and limit switch.
2. Ensure the **Jetson** is connected to the **ESP32** and your PC running **MATLAB**.

### Software Setup:
1. Install the necessary libraries for **ESP32**, **MATLAB**, and **Jetson**.
2. Set up the TCP communication between **ESP32**, **Jetson**, and **MATLAB**.
3. Run the **MATLAB GUI** to start controlling and monitoring the conveyor system.

