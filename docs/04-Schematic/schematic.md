---
title: Schematics
---

## Overview

My Fill-A-Bot pump subsystem uses a PIC18F57Q43 Curiosity Nano to control an R385 pump through a MOSFET switching circuit. It receives a two-bit digital command from the main controller through PUMP_CMD0 on RC3 and PUMP_CMD1 on RC2. The board includes a local power input, 5 V regulator, fuse, flyback diode, and debugging components. The current motor circuit supports one-direction operation; forward/reverse control remains a planned revision.

## Schematic

<img src="https://raw.githubusercontent.com/ccbelshe/ccbelshe_EGR304_F26.github.io/main/docs/04-Schematic/CB_Pump_Control.png" alt="Pump schematic" class="skip-glightbox" style="width:100%;">

## Downloads

- [Schematic PDF](CB_Pump_Control.pdf)
- [Complete KiCad project ZIP](CB_Pump_Control.zip)

## Circuit Operation

### Power Supply

Power enters through barrel jack J1 and fuse F1. U1 is the L7805 regulator that provides the planned 5 V supply for the controller. C1 and C2 help keep the regulator input and output stable. C4 provides additional decoupling near the controller.

My current schematic uses a 12 V input. I still need to resolve that choice with the course requirement for a 9 V adapter.

### Command Inputs

The main board sends commands to my pump board using two digital signals. Each signal can be HIGH or LOW, giving four possible combinations.

| Signal | Connector | PIC port |
|---|---|---|
| PUMP_CMD0 | J3 pin 1 | RC3 |
| PUMP_CMD1 | J3 pin 2 | RC2 |
| Shared ground | J3 pin 8 | GND |

RC2 and RC3 will be set as digital inputs in firmware. The UART text on the PIC symbol lists another function those pins can perform. Our team still needs to agree on what each command combination means.

The ribbon uses pins 1–5 for digital I/O, pins 6–7 for analog, and pin 8 for ground. Each board has its own power connection.

### Pump Control

The PIC controls Q1 through its RA2 output, jumper JP1, and resistor R1. Q1 acts as a switch in the pump's return path to ground. This lets the PIC control the pump without carrying the motor current through a GPIO pin.

R1 limits current into the MOSFET gate during switching. R2 pulls the gate toward ground when it is not driven, helping keep Q1 off. D1 gives the motor current a path when Q1 switches off. The pump connects at J2.

Opening JP1 disconnects the PIC from the gate-drive path so that connection can be isolated during testing.

### Testing and Spare Connections

D2 is a debug LED with current-limiting resistor R3. SW1 provides a local button input, with R4 pulling the input high and C3 providing filtering. I still need to define the LED and button functions in the program.

J4 brings out spare GPIO pins, 5 V, and ground for local testing or expansion.

## Changes Still Needed

The current MOSFET circuit controls the pump in one direction. I still need to revise the driver for forward and reverse operation and check whether the selected pump can move water in reverse. The supply voltage and command meanings also need to be finalized.
