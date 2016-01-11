---
title: STM NUCLEOF401RE TIMER IO
permalink: wiki/STM_NUCLEOF401RE_TIMER_IO/
layout: wiki
---

How to drive the GPIO pins without using the CPU.
-------------------------------------------------

It's all very well to use the CPU to turn on and off an LED connected to
a GPIO pin, but it is wasteful of resources. Having the CPU sit in a
tight delay-loop wasting time until it's time to toggle the GPIO pin
wastes processing time that could be doing something much more useful.

The STM32F4 device features a host of peripherals that, for the most
part, can run independently of the CPU. In this example we'll configure
a timer to toggle the GPIO pin the LED is connected to on the
Nucleo-F401RE board.

#### Connections

Checking the schematic for the Nucleo board and the datasheet for the
CPU, we discover that the LED is connected to pin A5, and that this is
shared with Timer\_2, (actually TIM2\_CH1 / TIM2\_ETR alternate
functions using register AF01). With some digging in the datasheets and
header files for the STM32F401, I was able to figure out that we can map
the GPIO status to the status of Timer\_2. So as the timer reaches a set
value, the GPIO pin will toggle - entirely independently of the CPU. I
actually proved the CPU does nothing by accidentally writing soem buggy
code that set the CPU to a hard fault (crash), but the LED kept on
blinking.
