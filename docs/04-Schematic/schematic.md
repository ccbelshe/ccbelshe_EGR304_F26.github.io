---
title: Schematics
---

## Overview

My Fill-A-Bot pump subsystem uses a PIC18F57Q43 Curiosity Nano to control a 12 V R385 pump through a MOSFET driver. It includes a 5 V regulator, fuse, flyback diode, UART interface, and debugging components.

## Schematic

<img src="https://raw.githubusercontent.com/ccbelshe/ccbelshe_EGR304_F26.github.io/main/docs/04-Schematic/CB_Pump_Control.png" alt="Pump schematic" class="skip-glightbox" style="width:100%;">

## Downloads

- [Schematic PDF](CB_Pump_Control.pdf)
- [Complete KiCad project ZIP](CB_Pump_Control.zip)

## Pending Verification

The fuse rating, PWM/gate-drive performance, footprint assignments, and team UART pinout require verification before fabrication.
