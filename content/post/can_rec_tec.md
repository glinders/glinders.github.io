+++
date = 2025-12-24T07:57:36+13:00
title = "CAN FD REC and TEC"
author = "Geert"
description = "Description of error counters in CAN FD"
categories = ["CAN FD"]
tags = ["REC", "TEC",]
draft = false
+++

# Introduction
In CAN FD, the Transmit Error Counter (TEC) and Receive Error Counter (REC) are part of the CAN error management defined by ISO 11898-1. These counters dictate the node's Error State, preventing a faulty node from permanently affecting the communication on the bus.

The rules are the same as in classic CAN, but CAN FD introduces the ESI (Error State Indicator) bit to communicate the error state of a node to the rest of the bus.

# Error States

| **State** | **Condition** | **Behavior** |
|-----------|---------------| -------------|
| Error Active | TEC ≤ 127 and REC ≤ 127 | The node can send Active Error Flags (6 dominant bits) to mark a faulty frame. It participates fully in the bus. |
| Error Passive | TEC > 127 or REC > 127 | The node sends Passive Error Flags (6 recessive bits). It cannot stop others from communicating even if it detects an error. |
| Bus Off | TEC > 255 | The node is disconnected from the bus by the controller. It cannot transmit or receive anything until a reset/recovery sequence occurs. |

Many controllers also have an Error Warning limit (typically 96), which triggers an interrupt to warn the software before the node goes passive.

# Transmit Error Counter (TEC)

Increments when the node transmits an error or when its transmitted frame is not acknowledged correctly.
Decrements when the node successfully transmits a frame.

Increment size depends on error severity:
- Bit error in its own frame; +8
- ACK error; +1
- Lost arbitration; no increment (normal behaviour)

Decrement:
 - -1 for each successful transmission.

 # Receive Error Counter (REC)

Increment:
- When the node detects an error in a received frame; +1

Decrement:
- When a frame is received correctly: -1

REC never causes Bus-Off; it only affects error-active vs error-passive state.

# Error State Indicator (ESI)

Every CAN FD frame contains an ESI bit in the header:
- Dominant (0); The transmitter is currently Error Active.
- Recessive (1): The transmitter is currently Error Passive.

The health of every node on the bus can be determined by looking at the traffic.