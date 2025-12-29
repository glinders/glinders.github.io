+++
title = "WeAct Studio USB2CANFD"
date = 2025-12-29T13:59:15+13:00
author = "Geert"
description = "Isolated USB to CAN FD interface"
categories = ["CAN FD"]
tags = ["WeAct Studio", "USB2CANFD",]
draft = false
+++

# Table of contents
1. [Introduction](#introduction)
    1. [Links](#links)
2. [Firmware](#firmware)
3. [Flash Firmware](#flash_firmware)
    1. [Programming Header](#programming_header)
    2. [STM32 Cube Programmer](#stm32cubeprogrammer)
    3. [Precompiled Image](#precompiled_image)

## Introduction <a name="introduction"></a>

The WeAct Studio USB2CANFD USB to CAN FD interface features:
- 1500V isolation using a DC-DC converter
- a switch to enable/disable termination resistor
- [STM32G0B1CB](https://www.st.com/en/microcontrollers-microprocessors/stm32g0b1cb.html) CPU
- [SIT1051A/3](http://www.sitcores.com/uploadfile/2023/1108/20231108095540224.pdf) CAN FD transceiver ([datasheet](/can/SIT1051_Datasheet.pdf))

[#]: # (Leave an empty line before the image and its attributes, otherwise they won't be used)

![WeAct Studio USB2CANFD image](/can/weact_studio_usb2canfd.webp)
{width="525"}

### Related Links <a name="links"></a>

Hardware design: https://github.com/WeActStudio/WeActStudio.USB2CANFDV1  
Built-in firmware: https://github.com/WeActStudio/WeActStudio.USB2CANFDV1/tree/master/Firmware  
Buy on aliExpress: https://www.aliexpress.com/item/1005007126451299.html

## Firmware <a name="firmware"></a>

The built-in firmware uses the `slcan` protocol, with CAN FD extensions. It works well with Python-CAN on Windows and Linux.

The [candleLight_fw](https://github.com/romainreignier/candleLight_fw) firmware is a
[gs_usb](https://github.com/torvalds/linux/blob/master/drivers/net/can/usb/gs_usb.c) compatible firmware
that supports this device and CAN FD in branch [add_weactstudio_usb2canfdv1](https://github.com/romainreignier/candleLight_fw/tree/add_weactstudio_usb2canfdv1).

The `candleLight_fw` firmware registers a CAN node with [SocketCAN](https://www.kernel.org/doc/html/latest/networking/can.html) on Linux.

To flash the `candleLight_fw` firmware, see the instructions below. I used SWD, accessible through a programming header to the board as detailed below.

## Flash Firmware <a name="flash_firmware"></a>

You will need an [ST-LINK/V2](https://www.st.com/en/development-tools/st-link-v2.html)
or [STLINK-V3SET](https://www.st.com/en/development-tools/stlink-v3set.html)
programmer and matching software
([STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html))

### Programming Header <a name="programming_header"></a>

First, add an `SWD` programming header. There are 4 pads on the board for a standard SWD/JTAG programmer, as shown below:

![WeAct Studio USB2CANFD SWD connection](/can/weact_studio_usb2canfd_swd.webp)
{width="525"}

If the header is attached vertically it will interfere with the top plastic cover that goes on the board. If you mount the header sideways then the top plastic cover will fit nicely:

![WeAct Studio USB2CANFD programming header](/can/weact_studio_usb2canfd_header.webp)
{width="525"}

### STM32 Cube Programmer <a name="stm32cubeprogrammer"></a>

You first have to set up the device to be reprogrammed. Follow steps 1 to 7 in the image below (click to enlarge):

[![Set up device using STM32 Cube Programmer](/can/stm32cubeprogrammer_setup.png)](/can/stm32cubeprogrammer_setup.png)

You might need to run STM32 Cube Programmer as root on Linux.

### Precompiled image <a name="precompiled_image"></a>

Here is a precompiled image: [WeActStudio_USB2CANFDV1_fw.bin](/can/WeActStudio_USB2CANFDV1_fw.bin)

This has to be programed at address 0x08000000.

