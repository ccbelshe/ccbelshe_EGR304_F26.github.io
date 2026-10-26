---
title: Fill-A-Bot Pump Control
---

# Fill-A-Bot Pump Control

**Clay Belsher | EGR 304**

## Project Summary

Fill-A-Bot is an automated water-dispensing system that fills a container to a selected amount. The main controller coordinates the user controls, measurement subsystems, and pump board to start dispensing and stop at the target. My responsibility is the pump-control subsystem, including its local power circuit, motor driver, and digital command connections.

## My Contribution

My board uses a PIC18F57Q43 Curiosity Nano to receive two digital command signals from the main controller. CMD0 connects to RC3, CMD1 connects to RC2, and ribbon pin 8 provides a common ground. RA2 controls the motor-switching circuit.

The board includes a barrel-jack power input, fuse, 5V linear regulator, motor flyback protection, and a debug LED and button. The current single-MOSFET driver requires an H-bridge revision for forward-and-reverse operation. The linked design pages explain the component choices, configuration, calculations, and remaining decisions.

## Design Documentation

| Page | Contents |
|---|---|
| [Block Diagram](01-Block-Diagram/Block-Diagram.md) | Power and signal connections, design decisions, and editable diagram files. |
| [Component Selection](02-Component-Selection/Component-Selection.md) | Component comparisons, selection reasons, pin assignments, and MCC configuration. |
| [Bill of Materials](03-BOM/BOM.md) | Component-list page. |
| [Schematic](04-Schematic/schematic.md) | Circuit operation, schematic image, high-resolution PDF, and KiCad files. |
| [Power Budget](05-Power-Budget/Power-Budget.md) | Preliminary current and heat calculations, assumptions, and the editable spreadsheet. |
