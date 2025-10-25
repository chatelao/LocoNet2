# Usage

## Dependencies

This project requires the [Embedded Template Library](https://www.etlcpp.com/). If you are using PlatformIO, you can add it to your `platformio.ini` file like this:

```ini
lib_deps =
  etlcpp/Embedded Template Library
```

## Basic Usage

To use the LocoNet2 library, you need to instantiate a few core components.

First, you need a `LocoNetBus`. This object represents the physical LocoNet bus.

```cpp
#include <LocoNetBus.h>

LocoNetBus bus;
```

Next, you need a `LocoNetDispatcher`. This object is responsible for parsing and dispatching LocoNet messages to your application. It takes a pointer to the `LocoNetBus` as a constructor argument.

```cpp
#include <LocoNetDispatcher.h>

LocoNetDispatcher dispatcher(&bus);
```

Finally, you need a hardware-specific class to handle the physical communication. For example, if you are using an ESP32, you would use `LocoNetStreamESP32`. This class takes the UART number, RX and TX pin numbers, and a pointer to the `LocoNetBus` as constructor arguments.

```cpp
#include <LocoNetStreamESP32.h>

#define LOCONET_PIN_RX 33
#define LOCONET_PIN_TX 32

LocoNetStreamESP32 lnStream(1, LOCONET_PIN_RX, LOCONET_PIN_TX, true, true, &bus);
```

In your `setup()` function, you must call the `start()` method on your hardware-specific object:

```cpp
void setup() {
  // ... your other setup code ...
  lnStream.start();
  // ...
}
```

And in your `loop()` function, you must call the `process()` method:

```cpp
void loop() {
  lnStream.process();
  // ... your other loop code ...
}
```

## Receiving Messages

The `LocoNetDispatcher` allows you to subscribe to different types of LocoNet messages using callbacks. These are typically registered in your `setup()` function.

### All Packets

You can subscribe to all incoming LocoNet packets using the `onPacket()` method.

```cpp
dispatcher.onPacket(CALLBACK_FOR_ALL_OPCODES, [](const lnMsg *rxPacket) {
  // This code will be executed for every LocoNet message received.
  // You can inspect the rxPacket to get the message details.
});
```

### Switch Requests

To handle switch requests, use the `onSwitchRequest()` method.

```cpp
dispatcher.onSwitchRequest([](uint16_t address, bool output, bool direction) {
  // address: The address of the switch (1-2048).
  // output: The state of the output (true for on, false for off).
  // direction: The direction of the switch (true for closed, false for thrown).
});
```

### Switch/Sensor Reports

Use `onSwitchReport()` to handle reports from switches or sensors.

```cpp
dispatcher.onSwitchReport([](uint16_t address, bool state, bool sensor) {
  // address: The address of the switch/sensor.
  // state: The state of the switch/sensor (true for active, false for inactive).
  // sensor: The type of report (true for switch, false for aux).
});
```

### Sensor Changes

To handle sensor state changes, use the `onSensorChange()` method.

```cpp
dispatcher.onSensorChange([](uint16_t address, bool state) {
  // address: The address of the sensor.
  // state: The state of the sensor (true for active, false for inactive).
});
```

## Sending Messages

You can send LocoNet messages using the `send()` method on the `LocoNetDispatcher` object. This method takes the OPC (opcode) and up to two data bytes.

```cpp
// Send a custom LocoNet message
dispatcher.send(0xA0, 0x03, 0x00);
```

The library also provides several helper functions to send common LocoNet messages. For example, to report a sensor state change, you can use the `reportSensor()` function.

```cpp
#include <LocoNet.h>

// Report sensor 123 as active
reportSensor(&bus, 123, 1);
```

## Hardware Configuration

The hardware-specific class (e.g., `LocoNetStreamESP32`) requires you to define the pins for RX and TX. It's common to use `#define` for this.

```cpp
#define LOCONET_PIN_RX 33
#define LOCONET_PIN_TX 32
```

Depending on your hardware interface, you may also need to invert the RX and TX signals.

```cpp
#define LOCONET_UART_RX_SIGNAL_INVERT  true
#define LOCONET_UART_TX_SIGNAL_INVERT  true
```

These values are then passed to the constructor of the hardware-specific class:

```cpp
LocoNetStreamESP32 lnStream(1,
                           LOCONET_PIN_RX,
                           LOCONET_PIN_TX,
                           LOCONET_UART_RX_SIGNAL_INVERT,
                           LOCONET_UART_TX_SIGNAL_INVERT,
                           &bus);
```
