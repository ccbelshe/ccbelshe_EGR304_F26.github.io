
# Component Selection

**Fill-A-Bot — Pump Control Subsystem**

## Overview

My subsystem controls the 12V water pump used to move water from the reservoir into the container. The main controller sends two digital command signals to my PIC18F57Q43, which determines how the motor should operate.

I selected the components based on motor control, power requirements, ease of assembly, and compatibility with the other boards. The design now uses a DRV8871 H-bridge instead of the original single-MOSFET circuit because the motor needs to operate in both directions.

The schematic and block diagram reflect the 12V design. The final supply arrangement and motor performance still need to be confirmed during testing.

## Water Pump and Motor

The pump needs to provide a useful flow rate while allowing the system to stop dispensing consistently. I also wanted a pump that could be mounted outside the reservoir and connected using tubing.

| Option | Estimated Cost | Advantages | Limitations |
|---|---|---|---|
| Gikfun R385 | $10.88 | External mounting, compact design, useful advertised flow rate | Startup current and reverse operation need testing |
| Adafruit 1150 Peristaltic Pump | $24.95 | Water stays inside tubing, easier cleaning | Higher cost and slower flow |
| DFRobot FIT0200 | $9.20 | Compact and inexpensive | Submersible design makes maintenance less convenient |

**Selected component: Gikfun R385 12V Water Pump**

I selected the R385 because it fits our planned tubing arrangement and provides a reasonable balance between size, flow rate, and cost.

The pump assembly includes the DC motor, so a separate motor purchase is not required.

Reversing the motor electrically does not guarantee that the pump will move water backward. I will verify its actual behavior when the hardware is available.

## Motor Driver

The motor cannot be powered directly from the PIC because its current requirements exceed what a microcontroller output can provide.

My original design used a MOSFET to switch the pump on and off. That approach worked for single-direction operation but did not meet the forward-and-reverse requirement.

| Option | Advantages | Limitations |
|---|---|---|
| AOTF2618L MOSFET | Simple circuit, suitable for basic switching | Cannot reverse the motor by itself |
| Discrete H-bridge | Supports forward and reverse operation | Requires more components and design work |
| TI DRV8871 H-bridge | Integrated bidirectional motor driver with current regulation | Current limit and thermal performance must be verified |

**Selected component: Texas Instruments DRV8871**

I selected the DRV8871 because it provides bidirectional motor control in one integrated device. The PIC supplies the control signals, while the driver handles the motor current.

The DRV8871 uses two control inputs and two motor outputs. This allows the subsystem to control motor direction without sending motor current through the PIC.

The design includes a 68kΩ resistor connected to the driver's ILIM input.

Using the datasheet equation:

**I_TRIP = 64 / R_ILIM(kΩ)**

**I_TRIP = 64 / 68 ≈ 0.94A**

This provides an estimated nominal current-trip threshold of 0.94A. I will check whether that setting is appropriate after measuring the pump's operating and startup current.

The DRV8871 handles inductive motor-current recirculation through its internal switching circuitry. The separate flyback diode from my original MOSFET design is no longer used across the motor.

