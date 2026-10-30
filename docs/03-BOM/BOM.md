
---
title: Bill of Materials
tags:
  - Fill-A-Bot
  - Pump Control
  - EGR304
---

# Bill of Materials

## Overview

This page documents the Bill of Materials (BOM) for my Fill-A-Bot pump-control subsystem in EGR304.

My subsystem uses a PIC18F57Q43 Curiosity Nano microcontroller and a DRV8871 H-bridge motor driver to control a 12V DC water pump. The circuit includes a 5V voltage regulator, fuse protection, an eight-pin ribbon interface, and debugging components.

The BOM is based on my KiCad schematic and exported component list. Components already owned or available in the electronics lab are included but do not need to be purchased again.

## Complete Bill of Materials

| Reference | Component / Value | Design Qty | Purchase Qty | Source / Status |
|---|---|---:|---:|---|
| C1 | 0.33 uF, 25V capacitor | 1 | 0 | Lab supplied |
| C2 | 0.1 uF, 16V capacitor | 1 | 0 | Lab supplied |
| C3 | 1 uF, 16V capacitor | 1 | 0 | Lab supplied |
| C4 | 0.1 uF, 16V capacitor | 1 | 0 | Lab supplied |
| C5 | 0.1 uF, 50V capacitor | 1 | 0 | Lab supplied |
| C6 | 47 uF, 25V capacitor | 1 | 0 | Lab supplied |
| D2 | Debug LED | 1 | 0 | Lab supplied |
| F1 | 1A slow-blow fuse | 1 | 3 | DigiKey |
| J1 | DC barrel jack | 1 | 1 | DigiKey |
| J2 | 2-pin pump connector | 1 | 1 | DigiKey |
| J3 | 2x4 ribbon header | 1 | 3 | DigiKey |
| J4 | 1x4 spare GPIO header | 1 | 0 | Lab stock |
| JP1 | 2-pin pump enable jumper | 1 | 0 | Lab stock |
| R1 | 68k ohm resistor | 1 | 0 | Lab supplied |
| R2 | 10k ohm resistor | 1 | 0 | Lab supplied |
| R3 | 1k ohm, 0.25W resistor | 1 | 0 | Lab supplied |
| R4 | 10k ohm, 0.25W resistor | 1 | 0 | Lab supplied |
| SW1 | Pushbutton switch | 1 | 0 | Lab supplied |
| TP1 | Test point | 1 | 0 | Lab supplied |
| TP2, TP3 | Test points | 2 | 0 | Lab supplied |
| U1 | L7805 5V voltage regulator | 1 | 0 | Lab stock |
| U2 | PIC18F57Q43 Curiosity Nano | 1 | 0 | Already owned |
| U3 | DRV8871 H-bridge motor driver | 1 | 3 | DigiKey |

The KiCad BOM contains 23 grouped entries representing 24 individual component instances.

## Additional Pump Assembly Components

These components are needed to assemble and operate the subsystem but are not all represented as separate schematic components.

| Component | Quantity | Source | Notes |
|---|---:|---|---|
| Gikfun EK1912 R385 12V water pump kit | 1 | Amazon | $18.98 |
| 12V DC power adapter | 1 | Included with pump kit | No separate purchase |
| Water tubing and fittings | 1 set | Included with pump kit | No separate purchase |
| 8-pin IDC ribbon sockets | 4 | DigiKey | For ribbon connections |
| 8-conductor ribbon cable | 5 feet | DigiKey | Team communication |
| 5x20 mm fuse holders | 2 | DigiKey | For 1A fuse |

### Selected Pump

The selected pump is the Gikfun EK1912 R385 diaphragm water pump kit.

- Vendor: Amazon
- Quantity: 1
- Price: $18.98
- Supply voltage: 12V
- Includes a power adapter and tubing

[Amazon Gikfun EK1912 Pump Kit](https://www.amazon.com/dp/B07DW4WRV8)

The pump is intended for prototyping and testing the liquid dispensing system. Its startup current will be checked during testing.

## Major Component Selection

### Microcontroller

The PIC18F57Q43 Curiosity Nano receives control signals from the other subsystems and sends the required signals to the motor driver.

I already have this microcontroller, so it does not need to be ordered.

### H-Bridge Motor Driver

The DRV8871 H-bridge controls the direction and operation of the DC pump motor.

The selected DigiKey ordering part is DRV8871DDAR.

- Manufacturer: Texas Instruments
- Quantity ordered: 3
- Unit price: $2.98
- Total: $8.94

[DigiKey DRV8871 Motor Driver](https://www.digikey.com/en/products/detail/texas-instruments/DRV8871DDAR/5639726)

The schematic uses a 68k ohm current-limit resistor.

The nominal current-trip calculation is:

`I_TRIP = 64 / 68 = 0.94 A`

This is a preliminary value until the pump's operating and startup current are tested.

[DRV8871 Datasheet](https://www.ti.com/lit/ds/symlink/drv8871.pdf)

### Fuse Protection

The board includes a 1A slow-blow fuse to protect the power input.

The selected fuse is the Littelfuse 0218001.MXP.

- Vendor: DigiKey
- Quantity ordered: 3
- Unit price: $1.56
- Total: $4.68

[DigiKey 1A Slow-Blow Fuse](https://www.digikey.com/en/products/detail/littelfuse-inc/0218001-MXP/777598)

Two compatible fuse holders are included in the hardware order.

The final fuse rating will be checked against the pump's current requirements during testing.

### Power Regulation

The subsystem uses a 12V DC supply to operate the motor driver and pump.

An L7805 voltage regulator provides a regulated 5V rail for the control circuitry.

The selected pump kit includes a 12V adapter, so a separate 9V or 12V adapter is not included in the current purchase request.

### Team Communication

The subsystem uses an eight-pin ribbon connector to communicate with the other Fill-A-Bot boards.

The planned interface uses:

- Pins 1-5: Digital signals
- Pins 6-7: Analog signals
- Pin 8: Common ground

Power is supplied locally rather than shared between subsystem boards.

The planned motor-control commands are:

- 00 = Stop
- 01 = Forward
- 11 = Reverse

The team uses direct pin-to-pin communication rather than I2C or UART.

## Purchasing Plan

Purchase requests have been prepared using the EGR304 purchasing template.

The hardware order is divided between two vendors.

### DigiKey

The DigiKey request includes the motor driver, fuses, fuse holders, barrel jack, pump connector, ribbon headers, IDC sockets, and ribbon cable.

Estimated subtotal: $25.14 before tax and shipping.

### Amazon

The Amazon request includes one Gikfun EK1912 R385 water pump kit.

Estimated subtotal: $18.98 before tax and shipping.

### Combined Estimated Cost

| Vendor | Estimated Subtotal |
|---|---:|
| DigiKey | $25.14 |
| Amazon | $18.98 |
| **Total** | **$44.12** |

Prices are estimates from the prepared purchase requests and may change before the orders are approved.

## Testing and Verification

Once the parts arrive, I'll test the pump with the motor driver and check the current draw. I'll also verify the 5V regulator output and make sure the ribbon connections work with the other boards.

Any changes needed after testing will be updated in the schematic and BOM.

## Project Information

**Student:** Clay Belsher

**Email:** ccbelshe@asu.edu

**Course:** EGR304 - Fall 2026

**Project:** Fill-A-Bot

**Subsystem:** Pump Control

**Schematic:** CB_Pump_Control.kicad_sch

**KiCad BOM Export:** CB_Pump_Control.csv

