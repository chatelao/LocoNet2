# LocoNet2 End-User Guide

Welcome to the LocoNet2 library! This guide will help you get started with using the library in your Arduino projects.

## Installation

The recommended way to install the LocoNet2 library is through the PlatformIO IDE.

1.  Open the PlatformIO IDE.
2.  Navigate to the "Libraries" section.
3.  Search for "LocoNet2".
4.  Click "Install" to add the library to your project.

Alternatively, you can manually install the library by downloading the source code from the GitHub repository and placing it in your Arduino `libraries` folder.

## Basic Usage

To use the LocoNet2 library, you will need to include the `LocoNet2.h` header file in your sketch.

```cpp
#include <LocoNet2.h>
```

You will then need to create a `LocoNetBus` object and a `LocoNetDispatcher` object. The `LocoNetBus` represents the physical LocoNet bus, and the `LocoNetDispatcher` is used to send and receive LocoNet messages.

```cpp
LocoNetBus lnBus;
LocoNetDispatcher ln(&lnBus);
```

### Receiving Messages

To receive LocooNet messages, you can register a callback function with the `onPacket` method of your `LocoNetDispatcher` object. This function will be called whenever a LocoNet message is received.

```cpp
void onLocoNetPacket(const LnMsg* msg) {
  // Handle the received message
  Serial.printf("Received LocoNet message: OPC_LOCO_SPD %d\\n", msg->data[0]);
}

void setup() {
  ln.onPacket(OPC_LOCO_SPD, onLocoNetPacket);
}
```

### Sending Messages

To send a LocoNet message, you can use the `send` method of your `LocoNetDispatcher` object.

```cpp
void loop() {
  // Send a speed command to locomotive 3
  ln.send(OPC_LOCO_SPD, 3, 100);
  delay(1000);
}
```

## Example Sketch

Here is a complete example sketch that demonstrates how to receive and send LocoNet messages.

```cpp
#include <Arduino.h>
#include <LocoNet2.h>

LocoNetBus lnBus;
LocoNetDispatcher ln(&lnBus);

void onLocoNetPacket(const LnMsg* msg) {
  Serial.printf("Received LocoNet message: OPC_LOCO_SPD %d\\n", msg->data[0]);
}

void setup() {
  Serial.begin(115200);
  ln.onPacket(OPC_LOCO_SPD, onLocoNetPacket);
}

void loop() {
  // Send a speed command to locomotive 3
  ln.send(OPC_LOCO_SPD, 3, 100);
  delay(1000);
}
```
