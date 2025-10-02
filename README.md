# SacrOS-Phone
A Fully Open Source modular phone encouraging innovation and design.

## Overview

This phone (As of PROTO-I) uses the Raspberry Pi Compute Module 5 (CM5) as the main power house along with a custom IO board. In the future, we plan to make a fully custom board, but currently we need to reply on pre-developed architecture.

## Baseplate Spec Sheet

### Components (For Baseplate)

144x Extended Bus Connections for Modular QCMFS (Quick Connect Modular Framework System) ports.

1x USB-C port for power and/or data.

Integrated Power management for CM5 and all peripheral buses.

### Board Dimensions and Constraints (For Baseplate)

High-speed pins: Keep direct, short traces.

Board Size: 145x70mm (Baseplate is 150x75mm).

High-speed pins: Keep direct, short traces.

Low-speed pins: Can route through expanders/buffers.

### Pin Architecture

The 144 pins are divided into tiers:

3.1 Tier 1 – High-Priority Pins

Number of pins: ~30–40 (configurable).

Connection: Directly to CM5 GPIO.

Purpose: Timing-critical tasks, high-speed signals.

Routing: Short traces, dedicated ground/power planes, minimal interference.

3.2 Tier 2 – Low-Priority Pins

Number of pins: Remaining ~104–114 pins.

Connection: Routed through GPIO expanders (I²C/SPI) and bus buffers.

Purpose: Non-time-critical tasks, mass peripheral support.

Routing: Dense routing acceptable, lower speed tolerances.

Optional Flex: Some Tier 2 pins can be routed via a flex PCB segment to fit inside the device.

### Expanders & Buffers

Expanders: I²C/SPI GPIO expanders allow chaining to reach 100+ pins.

Bus Buffers: Isolate expanders from CM5 when not in use, reduce interference.

Tradeoff: Tier 2 pins slower due to bus latency, but fully addressable.

### USB-C Port

Usage: Power only, or power + data depending on choice.

Type: Low-profile SMT USB-C for thin PCB.

Placement: Near edge of the board for external accessibility.

Power Delivery: Optional PD IC for dynamic voltage/current negotiation.

### Power Management

CM5: 5V supply via USB-C or external source (such as one of the modules).

Tier 1 Pins: Powered directly from regulated 3.3V/1.8V rails.

Tier 2 Pins: Powered through expanders, allowing slightly lower voltage tolerance.

### OS and Kernel Integration

Device Tree Overlay: Map all 144 pins (Tier 1 and Tier 2) to virtual GPIOs.

Driver Module Features:

Allows dynamic allocation of high-speed vs low-speed pins.

Exposes all pins in /sys/class/gpio or through a custom API.

Can prioritize specific pins for kernel real-time tasks.

Software Stack:

Tier 1 pins: Direct access for real-time kernel tasks. (Manageable in Software by Kernel)

Tier 2 pins: Access via expanders, slower polling or interrupt-driven. (Manageable in Software by Kernel)
