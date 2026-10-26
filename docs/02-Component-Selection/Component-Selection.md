# Component Selection

**Clay Belsher — Fill-A-Bot Pump Control Subsystem**

## Overview

My subsystem controls the pump that moves water from the reservoir to the container. The main controller sends two digital command signals to my board. My controller reads those signals and controls the motor circuit.

The main controller determines when the selected amount has been dispensed and sends the command to stop the pump. Filling accuracy depends on the complete system, including measurement, communication, pump response, and water remaining in the tubing.

**Current design status:** The schematic and original component comparisons use a 12V supply. The project specifies a 9V adapter, so the supply selection and pump performance at that voltage still need confirmation. The current single-MOSFET circuit supports one-direction operation. An H-bridge is needed to meet the forward-and-reverse requirement.

The tables below document the component comparisons and explain the current choices. Prices are earlier estimates in USD, excluding shipping and taxes. Prices, availability, and specifications need checking before ordering.

## Water Pump

The pump needs to move water at a useful rate, fit the tubing arrangement, and work with the final supply. The required reverse operation also needs checking with the actual pump mechanism. Reversing motor rotation does not automatically mean the pump will reverse water flow.

| Option | Estimated Unit Cost | Pros | Cons |
|---|---|---|---|
| **Gikfun EK1856 / R385**<br>[Product information](https://gikfun.com/products/gikfun-mini-dc-6v-to-12v-r385-water-cooled-water-pump-air-diaphragm-pump) | $10.88 | Originally considered for the 12V design.<br>Can sit outside the reservoir and connect through tubing.<br>Advertised flow suggests shorter filling times than the peristaltic option. | Startup and stall current need confirmation.<br>Actual flow at the final supply voltage needs testing.<br>Reverse-flow capability and dripping after stopping are not verified. |
| **Adafruit 1150 Peristaltic Pump**<br>[Product information](https://www.adafruit.com/product/1150) | $24.95 | Water stays inside replaceable tubing.<br>Lower flow may make small fills easier to control.<br>Tubing can be removed for cleaning or replacement. | Originally compared as a 12V option.<br>Lower flow increases filling time.<br>Tubing wear can change performance, and this option costs more. |
| **DFRobot FIT0200 Immersible Pump**<br>[Product information](https://www.dfrobot.com/product-667.html) | $9.20 | Lower listed cost.<br>Compact arrangement inside the reservoir.<br>Advertised flow could suit larger fills. | Must remain submerged during operation.<br>Removal and cleaning are more involved.<br>Suitability for small fills, the final voltage, and reverse operation needs confirmation. |

**Current selection: Gikfun EK1856 / R385, pending requirement checks.**

**Why I chose it:** I originally chose the R385 because it fit the planned external tubing arrangement and offered a useful advertised flow rate at a lower cost than the peristaltic option. It also lets us mount the pump outside the reservoir.

**What still needs checking:** I do not have the pump yet. I need to confirm performance at the required supply voltage, operating and startup current, and whether this pump can perform the reverse operation the system needs. If it cannot, the pump selection must change.

**Custom option considered:** Building the pump itself would add work on sealing, moving parts, and consistent flow. A purchased pump lets us focus on the control board and dispensing performance.

## Motor Switching — Current MOSFET Circuit

The microcontroller controls the motor circuit but does not supply motor current directly. The current schematic uses a low-side MOSFET switch. A gate resistor limits gate-charging current, and a pull-down resistor holds the gate low when the controller is not driving it.

These comparisons apply to the current single-MOSFET circuit. They do not represent a completed H-bridge selection.

| Option | Estimated Unit Cost | Pros | Cons |
|---|---|---|---|
| **AOS AOTF2618L**<br>[Manufacturer information](https://www.aosmd.com/products/mosfets/medium-voltage-mosfets-40v-400v/aotf2618l) | $1.90 | Originally selected for the planned logic-level gate drive.<br>Voltage rating provides margin above the nominal motor supply.<br>Symbol and footprint are already included in the project. | Availability needs checking.<br>Needs gate components and motor-current protection.<br>Current, switching loss, and temperature still need verification. |
| **Infineon IRLZ44NPBF**<br>[Datasheet](https://www.infineon.com/assets/row/public/documents/24/49/infineon-irlz44n-datasheet-en.pdf) | $1.80 | Logic-level MOSFET option.<br>Through-hole package is convenient for assembly and testing.<br>Considered as an alternative to the current part. | Pinout and footprint must be checked before substitution.<br>Gate drive and switching frequency need review.<br>The drain-connected metal tab affects mounting. |
| **Infineon IRLB8721PBF**<br>[Datasheet](https://cdn-shop.adafruit.com/datasheets/irlb8721pbf.pdf) | $2.25 | Low on-resistance can reduce conduction loss.<br>Logic-level gate-drive option.<br>Through-hole package is convenient for prototyping. | Lower voltage rating than the other compared parts.<br>Highest estimated price in this comparison.<br>Gate drive, footprint, and temperature need verification. |

**Component in the current schematic: AOTF2618L.**

**Why I chose it:** I originally chose the AOTF2618L for the planned 5V control signal and motor supply. Having its symbol and footprint in the project also made it convenient to include in the schematic. Its final suitability depends on the actual gate voltage, motor current, and temperature.

**Supporting components:** R1 is the 1kΩ series gate resistor. R2 is the 10kΩ gate pull-down resistor. JP1 provides an enable jumper in the control path. These components support the current switching circuit; they do not provide direction control.

**Required revision:** One low-side MOSFET cannot reverse the motor. I need to replace this switching arrangement with an H-bridge that supports the final supply voltage, startup and stall current, and controller logic levels. Motor-current recirculation protection must match that design.

**Custom option considered:** A discrete H-bridge or an integrated H-bridge IC could provide direction control. The final choice has not been made. The motor-control circuitry must be built on our board rather than supplied by a peripheral daughterboard.

## 5V Voltage Regulator

The control electronics use a planned 5V rail. The assignment requires a linear regulator, so I compared three linear options. The pump current does not pass through this regulator.

| Option | Estimated Unit Cost | Pros | Cons |
|---|---|---|---|
| **ST L7805CV**<br>[Datasheet](https://www.st.com/resource/en/datasheet/l78.pdf) | $1.08 | Fixed 5V output keeps the circuit simple.<br>Already included in the schematic.<br>Includes current limiting and thermal protection. | Dissipates heat while reducing the supply voltage.<br>May need a heat sink depending on load and mounting.<br>Requires appropriate capacitors and a thermal check. |
| **TI LM2940CT-5.0/NOPB**<br>[Datasheet](https://www.ti.com/lit/ds/symlink/lm2940-n.pdf) | $2.00 | Fixed 5V output.<br>Lower dropout can help when the input is closer to 5V.<br>Through-hole package is convenient for assembly. | Higher estimated price.<br>Output capacitance and ESR must meet its requirements.<br>Lower dropout does not remove the heat produced at the same input voltage and load. |
| **ST LM317T**<br>[Datasheet](https://www.st.com/resource/en/datasheet/lm317.pdf) | $1.13 | Adjustable output.<br>Through-hole package is convenient for assembly.<br>Includes current limiting and thermal protection. | Needs resistors to set the output voltage.<br>Resistor tolerance affects the output setting.<br>Adds parts when the design only needs a fixed 5V rail. |

**Current selection: L7805CV.**

**Why I chose it:** The L7805 provides the fixed 5V output the control circuit needs without an adjustable resistor network. It also meets the assignment’s linear-regulator requirement.

**Supporting components:** The input and output capacitors support regulator operation and reduce supply disturbances. Local decoupling near the controller helps supply brief changes in current demand. Capacitor values, voltage ratings, and placement need to follow the final regulator and board requirements.

**Power and heat:** The preliminary budget assumes a 106mA control load and a 10mA allowance for the regulator’s own operating current. Under those assumptions, estimated regulator heat is 0.514W at a 9V input or 0.862W at a 12V input. These are calculated estimates, not measurements or verified maximum values. Actual load, mounting, and temperature will determine whether a heat sink is needed.

The Curiosity Nano external-power configuration must also be verified before connecting the regulator to VTG.

**Custom option considered:** The regulator and supporting capacitors will be placed on the pump board. A resistor divider cannot maintain a regulated supply as the load changes. A switching regulator could reduce heat, but it would not meet the stated linear-regulator requirement.

## Flyback Diode — Current MOSFET Circuit

The motor is an inductive load. When the MOSFET turns off, motor current needs a path to continue flowing briefly. The flyback diode provides that path and limits the resulting voltage spike.

| Option | Estimated Unit Cost | Pros | Cons |
|---|---|---|---|
| **Diotec 1N5822**<br>[Datasheet](https://diotec.com/request/datasheet/1n5820.pdf) | $0.71 | Schottky diode option for motor switching.<br>Listed 3A average-current rating.<br>Listed 40V reverse-voltage rating. | Current capability depends on mounting and temperature.<br>Actual motor current must be checked.<br>Protection arrangement must be reconsidered for the H-bridge. |
| **Diotec 1N5819**<br>[Datasheet](https://diotec.com/request/datasheet/1n5817.pdf) | $0.29 | Lowest estimated price.<br>Schottky diode option.<br>Smaller package. | Listed 1A average-current rating leaves less current margin.<br>Motor-current pulses may exceed its suitability.<br>Thermal performance still needs checking. |
| **Vishay SB560-E3/54**<br>[Datasheet](https://www.vishay.com/docs/88721/sb520.pdf) | $1.26 | Listed 5A average-current rating.<br>Listed 60V reverse-voltage rating.<br>Through-hole package is convenient for assembly. | Highest estimated price.<br>Larger package uses more board space.<br>Additional capacity may be unnecessary after motor current is established. |

**Component in the current schematic: 1N5822.**

**Why I chose it:** The 1N5822 offers more rated current capacity than the 1N5819 at a lower estimated cost than the SB560. Its suitability still depends on motor current, switching conditions, and temperature.

In the current single-direction circuit, the cathode connects to motor-supply positive. The anode connects to pump negative and the MOSFET drain.

**Required revision:** This single diode across the motor must not be carried unchanged into a bidirectional H-bridge circuit. It would conduct when motor polarity is reversed. The final H-bridge needs the recirculation paths specified for that driver arrangement.

**Custom option considered:** Other clamping arrangements could affect how quickly motor current decays. The protection choice must be made with the final motor driver and tested for stopping behavior.

## Power Supply — Original 12V Comparison

The original design used a 12V adapter to supply the motor and the input of the 5V regulator. These comparisons document that original decision.

**The project specifies a 9V adapter. None of the 12V models below is a finalized supply selection for that requirement.**

| Option | Earlier Estimated Unit Cost | Pros in the Original 12V Design | Cons |
|---|---|---|---|
| **MEAN WELL GST25A12-P1J**<br>[Datasheet](https://www.meanwell.com/Upload/PDF/GST25A/GST25A-SPEC.PDF) | $18.32 | Listed 12V, 2.08A output.<br>Lowest estimated price in this comparison.<br>Originally considered for the pump and regulator input. | Does not match the specified 9V adapter.<br>Startup current and total load still need checking.<br>Plug compatibility and AC cord requirements need confirmation. |
| **MEAN WELL GST40A12-P1J**<br>[Datasheet](https://www.meanwell.com/Upload/PDF/GST40A/GST40A-SPEC.PDF) | $22.93 | Listed 12V, 3.34A output.<br>More current capacity than the 25W option.<br>Originally considered if the load exceeded the smaller supply. | Does not match the specified 9V adapter.<br>Higher estimated cost.<br>Connectors and wiring still need suitable current ratings. |
| **MEAN WELL GST60A12-P1J**<br>[Datasheet](https://www.meanwell.com/Upload/PDF/GST60A/GST60A-SPEC.PDF) | $24.86 | Listed 12V, 5A output.<br>Highest current capacity in this comparison.<br>Originally considered for additional connected loads. | Does not match the specified 9V adapter.<br>Highest estimated cost.<br>Extra supply capacity does not reduce regulator heating. |

**Original candidate: GST25A12-P1J. Final supply selection: pending.**

**Why I originally chose it:** It provided the planned 12V output at a lower estimated cost than the larger options. That choice was based on the original voltage assumption.

**What changes now:** The final supply must match the project’s voltage requirement and support the completed load calculation, including motor startup. Barrel-jack size, polarity, fuse, wiring, and connector ratings must match the selected supply.

The preliminary budget gives 616mA operating current, or 770mA with a 25% margin, using explicitly assumed loads. This does not establish the current needed during startup or a stall. It also does not include the final H-bridge demand.

Each board uses a local power connection. The pump board does not receive power through its ribbon connector.

**Custom option considered:** A commercial enclosed adapter provides the low-voltage input. The local regulation and motor-control circuitry go on our board.

## Controller and Communication

The current design uses the PIC18F57Q43 Curiosity Nano, which I already have from lab work. It provides the GPIO needed for the command inputs, motor output, and local debugging. The project allows the Curiosity Nano to be used as the controller.

The main controller sends C1 and C0 as two digital signals. C1 connects from ribbon pin 1 to RC3, and C0 connects from ribbon pin 2 to RC2. Read as C1 C0, 00 = Stop, 01 = Run, 10 = Stop, and 11 = Stop. UART and I²C are not used for inter-board commands.

The main controller receives the system’s measurement information and decides when to stop dispensing. My board controls the motor in response to its commands.

UART and I²C are not used for inter-board commands.

## Supporting Connections and Debugging

| Component or Connection | Reason for Including It |
|---|---|
| Barrel jack | Provides a local DC power connection for the board. |
| Fuse | Intended to limit damage from excessive current; its rating must be checked against startup current and wiring limits. |
| Enable jumper | Provides a removable connection in the current motor-control path. |
| Debug LED and series resistor | Provides a local visual indicator while limiting LED current. |
| Debug button, pull-up resistor, and capacitor | Provides a local test input, a defined released state, and input filtering. Firmware may still need debounce handling. |
| Spare header | Makes RD2, RD3, power, and ground available for local testing or a later assigned function. |
| Common ground | Gives the controller, motor circuit, and inter-board signals a shared reference. |

The flyback diode protects against motor-switching transients. It does not provide reverse-polarity protection at the barrel jack.

## Pin Assignments

These assignments match the updated schematic and block diagram.

| Connection | Pin | Purpose |
|---|---|---|
| CMD0 | Ribbon pin 1 → PIC RC3 | First digital command input. |
| CMD1 | Ribbon pin 2 → PIC RC2 | Second digital command input. |
| Common ground | Ribbon pin 8 → GND | Shared signal reference between boards. |
| Unused ribbon connections | Ribbon pins 3–7 | No assigned connection on the pump board. |
| Motor control | PIC RA2 | Drives the current MOSFET gate circuit through the enable jumper and gate resistor. |
| Debug LED | PIC RD0 | Local visual indication for testing. |
| Debug button | PIC RD1 | Local input for testing. |
| Spare header | PIC RD2 and RD3 | Available local connections; no assigned system function. |
| Controller power | Curiosity Nano VTG | Planned 5V regulator connection; external-power setup still needs verification. |
| MOSFET gate | AOTF2618L pin 1 | Receives the control signal through the gate resistor; pull-down connects to ground. |
| MOSFET drain | AOTF2618L pin 2 | Connects to pump negative and the flyback diode anode. |
| MOSFET source | AOTF2618L pin 3 | Connects to ground. |
| Flyback diode cathode | Banded end | Connects to motor-supply positive in the current circuit. |
| Flyback diode anode | Unbanded end | Connects to pump negative and the MOSFET drain in the current circuit. |
| Regulator input | L7805CV pin 1 | Receives the local supply voltage. |
| Regulator ground | L7805CV pin 2 | Connects to ground. |
| Regulator output | L7805CV pin 3 | Provides the planned 5V control supply. |
| Pump positive | Positive terminal | Connects to motor-supply positive in the current circuit. |
| Pump negative | Negative terminal | Connects to the MOSFET drain in the current circuit. |

The motor connections above describe the current single-MOSFET circuit. They will change when the H-bridge is added. Package pinouts and footprints still need verification before board layout.

## MCC Configuration

I configured the PIC18F57Q43 in MCC Melody and generated the initialization files. These settings match the GPIO connections in my current schematic.

| Pin | Generated Name | Configuration | Purpose |
|---|---|---|---|
| RC3 | PUMP_CMD0 | Digital input | Command bit from ribbon pin 1. |
| RC2 | PUMP_CMD1 | Digital input | Command bit from ribbon pin 2. |
| RA2 | MOTOR_CNTRL | Digital output, initially low | Controls the current MOSFET circuit. |
| RD0 | DEBUG_LED | Digital output, initially low | External debug LED. |
| RD1 | DEBUG_BTN | Digital input | Debug button with the external pull-up shown in the schematic. |

Analog functions, internal weak pull-ups, open-drain outputs, and interrupt-on-change are disabled for these five pins. The internal oscillator is configured for 64MHz. UART, I²C, and PWM are not configured.

[Download the MPLAB X project and MCC configuration](Pump_Control_CB.X.zip)

Code generation completed successfully. The generated program initializes the system but does not yet interpret commands or operate the pump. Compilation and hardware operation have not been verified. The configuration will need updating when the H-bridge is added.

## Remaining Design Work

- Confirm a pump that meets the supply-voltage and reverse-operation requirements.
- Select and add an H-bridge with suitable motor-current protection.
- Resolve the 12V design assumption against the specified 9V adapter.
- Replace the power-budget assumptions with documented limits or measurements.
- Include the final H-bridge demand in the power budget.
- Verify regulator temperature and Curiosity Nano external-power configuration.
- Verify input protection, fuse selection, connector ratings, and component footprints.
- Finalize CMD0/CMD1 assignments with the main-controller designer.
- Update the MCC configuration when the final H-bridge connections are selected.
- Update the schematic, block diagram, and component selections together after these decisions.
