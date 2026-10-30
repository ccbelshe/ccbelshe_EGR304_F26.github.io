
---
title: Bill of Materials
tags:
  - Fill-A-Bot
  - Pump Control
  - EGR304
---

# Bill of Materials

## Overview

This is the bill of materials for my Fill-A-Bot pump-control subsystem. It includes the components from my KiCad schematic, the parts needed for the pump assembly, and the hardware being ordered for testing.

Most of the basic components are already available in the lab. The larger components and connectors are included in the purchase requests.

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

These parts are needed for the pump assembly, connections, and breadboard testing. They are listed separately because they are not all individual components in the KiCad schematic.

| Component | Quantity | Source | Notes |
|---|---:|---|---|
| Gikfun EK1912 R385 12V water pump kit | 1 | Amazon | $18.98 |
| 12V DC power adapter | 1 | Included with pump kit | No separate purchase |
| Water tubing and fittings | 1 set | Included with pump kit | No separate purchase |
| Adafruit DRV8871 breakout board | 1 | DigiKey | Breadboard testing |
| 8-pin IDC ribbon sockets | 4 | DigiKey | For ribbon connections |
| 8-conductor ribbon cable | 5 feet | DigiKey | Team communication |
| 5x20 mm fuse holders | 2 | DigiKey | For the 1A fuse |

## Component Selection

### Microcontroller

I'm using the PIC18F57Q43 Curiosity Nano for the control logic. It receives signals from the other boards and sends the motor-control signals to the H-bridge.

I already have the PIC, so it does not need to be ordered.

### Motor Driver

The DRV8871 H-bridge controls the pump motor. It allows the motor to run in either direction, which is needed for the forward and reverse operation.

Three DRV8871DDAR chips are included in the DigiKey request for the PCB and spares.

[DRV8871 Motor Driver - DigiKey](https://www.digikey.com/en/products/detail/texas-instruments/DRV8871DDAR/5639726)

The current-limit resistor is R1, which is 68k ohms.

The nominal current-trip calculation is:

`I_TRIP = 64 / 68 = 0.94 A`

This value will be checked once the pump is available for testing.

### Breadboard Testing

The DRV8871DDAR is a surface-mount component, so it cannot plug directly into a breadboard.

One Adafruit DRV8871 breakout board is included in the DigiKey order to make testing easier before assembling the PCB.

The breakout board will be used to test the pump and motor-control signals. Header pins will need to be soldered before using it on a breadboard.

### Fuse Protection

The board uses a 1A slow-blow fuse on the power input.

Three Littelfuse 0218001.MXP fuses and two fuse holders are included in the DigiKey request.

[1A Slow-Blow Fuse - DigiKey](https://www.digikey.com/en/products/detail/littelfuse-inc/0218001-MXP/777598)

The fuse rating will be checked against the pump's actual current during testing.

### Power Supply

The pump-control subsystem is designed around a 12V DC supply.

The L7805 voltage regulator provides 5V for the control circuitry. The pump kit includes a 12V adapter, so a separate power adapter is not being ordered.

### Pump

The selected pump is the Gikfun EK1912 R385 12V diaphragm water pump kit.

The kit includes the pump, power adapter, and tubing needed for initial testing.

- Vendor: Amazon
- Quantity: 1
- Price: $18.98

[Gikfun EK1912 Pump Kit - Amazon](https://www.amazon.com/dp/B07DW4WRV8)

### Team Communication

The subsystem uses an eight-pin ribbon connector for communication with the other Fill-A-Bot boards.

The planned connections are:

- Pins 1-5: Digital signals
- Pins 6-7: Analog signals
- Pin 8: Common ground

Each board has its own power input. The boards communicate through direct pin-to-pin signals rather than I2C or UART.

The planned motor-control commands are:

- 00 = Stop
- 01 = Forward
- 11 = Reverse

## Purchasing Plan

The parts that aren't available in the lab are included in two purchase requests.

**DigiKey:** DRV8871 motor drivers, one Adafruit DRV8871 breakout board for breadboard testing, fuses, fuse holders, power connector, pump connector, and ribbon cable components.

**Amazon:** Gikfun EK1912 R385 12V pump kit, including the power adapter and tubing.

| Vendor | Estimated Cost |
|---|---:|
| DigiKey | $42.18 |
| Amazon | $18.98 |
| **Total** | **$61.16** |

The purchase requests will be sent to the instructor for approval before ordering. Prices are estimates before tax and shipping.

## Testing and Verification

Once the parts arrive, I'll test the pump with the motor driver and check the current draw. I'll also verify the 5V regulator output and make sure the ribbon connections work with the other boards.

Any changes needed after testing will be updated in the schematic and BOM.

## Project Information

**Project:** Fill-A-Bot

**Course:** EGR304 - Fall 2026

**Subsystem:** Pump Control

**Student:** Clay Belsher

The component list was exported from my KiCad schematic and updated to include the parts needed for the pump assembly.
