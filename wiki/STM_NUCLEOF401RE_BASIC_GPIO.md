---
title: STM NUCLEOF401RE BASIC GPIO
permalink: wiki/STM_NUCLEOF401RE_BASIC_GPIO/
layout: wiki
---

A basic guide to how I got an LED to blink on the ST Nucleo F401RE board
without using MBED.

Step One - Install PlatformIO - the website will have the latest details
about how to do this. PlatformIO is a convenient way of installing both
a tool-chain and the CMSIS framework I want to use.

The GPIO pins on the STMF401RET6 device are arranged into three channels
A to C, each with a clock entry on the AHB1 bus. The clock must be
enabled for each channel you wish to use.

On the ST Nucleo F401RE development board, a single LED and resistor are
connected to pin 5 of GPIO channel A. To make it blink on and off (the
“Embedded Hello World”) we need to:

-   set up the GPIO port
-   enable the clock on the GPIO channel
-   toggle the bit on the GPIO channel corresponding to the LED

Strictly speaking, resetting the GPIO isn't required because the GPIO
will be in a reset state when the STMF401RET6 is powered on, however it
is good practice to make sure things are in a known state before using
them. The following code sets the bit in the AHB1 bus reset register
that corresponds to GPIO channel A, this isses a reset signal to GPIO
channel A. The next line brings us out of reset.

    RCC->AHB1RSTR |= RCC_AHB1RSTR_GPIOARST;    // Reset GPIOA 
    RCC->AHB1RSTR = 0;                                            // Exit reset state