[DRV8871 Datasheet](https://www.ti.com/lit/ds/symlink/drv8871.pdf)

## Microcontroller

**Selected component: Microchip PIC18F57Q43 Curiosity Nano**

I already have the Curiosity Nano from previous lab work, and it provides the digital inputs and outputs needed for this subsystem.

The PIC receives commands from the main controller, interprets the requested operating state, and sends control signals to the DRV8871.

The command structure is:

| Command | Operation |
|---|---|
| 00 | Stop |
| 01 | Forward |
| 10 | Reserved / Stop |
| 11 | Reverse |

These are the commands interpreted by the PIC, not the DRV8871's direct input truth table.

Using direct digital signals also meets our team's requirement for pin-to-pin communication without UART or I2C.

## Voltage Regulator

The control electronics require a regulated 5V supply. I compared three linear regulators before selecting the L7805.

| Option | Estimated Cost | Advantages | Limitations |
|---|---|---|---|
| ST L7805CV | $1.08 | Fixed 5V output, simple implementation | Heat dissipation at higher input voltages |
| TI LM2940CT-5.0 | $2.00 | Lower dropout voltage | More expensive and requires specific output capacitor characteristics |
| ST LM317T | $1.13 | Adjustable output | Requires additional resistors to set 5V |

**Selected component: ST L7805CV**

I selected the L7805 because it provides a fixed 5V output without requiring an adjustable resistor network.

The regulator supplies the control electronics, not the pump motor. Input and output capacitors help maintain stable operation.

Since the design uses a nominal 12V input, I will check regulator heating during testing. I also need to verify the correct external-power configuration for the Curiosity Nano before connecting the regulated supply.

[L7805 Datasheet](https://www.st.com/resource/en/datasheet/l78.pdf)

## Power Supply and Protection

The current schematic uses a nominal 12V supply for the pump driver and the input of the 5V regulator.

| Supply Option | Estimated Cost | Output | Consideration |
|---|---|---|---|
| MEAN WELL GST25A12-P1J | $18.32 | 12V, 2.08A | Lowest-cost option in the original comparison |
| MEAN WELL GST40A12-P1J | $22.93 | 12V, 3.34A | Additional current capacity |
| MEAN WELL GST60A12-P1J | $24.86 | 12V, 5A | More capacity than currently estimated |

**Current design voltage: 12V**

The R385 pump kit was selected for the 12V design. The final supply must still be checked against the course requirements and measured motor current.

Each subsystem has its own barrel-jack power connection. The ribbon cable is used for communication and a shared ground reference, not to distribute motor power.

The schematic also includes a fuse for input overcurrent protection. Fuse suitability, supply polarity, and connector ratings need to be checked before hardware testing.

## Communication and Connections

The main controller communicates with my subsystem through an eight-pin ribbon connector.

The team interface reserves five positions for digital signals, two for analog signals, and one for ground. My subsystem currently uses two digital command signals and the common ground connection.

| Connection | Function |
|---|---|
| CMD0 and CMD1 | Digital commands received by the PIC |
| Common GND | Shared signal reference between boards |
| PIC motor-control outputs | Control the DRV8871 inputs |
| DRV8871 motor outputs | Connect to the two terminals of the R385 pump motor |
| 12V supply | Powers the motor driver |
| 5V regulator output | Supplies the control electronics |

I chose direct pin-to-pin communication because it keeps the interface straightforward and avoids requiring a separate serial communication protocol.

The exact GPIO assignments for both H-bridge control inputs must match the latest KiCad schematic and firmware configuration.

## Supporting Components

| Component | Purpose |
|---|---|
| DC barrel jack | Independent power input |
| Fuse | Input overcurrent protection |
| 68kΩ ILIM resistor | Sets the DRV8871 current-trip threshold |
| 0.1µF bypass capacitor | Local high-frequency supply decoupling |
| 47µF bulk capacitor | Helps support changes in motor supply current |
| Debug LED | Visual indication during testing |
| Debug pushbutton | Manual test input |
| Spare GPIO header | Additional connections for testing |
| Ribbon connector | Communication with the other boards |
| Pump connector | Connection to the external 12V pump motor |

Common resistors and capacitors are available through the lab. The PIC is already owned, so these items do not need to be purchased again.

## Firmware and MCC Configuration

I previously configured the PIC18F57Q43 using MCC Melody and generated initialization files for the original motor-control design.

The existing configuration includes the command inputs and local debugging connections. However, the firmware must be updated to drive both DRV8871 control inputs and interpret the reverse command correctly.

[Download the MPLAB X project and MCC configuration](Pump_Control_CB.X.zip)

Generating the initialization files does not verify that the completed firmware operates the motor. That will require compilation and hardware testing.

## Testing and Verification

Once the hardware is assembled, I will verify the following:

- The 12V input and regulated 5V supply operate correctly.
- The PIC receives and interprets the command signals.
- The DRV8871 responds correctly to forward, reverse, and stop commands.
- The pump's operating and startup currents are compatible with the driver and current-limit setting.
- The regulator and motor driver remain within acceptable temperatures.
- The pump stops consistently and its behavior during reverse operation is understood.
- The ribbon interface and shared ground work correctly with the other boards.

## Conclusion

The main component choices are now established: the PIC18F57Q43 handles control logic, the DRV8871 drives the motor, the R385 pump moves water, and the L7805 provides the regulated control supply.

The design has progressed from a basic MOSFET switching circuit to a bidirectional motor-control subsystem. The next step is verifying the electrical connections and actual hardware performance before completing the PCB layout.
