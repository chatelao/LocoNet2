# LocoNet2 Hardware Developer Guide

This guide provides information for hardware developers who want to use the LocoNet2 library with their custom hardware.

## Supported Microcontrollers

The LocoNet2 library is designed to be platform-independent, but it includes specific support for the following microcontrollers:

*   **ESP32:** The library includes a hardware-specific implementation for the ESP32, which uses the UART peripheral to communicate with the LocoNet bus.
*   **Raspberry Pi Pico (RP2040):** The library includes a hardware-specific implementation for the RP2040, which uses the PIO peripheral to communicate with the LocoNet bus.

## Wiring Diagrams

To connect your microcontroller to a LocoNet bus, you will need a LocoNet interface circuit. A common and effective circuit is the one designed by John Kabat.

### ESP32 Wiring

| ESP32 Pin | LocoNet Interface |
|---|---|
| GPIO2 | TX |
| GPIO4 | RX |

### Raspberry Pi Pico (RP2040) Wiring

| RP2040 Pin | LocoNet Interface |
|---|---|
| GPIO0 | TX |
| GPIO1 | RX |

## Custom Hardware

If you are using a microcontroller that is not officially supported, you will need to create a custom implementation of the `LocoNetPhy` class. This class is responsible for the low-level communication with the LocoNet bus.

You will need to implement the `sendLocoNetPacketTry` method, which is responsible for sending a LocoNet packet to the bus. You will also need to call the `consume` method whenever a new byte is received from the bus.
