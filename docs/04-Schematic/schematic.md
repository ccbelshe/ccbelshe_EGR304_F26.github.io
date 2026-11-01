
---
tags:
  - Fill-A-Bot
  - Pump Control
  - EGR304
---

# Schematic

## Overview

My part of the Fill-A-Bot project focuses on controlling the water pump. The circuit uses a PIC18F57Q43 Curiosity Nano to receive commands from the main controller and a DRV8871 H-bridge to control the pump motor.

I designed the subsystem around three main requirements: reliable motor control, independent power distribution, and straightforward communication with the other boards. The schematic brings these functions together while keeping the higher-current motor circuit separate from the microcontroller's control signals.

## Schematic

![Fill-A-Bot Pump Control Schematic](CB_Pump_Control(1).png)

[View Full-Resolution Schematic PDF](CB_Pump_Control.pdf)

[Download KiCad Project ZIP](CB_Pump_Control.zip)

## Circuit Design

### Power Input and Protection

The board has its own DC barrel jack so it can receive power independently of the other subsystems. A 1A slow-blow fuse is included to protect the input against excessive current.

The design uses a nominal 12V supply for the pump motor. This allows the motor to receive power through the H-bridge without drawing its operating current through the microcontroller.

### Voltage Regulation

I selected the L7805 regulator to provide a 5V rail for the control electronics. The input and output capacitors help maintain stable operation when the supply experiences changes in load.

Since the regulator steps 12V down to 5V, heat dissipation is something I'll need to check during testing. I'll also verify the correct external-power configuration for the Curiosity Nano.

### Microcontroller

The PIC18F57Q43 Curiosity Nano handles the control logic for the subsystem. It reads the incoming digital commands and determines whether the pump should stop, run forward, or run in reverse.

Using the PIC for command processing also gives me flexibility to adjust the motor-control behavior in firmware without changing the communication method between boards.

### Motor Driver

I selected the DRV8871 H-bridge because the pump-control subsystem needs to support both forward and reverse motor operation. A single MOSFET can switch the motor on and off, but it cannot reverse the motor's polarity on its own.

The DRV8871 handles the motor current while the PIC provides the logic signals. This keeps the high-current switching separate from the microcontroller's GPIO pins.

The driver uses a 68kΩ current-limit resistor. Based on the DRV8871 current-limit equation:

I_TRIP = 64 / 68 ≈ 0.94A

This gives a nominal current-trip threshold of approximately 0.94A. I'll confirm that this setting is appropriate once the pump's actual operating and startup currents have been measured.

[DRV8871 Datasheet](https://www.ti.com/lit/ds/symlink/drv8871.pdf)

## Motor Control Logic

The subsystem receives two digital command signals from the main controller. I chose a simple two-bit command structure because it provides the required operating states without needing a more complicated communication protocol.

| Command | Operation |
|---|---|
| 00 | Stop |
| 01 | Forward |
| 10 | Reserved / Stop |
| 11 | Reverse |

The PIC interprets these commands and sets the DRV8871 inputs accordingly. These are the subsystem commands, not the driver's direct input truth table.

## Team Communication

The board communicates with the main controller through an eight-pin ribbon connector. Following the class interface requirements, the connector reserves five pins for digital signals, two for analog signals, and one for ground.

| Ribbon Pin | Function |
|---|---|
| 1–5 | Digital signals |
| 6–7 | Analog signals |
| 8 | Shared ground |

Communication is handled through direct pin-to-pin connections rather than UART or I2C. This keeps the interface simple and allows the boards to exchange logic signals without additional communication hardware.

Each subsystem also has its own power input, so the ribbon connector is used for communication and a common ground reference rather than distributing power between boards.

## Debugging and Testing

I included a debug LED, pushbutton, and spare GPIO connections to make troubleshooting easier as the hardware is assembled.

Once the components arrive, I'll start by checking the power rails and confirming that the PIC responds correctly to the incoming commands. From there, I'll test the H-bridge with the pump and measure its current during normal operation and startup.

I'll also check regulator heating, motor stopping behavior, and whether reversing the motor produces the intended fluid movement. These tests will help determine whether any changes are needed before the final PCB design.

## Design Files

- [Schematic Image](CB_Pump_Control(1).png)
- [Schematic PDF](CB_Pump_Control.pdf)
- [KiCad Project ZIP](CB_Pump_Control.zip)
