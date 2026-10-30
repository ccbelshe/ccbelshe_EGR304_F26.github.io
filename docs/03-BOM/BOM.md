
---
title: Bill of Materials
tags:
  - Fill-A-Bot
  - Pump Control
  - EGR304
---

# Bill of Materials

## Overview

This page documents the Bill of Materials (BOM) for my Fill-A-Bot pump-control subsystem. My board uses a PIC18F57Q43 Curiosity Nano microcontroller and a DRV8871 H-bridge motor driver to control a 12V DC water pump.

The circuit includes local power regulation, fuse protection, digital communication through an eight-pin ribbon connector, and debugging components.

The following table is based on my KiCad schematic export, `CB_Pump_Control.csv`. All schematic components are included, even if they are already owned or supplied by the electronics laboratory.

## Complete Bill of Materials

| Reference | Component / Value | Qty | Purchase Qty | Source / Status |
|---|---|---:|---:|---|
| C1 | 0.33 uF, 25V capacitor | 1 | 0 | Lab supplied |
| C2 | 0.1 uF, 16V capacitor | 1 | 0 | Lab supplied |
| C3 | 1 uF, 16V capacitor | 1 | 0 | Lab supplied |
| C4 | 0.1 uF, 16V capacitor | 1 | 0 | Lab supplied |
| C5 | 0.1 uF, 50V capacitor | 1 | 0 | Lab supplied |
| C6 | 47 uF, 25V capacitor | 1 | 0 | Lab supplied |
| D2 | Debug LED | 1 | 0 | Lab supplied |
| F1 | 1A slow-blow fuse | 1 | TBD | Verify lab stock |
| J1 | DC barrel jack | 1 | 1 | Purchase |
| J2 | Pump connector, 2-pin | 1 | 1 | Purchase |
| J3 | Team interface, 2x4 header | 1 | 1 | Purchase |
| J4 | Spare GPIO, 1x4 header | 1 | 1 | Purchase if needed |
| JP1 | Pump enable, 2-pin jumper | 1 | 1 | Purchase if needed |
| R1 | 68k ohm resistor | 1 | 0 | Lab supplied |
| R2 | 10k ohm resistor | 1 | 0 | Lab supplied |
| R3 | 1k ohm, 0.25W resistor | 1 | 0 | Lab supplied |
| R4 | 10k ohm, 0.25W resistor | 1 | 0 | Lab supplied |
| SW1 | Pushbutton switch | 1 | 0 | Lab supplied |
| TP1, TP2, TP3 | PCB test points | 3 | 0 | PCB pads |
| U1 | L7805 5V voltage regulator | 1 | TBD | Verify lab stock |
| U2 | PIC18F57Q43 Curiosity Nano | 1 | 0 | Already owned |
| U3 | DRV8871DDA H-bridge driver | 1 | 3 | Purchase, including prototype and spare |

**Note:** The exported KiCad BOM contains 23 rows representing 24 individual component instances. Purchase quantities are preliminary and will be finalized in the purchase request.

## External Pump Assembly Components

These components are required for the complete pump assembly but are not all represented as separate schematic symbols.

| Component | Qty | Purpose |
|---|---:|---|
| R385 12V DC water pump | 1 | Dispenses water |
| Regulated 12V DC power adapter | 1 | Powers the pump-control board |
| Eight-conductor ribbon cable assembly | 1 | Connects team subsystems |
| Mating connectors and jumper caps | As needed | Electrical connections |
| Tubing and fittings | As needed | Water connections |

## Major Component Selection

### Microcontroller

The PIC18F57Q43 Curiosity Nano receives digital commands from the main controller and controls the motor driver. This development board is already owned and does not need to be purchased.

### Motor Driver

The DRV8871 H-bridge motor driver provides forward and reverse motor control. This supports the project's motor-direction requirements.

The schematic uses a 68k ohm current-limit resistor. The nominal current-trip calculation is:

`I_TRIP = 64 / 68 = 0.94 A`

This value is provisional until the pump's startup and stall current are verified.

[DRV8871 Datasheet](https://www.ti.com/lit/ds/symlink/drv8871.pdf)

### Power Supply

The board uses a local 12V power input through a barrel jack. The motor receives power from the 12V supply, while an L7805 regulator provides a 5V control rail.

A 1A slow-blow fuse is included for input protection. The final power adapter and fuse rating must be checked against the selected pump's current requirements.

### Team Communication

The pump-control board communicates through an eight-pin ribbon connector.

The class interface assigns pins 1 through 5 to digital signals, pins 6 and 7 to analog signals, and pin 8 to common ground.

The motor commands are:

- `00` = Stop
- `01` = Forward
- `11` = Reverse

Communication uses direct logic-level signals rather than I2C or UART.

## Purchasing Plan

The hardware order focuses on missing major components rather than common components already available in the lab.

The main purchasing priorities are the DRV8871 motor driver, R385 water pump, compatible 12V power supply, barrel jack, and required connectors.

Parts will be organized by vendor using the EGR304 purchase request template. Final prices, product links, and availability will be recorded on the purchase request forms.

## Verification and Future Testing

The KiCad schematic and BOM have been prepared for the hardware ordering assignment.

Before PCB fabrication, footprints must be matched to the physical components being purchased. The pump's startup current, reversal behavior, and electrical connections must also be tested.

## Documentation

**Project:** Fill-A-Bot

**Subsystem:** Pump Control

**Course:** EGR 304, Fall 2026

**KiCad schematic:** `CB_Pump_Control.kicad_sch`

**KiCad BOM export:** `CB_Pump_Control.csv`
