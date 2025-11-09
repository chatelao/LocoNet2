# LocoNet Specification Compliance Matrix

This document tracks the implementation status of the LocoNet specification in the LocoNet2 library.

## General Commands

| Opcode | Name | Implemented | Notes |
|---|---|---|---|
| 0x81 | OPC_BUSY | Yes | Basic busy message. |
| 0x82 | OPC_GPOFF | Yes | Global power off. Handled by `reportPower` and `onPowerChange`. |
| 0x83 | OPC_GPON | Yes | Global power on. Handled by `reportPower` and `onPowerChange`. |
| 0x85 | OPC_IDLE | Yes | Bus idle message. |

## Switch and Sensor Commands

| Opcode | Name | Implemented | Notes |
|---|---|---|---|
| 0xB0 | OPC_SW_REQ | Yes | Switch request. Handled by `onSwitchRequest`. |
| 0xB1 | OPC_SW_REP | Yes | Switch report. Handled by `onSwitchReport`. |
| 0xB2 | OPC_INPUT_REP | Yes | Input (sensor) report. Handled by `onSensorChange`. |
| 0xBC | OPC_SW_STATE | Yes | Switch state report. Handled by `onSwitchState`. |
| 0xBD | OPC_SW_ACK | No | No explicit handler in `LocoNetDispatcher`. |

## Locomotive Commands

| Opcode | Name | Implemented | Notes |
|---|---|---|---|
| 0xA0 | OPC_LOCO_SPD | Yes | Can be sent, but no specific dispatcher handler. |
| 0xA1 | OPC_LOCO_DIRF | Yes | Can be sent, but no specific dispatcher handler. |
| 0xA2 | OPC_LOCO_SND | Yes | Can be sent, but no specific dispatcher handler. |
| 0xBF | OPC_LOCO_ADR | Yes | Can be sent, but no specific dispatcher handler. |

## Slot Management Commands

| Opcode | Name | Implemented | Notes |
|---|---|---|---|
| 0xB5 | OPC_SLOT_STAT1 | No | No explicit handler in `LocoNetDispatcher`. |
| 0xB6 | OPC_CONSIST_FUNC | No | No explicit handler in `LocoNetDispatcher`. |
| 0xB8 | OPC_UNLINK_SLOTS | No | No explicit handler in `LocoNetDispatcher`. |
| 0xB9 | OPC_LINK_SLOTS | No | No explicit handler in `LocoNetDispatcher`. |
| 0xBA | OPC_MOVE_SLOTS | No | No explicit handler in `LocoNetDispatcher`. |
| 0xBB | OPC_RQ_SL_DATA | No | No explicit handler in `LocoNetDispatcher`. |
| 0xE7 | OPC_SL_RD_DATA | No | No explicit handler in `LocoNetDispatcher`. |
| 0xEF | OPC_WR_SL_DATA | No | No explicit handler in `LocoNetDispatcher`. |

## Programming and Configuration

| Opcode | Name | Implemented | Notes |
|---|---|---|---|
| 0xE5 | OPC_PEER_XFER | No | No explicit handler in `LocoNetDispatcher`. |
| 0xED | OPC_IMM_PACKET | No | No explicit handler in `LocoNetDispatcher`. |

## Extended Commands

| Opcode | Name | Implemented | Notes |
|---|---|---|---|
| 0xD0 | OPC_MULTI_SENSE | Yes | Multi-sense (transponding). Handled by `onMultiSenseDeviceInfo` and `onMultiSenseTransponder`. |

## Miscellaneous

| Opcode | Name | Implemented | Notes |
|---|---|---|---|
| 0xB4 | OPC_LONG_ACK | Yes | `makeLongAck` function exists, but no explicit dispatcher handler. |
| 0xB3 | OPC_UNKNOWN | Yes | Generic handling for unknown opcodes. |
