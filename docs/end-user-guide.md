# LocoNet2 End-User Guide

This guide provides instructions on how to use the LocoNet2 library in your projects.

## Installation

The LocoNet2 library is designed to be used with PlatformIO. To use it in your project, add the following to your `platformio.ini` file:

```ini
lib_deps =
    EEPROM
    etlcpp/Embedded Template Library
    https://github.com/mrrwa/LocoNet2.git
```

## Basic Usage

Here is a basic example of how to use the LocoNet2 library to listen for LocoNet messages on an ESP32:

```cpp
#include <Arduino.h>
#include "LocoNetESP32.h"
#include "LocoNet2.h"

LocoNetESP32 ln(14, 12, 0);
LocoNetDispatcher dispatcher(&ln);

void setup() {
  Serial.begin(115200);
  Serial.println("LocoNet2 Basic Listener Example");

  ln.begin();

  dispatcher.onPacket(CALLBACK_FOR_ALL_OPCODES, [](const lnMsg *msg) {
    Serial.print("Received LocoNet message: ");
    for (int i = 0; i < msg->sz.command; i++) {
      Serial.printf("%02X ", ((uint8_t*)msg)[i]);
    }
    Serial.println();
  });
}

void loop() {
  ln.receive();
}
```

## Core Classes

### LocoNetDispatcher

The `LocoNetDispatcher` class is the central hub for handling LocoNet messages. It receives messages from a `LocoNetPhy` object and dispatches them to registered callbacks.

**Key Methods:**

* `LocoNetDispatcher(LocoNetBus *ln)`: The constructor takes a pointer to a `LocoNetBus` object.
* `void onPacket(uint8_t opCode, std::function<void (const LnMsg *)> callback)`: Registers a callback function to be called when a message with a specific opcode is received. You can use `CALLBACK_FOR_ALL_OPCODES` to register a callback for all messages.
* `void onSensorChange(std::function<void (uint16_t, bool)> callback)`: Registers a callback for when a sensor changes state.
* `void onSwitchRequest(std::function<void (uint16_t, bool, bool)> callback)`: Registers a callback for when a switch is requested.
* `void onSwitchReport(std::function<void (uint16_t, bool, bool)> callback)`: Registers a callback for when a switch or sensor status is reported.
* `void onPowerChange(std::function<void (bool)> callback)`: Registers a callback for when the power status changes.
* `LN_STATUS send(LnMsg *txPacket)`: Sends a LocoNet message.
* `LN_STATUS send(uint8_t opCode, uint8_t data1, uint8_t data2)`: Sends a simple LocoNet message with two data bytes.

### LocoNetFastClock

The `LocoNetFastClock` class provides an interface to the LocoNet fast clock.

**Key Methods:**

* `LocoNetFastClock(LocoNet &locoNet, bool DCS100CompatibleSpeed, bool CorrectDCS100Clock)`: The constructor takes a reference to a `LocoNet` object and two boolean flags to control compatibility with the DCS100 command station.
* `void onUpdate(std::function<void (uint8_t, uint8_t, uint8_t, uint8_t, bool)> callback)`: Registers a callback that is triggered when the fast clock updates. The callback receives the rate, day, hour, minute, and a sync flag.
* `void onFractionalMinUpdate(std::function<void (uint16_t)> callback)`: Registers a callback for fractional minute updates.
* `void poll()`: This method should be called in your main loop to process incoming LocoNet messages and update the fast clock.

### LocoNetThrottle

The `LocoNetThrottle` class provides a high-level interface for controlling a locomotive.

**Key Methods:**

* `LocoNetThrottle(LocoNetDispatcher *locoNet)`: The constructor takes a pointer to a `LocoNetDispatcher` object.
* `void init(uint8_t userData, uint8_t options, uint16_t throttleId)`: Initializes the throttle.
* `TH_ERROR setAddress(uint16_t Address)`: Sets the address of the locomotive to control.
* `TH_ERROR setSpeed(uint8_t Speed)`: Sets the speed of the locomotive.
* `TH_ERROR setDirection(uint8_t Direction)`: Sets the direction of the locomotive.
* `TH_ERROR setFunction(uint8_t Function, uint8_t Value)`: Sets the state of a function (e.g., lights, horn).
* `void onAddressChange(std::function<void (LocoNetThrottle *, uint16_t, uint16_t)> callback)`: Registers a callback that is triggered when the address changes.
* `void onSpeedChange(std::function<void (LocoNetThrottle *, uint8_t)> callback)`: Registers a callback that is triggered when the speed changes.
* `void onDirectionChange(std::function<void (LocoNetThrottle *, uint8_t)> callback)`: Registers a callback that is triggered when the direction changes.
* `void onFunctionChange(std::function<void (LocoNetThrottle *, uint8_t, bool)> callback)`: Registers a callback that is triggered when a function changes.
