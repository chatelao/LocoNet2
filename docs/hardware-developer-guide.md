# LocoNet2 Hardware Developer Guide

This guide explains how to extend the LocoNet2 library to support new hardware.

## The `LocoNetPhy` Class

The `LocoNetPhy` class is the hardware abstraction layer for the LocoNet2 library. It provides a common interface for sending and receiving LocoNet messages, regardless of the underlying hardware.

To add support for a new hardware platform, you must create a new class that inherits from `LocoNetPhy` and implements the `sendLocoNetPacketTry` method. This method is responsible for sending a LocoNet packet to the physical bus.

The `LocoNetPhy` class also provides the `consume` method, which should be called from an interrupt service routine (ISR) whenever a new byte is received from the LocoNet bus. The `consume` method will then assemble the bytes into a complete LocoNet message and pass it to the `LocoNetDispatcher`.

## Creating a New Hardware Interface

Here is a step-by-step guide on how to create a new hardware interface for the LocoNet2 library:

1.  **Create a new header file** for your class (e.g., `LocoNetMyHardware.h`).
2.  **Create a new class** that inherits from `LocoNetPhy`.
3.  **Implement the `sendLocoNetPacketTry` method.** This method should take a pointer to a byte array, the length of the array, and a priority delay as input. It should then send the packet to the LocoNet bus.
4.  **Implement a method to receive bytes from the LocoNet bus.** This method should be called from an ISR and should call the `consume` method with each new byte.
5.  **Create a constructor** for your class that initializes the hardware.
6.  **Create a `begin` method** that enables the hardware.
7.  **Create an `end` method** that disables the hardware.
8.  **(Optional)** Create a platform-specific implementation file (e.g., `LocoNetMyHardware.cpp`) if necessary.

## Platform-Specific Implementation

The LocoNet2 library includes platform-specific implementations for the ESP32 and RP2040 microcontrollers. You can use these as a reference when creating your own hardware interface.

**ESP32:**

*   The `LocoNetESP32` class uses the ESP32's RMT peripheral to generate the LocoNet signal.
*   The `LocoNetESP32` class uses a GPIO pin for receiving LocoNet messages.

**RP2040:**

*   The `LocoNetRP2040` class uses the RP2040's PIO peripheral to generate the LocoNet signal.
*   The `LocoNetRP2040` class uses a GPIO pin for receiving LocoNet messages.
