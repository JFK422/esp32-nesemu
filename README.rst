ESP32-NESEMU, a Nintendo Entertainment System emulator for the ESP32
====================================================================

This is a quick and dirty port of Nofrendo, a Nintendo Entertainment System emulator. It lacks sound, but can emulate a NES at close
to full speed, albeit with some framedrop due to the way the display is driven.

Warning
-------

This is a proof-of-concept and not an official application note. As such, this code is entirely unsupported by Espressif.
The parallel interface code was written by Bodmer and is part of the TFT-eSPI library and is simply frankensteined into this project.


Compiling
---------

This code is an esp-idf project. You will need esp-idf to compile it. Newer versions of esp-idf may introduce incompatibilities with this code;
for your reference, the code was tested against commit 12caaed28063e32d8b1fb13e13548b6fa52f87b3 of esp-idf.


Display
-------

To display the NES output, please connect a 320x240 ST7789-based 8-Bit Parallel display to the ESP32 in this way:

    =====  =======================
    Pin    GPIO
    =====  =======================
    CS      33  // Chip select control pin (permanently low)
    DC      15  // Data Command control pin - must use a pin in the range 0-31
    RST     16  // Reset pin, toggles on startup
    WR      4   // Write strobe control pin - must use a pin in the range 0-31
    RD      2   // Read strobe control pin
    =====  =======================

    =====  =======================
    Pin    GPIO
    =====  =======================
    D0      12  // Must use pins in the range 0-31 for the data bus
    D1      17  // so a single register write sets/clears all bits.
    D2      5   // Pins can be randomly assigned, this does not affect
    D3      21  // TFT screen update performance.
    D4      25
    D5      26
    D6      27
    D7      14
    =====  =======================

Also connect the power supply and ground. For now, the LCD is controlled using a SPI peripheral, fed using the 2nd CPU. This is less than ideal; feeding
the SPI controller using DMA is better, but was left out due to this being a proof of concept.


Controller
----------

To control the NES, connect a Playstation 1 or 2 controller as such:

    =====  =====
    Pin    GPIO
    =====  =====
    CLK    14
    DAT    27
    ATT    16
    CMD    2
    =====  =====

Also connect the power and ground lines. Most PS1/PS2 controllers work fine from a 3.3V power supply, if a 5V one is unavailable.

It is planned to controll it via dedicated buttons but not yet implemented.

ROM
---
This NES emulator does not come with a ROM. Please supply your own and flash to address 0x00100000. You can use the flashrom.sh script as a template for doing so.

Copyright
---------

Code in this repository is Copyright (C) 2016 Espressif Systems, licensed under the Apache License 2.0 as described in the file LICENSE. Code in the
components/nofrendo is Copyright (c) 1998-2000 Matthew Conte (matt@conte.com) and licensed under the GPLv2.
TFT_eSPI code is under Copyright (C) by Bodmer under MIT License.
I dont claim any of the code in this repo to be mine. Im simply the one who put it together.

