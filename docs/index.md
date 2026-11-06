# Fill-A-Bot Pump Control

**Clay Belsher | EGR 304**

## Project Summary

Fill-A-Bot is an automated water-dispensing system designed to fill a container to a selected amount. The main controller coordinates the user controls, measurement subsystems, and pump board to start dispensing and stop at the target.

My responsibility is the pump-control subsystem. My board receives commands from the main controller and controls the pump motor used to move water from the reservoir.

## My Contribution

My pump-control board uses a **PIC18F57Q43 Curiosity Nano** to receive and interpret two digital command signals from the main controller. The PIC then sends control signals to a **DRV8871 H-bridge motor driver**, which controls the direction of the pump motor.

The subsystem uses the following two-bit commands:

| Command | Pump Operation |
| :---: | --- |
| 00 | Stop |
| 01 | Forward |
| 10 | Reserved / Stop |
| 11 | Reverse |

These are the commands sent to the PIC, not the direct input states of the DRV8871.

My board also includes its own barrel-jack power input, a fuse, a voltage regulator, a ribbon connector for communication, and a debug LED and button. The boards communicate using direct pin-to-pin signals and a shared ground.

## Current Design Status

The schematic includes the DRV8871 H-bridge in place of the original single-MOSFET motor driver. The H-bridge was selected because the design needs to control the motor in both forward and reverse directions.

The current schematic and written power budget use a nominal **12 V** supply. The final supply voltage still needs to be confirmed against the project requirements. The pump's operating and startup current, the driver current-limit setting, and the pump's behavior in reverse also need to be verified through testing.

The next step is to complete and verify the PCB layout for manufacturing.

## Design Documentation

| Page | Contents |
| --- | --- |
| [Block Diagram](01-Block-Diagram/Block-Diagram.md) | Subsystem connections, signals, and design decisions |
| [Component Selection](02-Component-Selection/Component-Selection.md) | Component choices, comparisons, and PIC configuration |
| [Bill of Materials](03-BOM/BOM.md) | Parts used in the pump-control subsystem |
| [Schematic](04-Schematic/schematic.md) | Circuit operation, schematic image, PDF, and design files |
| [Power Budget](05-Power-Budget/Power-Budget.md) | Preliminary current and power calculations |

## Team Project

Visit the [Fill-A-Bot team website](https://egr304-2026-f-201.github.io/) for the overall project design and links to my teammates' individual datasheets.
