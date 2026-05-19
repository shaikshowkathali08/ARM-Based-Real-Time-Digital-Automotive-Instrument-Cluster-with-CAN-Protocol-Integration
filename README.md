# ARM-Based-Real-Time-Digital-Automotive-Instrument-Cluster-with-CAN-Protocol-Integration
Developed an ARM-based real-time digital automotive instrument cluster using Embedded C and CAN protocol to monitor speed, RPM, fuel level, and temperature. Integrated sensor interfacing, CAN communication, and LCD/TFT display for reliable real-time vehicle data monitoring.

## Overview
This project implements a real-time automotive instrument cluster using the LPC21xx ARM7 microcontroller and CAN (Controller Area Network) protocol. The system is divided into two communicating nodes: Node A (Transmitter ECU) and Node B (Receiver ECU/Dashboard Unit).

Node A acquires sensor and switch data, processes it, and transmits vehicle parameters over the CAN bus. Node B receives the CAN messages and displays vehicle information on an LCD while controlling indicators and headlight status.

---

# Project Architecture

```text
+-------------------+         CAN BUS         +----------------------+
|     NODE A        |  --------------------> |       NODE B         |
| (Transmitter ECU) |                        | (Dashboard Receiver) |
+-------------------+                        +----------------------+

Inputs:
- Speed Sensor
- Temperature Sensor
- Indicator Switches
- Headlight Switch

Outputs:
- CAN Messages

                                       Outputs:
                                       - LCD Display
                                       - Left Indicator
                                       - Right Indicator
                                       - Headlight Status
```

---

# Node A – Transmitter ECU

Node A acts as the data acquisition and transmission unit of the automotive system.

## Responsibilities
- Reads analog sensor values using ADC
- Handles external interrupts for switches
- Sends CAN messages periodically
- Transmits:
  - Vehicle speed
  - Engine temperature
  - Left indicator status
  - Right indicator status
  - Headlight status

## Modules Used
- ADC Driver
- CAN Driver
- Timer Interrupt
- External Interrupts (EINT0, EINT1, EINT2)

## CAN IDs Used

| CAN ID | Function |
|------|-----------|
| `0x212` | Left Indicator |
| `0x213` | Headlight |
| `0x214` | Right Indicator |
| `0x215` | Vehicle Speed |
| `0x216` | Engine Temperature |

---

# Node B – Receiver ECU / Dashboard Unit

Node B acts as the dashboard display and monitoring system.

## Responsibilities
- Receives CAN messages using CAN interrupt
- Displays speed on LCD
- Controls left/right indicators
- Displays headlight status
- Processes real-time vehicle information

## Modules Used
- CAN Receive Interrupt
- LCD Driver
- GPIO Control
- Delay Functions

---

# Project Structure

## Node A Files

| File | Description |
|------|-------------|
| `main.c` | Main transmitter application |
| `adc_driver.c` | ADC sensor reading |
| `can_drive.c` | CAN transmission driver |
| `timer_interrupt.c` | Timer1 interrupt handling |
| `eint0.c` | External Interrupt 0 |
| `eint1.c` | External Interrupt 1 |
| `eint2.c` | External Interrupt 2 |
| `delay.c` | Delay functions |
| `header.h` | Common declarations |

---

## Node B Files

| File | Description |
|------|-------------|
| `main.c` | Main receiver application |
| `can_tx.c` | CAN receive interrupt handler |
| `can2_rx.drive.c` | CAN filtering and receive driver |
| `lcd_drive.c` | LCD interfacing functions |
| `delay.c` | Delay functions |
| `header.h` | Common declarations |

---

# Working Principle

## Vehicle Speed Monitoring
1. Node A reads analog speed value using ADC.
2. Converts and sends value using CAN ID `0x215`.
3. Node B receives the message.
4. Speed is displayed on LCD in Kmph.

---

## Headlight Control
1. External interrupt triggers headlight switch.
2. Node A sends CAN message `0x213`.
3. Node B updates headlight indicator and GPIO output.

---

## Indicator Control
- Left indicator → CAN ID `0x212`
- Right indicator → CAN ID `0x214`

Node B receives the signal and performs blinking animation on LCD and LEDs.

---

# Concepts Implemented

- ARM7 LPC2129 Programming
- Embedded C
- CAN Protocol
- Interrupt Handling
- Real-Time Embedded Systems
- ADC Interfacing
- LCD Interfacing
- Automotive ECU Communication

---

# Hardware Requirements

- LPC2129 ARM7 Development Board
- CAN Transceiver Module
- LCD 16x2 Display
- Push Buttons/Switches
- Potentiometer/Sensors
- LEDs
- Power Supply

---

# Software Requirements

- Keil uVision
- Embedded C
- Flash Magic
- Proteus (Optional)

---

# CAN Communication Example

## Node A Transmission

```c
v2.id = 0x215;
speed = adc_read(2);
v2.byteA = speed;
can2_tx(v2);
```

## Node B Reception

```c
if(m1.id == 0x215)
{
    speed = m1.byteA;
    lcd_string("SPEED:");
}
```

---

# Applications

- Automotive Dashboard Systems
- Vehicle Monitoring Systems
- Smart Instrument Clusters
- ECU Communication Networks
- Embedded Automotive Electronics

---

# Future Enhancements

- TFT graphical dashboard
- GPS integration
- OBD-II diagnostics
- Wireless CAN monitoring
- Data logging using SD card
- Fuel efficiency monitoring
- Bluetooth connectivity

---

# Learning Outcomes

This project provides hands-on experience in:
- Automotive embedded systems
- Real-time CAN communication
- ARM microcontroller programming
- Interrupt-driven programming
- Sensor interfacing
- Embedded hardware-software integration

---

# Author

BABAFEIN  
Embedded Systems and Automotive Electronics

---

# License

This project is developed for educational and learning purposes only.

---

# References

- LPC2129 User Manual
- CAN Protocol Documentation
- ARM7 Embedded System Concepts
