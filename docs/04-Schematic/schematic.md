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

## Pending Verification


