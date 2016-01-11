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
functions using register AF01).

With some digging in the datasheets and header files for the STM32F401,
I was able to figure out that we can map the GPIO status to the status
of Timer\_2. So as the timer reaches a set value, the GPIO pin will
toggle - entirely independently of the CPU.

I actually proved the CPU does nothing by accidentally writing some
buggy code that set the CPU to a hard fault (crash), but the LED kept on
blinking.

#### Some information on Timer\_2

The timer is clocked from the main system clock, I think on the Nucleo
board that this is 84MHz, but I may be wrong - I'm still learning. The
timer has a programmable pre-scaler between it and the main clock, this
lets us clock the timer at a more manageable rate (we can slow it down)
if the full clock rate is too fast.

When enabled, the timer counts up from zero until it reaches its maximum
value (2<sup>16</sup>) then it rolls over back to zero. We can set an
auto-reload value that will instead be the value the timer rolls over at
when reached, this allows more flexibility in how we use the timer.

There are several possible ways to detect when the timer overflows or
auto reloads, the timer will set a flag we can poll for in code, or it
can issue an interrupt, or as in this case, we can have it directly
interact with another peripheral in the STM32F401 device.

In this example, we set up a hardware comparator to compare the current
value of the timer and a pre-set value, on a match this toggles the GPIO
pin. The timer is set to auto-reload at the same value as the
comparator, but this isn't a requirement. The only requirement is that
the auto-reload value is greater or equal to the comparison value (if it
isn't, the comparison will ever be met, as the timer will never reach
the comparison value).

#### The code

The first thing to do is up the GPIO port and enables clocks to the GPIO
peripheral and the timer.

        RCC->AHB1RSTR |= RCC_AHB1RSTR_GPIOARST;              // Reset GPIOA
        RCC->AHB1RSTR = 0;                                                      // Exit reset state
        RCC->AHB1ENR |= RCC_AHB1ENR_GPIOAEN;                   // Enable GPIOA clock
        RCC->APB1ENR |= RCC_APB1ENR_TIM2EN;                      // Enable Timer Clock

Then we can set up the Alternate Function of the GPIO pin, there didn't
seem to be any pre-calculated values in the header file, so I had to hit
the datasheet for the correct value to set the AFR\[0\] register. If you
change GPIO pins, this value will need re-calculating.

        GPIOA->MODER |= GPIO_MODER_MODER5_1;                 // Set GPIOA.5 to Alternate function (Took some digging to find this!)
        GPIOA->AFR[0]|= 0x100000;                            // Set Alternate function AF01 on pin 5 (defines seem to be missing?)
