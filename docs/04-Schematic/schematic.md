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

J1 provides the local power input through fuse F1. U1 is the L7805 regulator for the planned 5 V control supply. C1 and C2 support the regulator, while C4 provides local supply decoupling.

The schematic currently assumes a 12 V input. The intended supply needs confirmation against the course's specified 9 V adapter.

### Command Inputs

The pump board receives two digital command bits from the main controller:

| Signal | Connector | PIC port |
|---|---|---|
| PUMP_CMD0 | J3 pin 1 | RC3 |
| PUMP_CMD1 | J3 pin 2 | RC2 |
| Shared ground | J3 pin 8 | GND |

RC2 and RC3 will be configured as digital inputs. Their UART descriptions identify alternate pin functions. The four command meanings still need team agreement.

Ribbon pins 1–5 are digital I/O, pins 6–7 are analog, and pin 8 is ground. No power is distributed through the ribbon.

### Pump Switching

The PIC's RA2 output connects through JP1 and gate resistor R1 to MOSFET Q1. Q1 switches the pump's return path to ground. R2 pulls the gate toward ground when it is not driven.

D1 provides a recirculation path for motor current when Q1 switches off. J2 connects the pump. Opening JP1 disconnects the controller's gate-drive path.

### Testing and Expansion

D2 and R3 provide a controller-driven LED. SW1, R4, and C3 form a local button-input circuit. Their firmware functions remain to be defined. J4 provides spare GPIO and local power connections.

## Remaining Design Work

The current motor circuit supports one-direction switching. Forward/reverse operation requires a bidirectional driver revision and verification of pump compatibility. Supply voltage and command definitions remain to be finalized.


