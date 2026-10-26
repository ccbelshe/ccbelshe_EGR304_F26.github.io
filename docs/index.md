---
title: Pump Control — Clay Belsher
---

# Fill-A-Bot Pump Control

**Clay Belsher — EGR 304**

## Project Summary

Fill-A-Bot is an automated water-dispensing system designed to fill a container to a selected amount. The team’s subsystems handle user commands, measurement, and pump operation. The main controller coordinates these functions and determines when dispensing should stop. My contribution is the pump-control board, which receives two digital command signals and controls the motor circuit.

## My Subsystem

My board uses a PIC18F57Q43 Curiosity Nano, a local power input, a 5V linear regulator, and a motor-switching circuit. The main controller sends CMD0 to RC3 and CMD1 to RC2 through the ribbon connector. Ribbon pin 8 provides a common ground.

The current schematic uses a single MOSFET for one-direction motor control. Forward-and-reverse operation requires an H-bridge revision. The original 12V supply assumption also needs to be reconciled with the project’s specified 9V adapter.

## Design Documentation

- [Block Diagram](01-Block-Diagram/Block-Diagram.md) — subsystem connections and design decisions.
- [Component Selection and MCC Configuration](02-Component-Selection/Component-Selection.md) — component comparisons, selection reasons, and generated GPIO configuration.
- [Bill of Materials](03-BOM/BOM.md) — component-list page.
- [Schematic](04-Schematic/schematic.md) — circuit explanation, schematic image, PDF, and KiCad project.
- [Power Budget](05-Power-Budget/Power-Budget.md) — preliminary calculations, assumptions, and Excel downloads.

## Current Status

The updated GPIO configuration has been generated in MCC. Hardware operation has not been verified, and the power budget uses labeled planning assumptions. The final pump, H-bridge, supply choice, and command assignments remain open design decisions.
