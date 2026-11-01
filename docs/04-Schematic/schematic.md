
---
Pump Control Schematic
tags:
  - Fill-A-Bot
  - Pump Control
  - EGR304
---

# Pump Control Schematic

## Overview

My Fill-A-Bot subsystem is responsible for controlling the water pump. It uses a PIC18F57Q43 Curiosity Nano microcontroller to receive commands from the main controller and operate the pump through a DRV8871 H-bridge motor driver.

The design supports forward, reverse, and stop commands. The H-bridge replaces the original single-MOSFET motor-control design to meet the bidirectional motor requirement.

The subsystem includes a dedicated 12V power input, fuse protection, a 5V voltage regulator, an eight-pin ribbon connector, and debugging components.

## Updated Pump Control Schematic

The updated schematic is displayed below.

![Updated Fill-A-Bot Pump Control Schematic](CB_Pump_Control(1).png)

**[View Full-Resolution Schematic PDF](CB_Pump_Control.pdf)**

**[Download Complete KiCad Project ZIP](CB_Pump_Control.zip)**

## Circuit Design

The schematic is organized into five main functional sections:

1. Power input and protection
2. Voltage regulation
3. PIC18F57Q43 microcontroller
4. DRV8871 H-bridge and pump control
5. Team communication and debugging

### Power Input and Protection

The pump-control subsystem uses its own DC barrel jack for power.

A 1A slow-blow fuse is included for input protection. The selected R385 pump kit uses a nominal 12V supply.

The pump receives power from the motor supply, while the microcontroller and control electronics use the regulated control rail.

### Voltage Regulation

An L7805 voltage regulator provides the planned 5V supply for the control electronics.

Input and output capacitors support stable operation.

Regulator heating and the Curiosity Nano external-power configuration must be verified before final hardware testing.

### Microcontroller

The PIC18F57Q43 Curiosity Nano serves as the subsystem controller.

It receives digital commands from the main controller, interprets the requested operating state, and generates the appropriate motor-driver control signals.

The microcontroller also supports local debugging through an LED, pushbutton, and spare GPIO connections.

### Motor Driver

The updated design selects the Texas Instruments DRV8871 integrated H-bridge.

Unlike the original single-MOSFET circuit, the H-bridge supports applying either polarity across the motor, allowing forward and reverse motor commands.

The PIC controls the DRV8871 through digital signals.

A 68kΩ current-limit resistor is included in the design.

Using the DRV8871 current-limit relationship:

I_TRIP = 64 / 68 = 0.94A

The nominal current-trip threshold is approximately 0.94A.

This setting is preliminary until the pump's operating, startup, and stall currents are verified.

The driver also uses supply capacitors to support stable operation.

[DRV8871 Datasheet](https://www.ti.com/lit/ds/symlink/drv8871.pdf)

## Motor Control Logic

The main controller communicates with the pump-control subsystem using two digital command signals.

| Command | Operation |
|---|---|
| 00 | Stop |
| 01 | Forward |
| 10 | Reserved / Stop |
| 11 | Reverse |

The PIC interprets these commands and generates the corresponding DRV8871 control signals.

The command table represents the intended subsystem behavior, not the DRV8871's direct input truth table.

The pump should remain stopped during startup, invalid commands, and conditions requiring shutdown.

## Team Communication

The subsystem connects to the main controller using an eight-pin ribbon connector.

| Ribbon Pin | Function |
|---|---|
| 1–5 | Digital communication |
| 6–7 | Analog connections |
| 8 | Shared ground |

The system uses direct pin-to-pin communication rather than UART or I2C.

Each board has its own power connection. The ribbon connector carries communication signals and a common ground reference, not shared power.

The final digital signal assignments must remain consistent with the team's interface documentation.

## Debugging and Testing

The design includes a debug LED, pushbutton, and spare GPIO connections to support testing.

The following items require verification:

- Correct 12V and 5V supply operation
- Digital communication between the boards
- Forward, reverse, and stop commands
- Pump operating and startup current
- DRV8871 current-limit behavior
- Regulator and motor-driver temperature
- Safe motor shutdown
- Pump reverse-flow behavior

## Design Revisions

The original design used a single MOSFET to control the pump in one direction.

Following design review, the motor-control stage was revised to use a DRV8871 H-bridge.

This change supports the required bidirectional motor operation and provides an integrated motor-driver solution.

The updated component selection and bill of materials include the DRV8871, current-limit resistor, and required supply capacitors.

## Design Files

- [Updated Schematic Image](CB_Pump_Control(1).png)
- [Full-Resolution Schematic PDF](CB_Pump_Control.pdf)
- [KiCad Project ZIP](CB_Pump_Control.zip)

The schematic PDF and KiCad ZIP should represent the same revision as the displayed schematic image. The ZIP must be replaced if it still contains the earlier MOSFET-only design.

## Current Verification Status

The DRV8871 H-bridge and R385 pump have been selected for the updated design.

Final hardware verification will confirm motor current, bidirectional operation, supply compatibility, and component footprints before PCB fabrication.
