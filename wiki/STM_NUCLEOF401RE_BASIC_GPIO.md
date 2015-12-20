---
title: STM NUCLEOF401RE BASIC GPIO
permalink: wiki/STM_NUCLEOF401RE_BASIC_GPIO/
layout: wiki
---

### A basic guide to how I got an LED to blink on the ST Nucleo F401RE board without using MBED.

Step One - Install [PlatformIO](http://platformio.org/#!/) - the website
will have the latest details about how to do this. PlatformIO is a
convenient way of installing both a tool-chain and the
[CMSIS](http://www.arm.com/products/processors/cortex-m/cortex-microcontroller-software-interface-standard.php)
framework I want to use.

The GPIO pins on the
[STMF401RET6](http://www.st.com/web/catalog/mmc/FM141/SC1169/SS1577/LN1810/PF258797)
device are arranged into three channels A to C, each with a clock entry
on the AHB1 bus. The clock must be enabled for each channel you wish to
use.

On the ST Nucleo F401RE development board, a single LED and resistor are
connected to pin 5 of GPIO channel A. To make it blink on and off (the
“Embedded Hello World”) we need to:

-   set up the GPIO port
-   enable the clock on the GPIO channel
-   set the LED pin to be an OUTPUT (default is INPUT)
-   toggle the bit on the GPIO channel corresponding to the LED

Strictly speaking, resetting the GPIO isn't required because the GPIO
will be in a reset state when the STMF401RET6 is powered on, however it
is good practice to make sure things are in a known state before using
them. The following code sets the bit in the AHB1 bus reset register
that corresponds to GPIO channel A, this issues a reset signal to GPIO
channel A. The next line brings us out of reset.

    RCC->AHB1RSTR |= RCC_AHB1RSTR_GPIOARST;    // Reset GPIOA 
    RCC->AHB1RSTR = 0;                                            // Exit reset state

Following this we can enable the clock to the GPIO channel. Again, we
just set a bit in the appropriate register to enable the clock across
the AHB1 bus.

    RCC->AHB1ENR |= RCC_AHB1ENR_GPIOAEN;       // Enable GPIOA clock

The GPIO pin the LED is connected to is now set to OUTPUT (the default
setting is an INPUT, this needs to be overridden)

    GPIOA->MODER |= GPIO_MODER_MODER5_0;       // Enable Output on A5 (LED2 on Nucleo F401RE board)

With all that done we can get to the fun part of actually blinking the
LED. We'll set in an infinite loop and toggle the GPIO bit connected to
the LED, a delay will be added to slow things down, so they are visible.
ODR is the OutputDataRegister on the GPIOA channel.

    while (1)
        {
            GPIOA->ODR ^= (1 << (5));   // toggle LED
            ms_delay(800);
        }

#### Wrapping this up into a full program

I stole the delay routine from the PlatformIO example code, everything
is is from the data sheet for the STMF401RET6 device.

    #include "stm32f4xx.h"

    void ms_delay (int ms)
    {
      while (ms-- > 0)
        {
          volatile int x = 500;
          while (x-- > 0)
        __asm ("nop");
        }
    }
                                                                                          
    int main (void)
    {
        //RCC->AHB1RSTR |= RCC_AHB1RSTR_GPIOARST;    // Reset GPIOA 
        //RCC->AHB1RSTR = 0;                         // Exit reset state
        
        RCC->AHB1ENR |= RCC_AHB1ENR_GPIOAEN;       // Enable GPIOA clock
        GPIOA->MODER |= GPIO_MODER_MODER5_0;       // Enable Output on A5 (LED2 on Nucleo F401RE board)
        
        while (1)
        {
            GPIOA->ODR ^= (1 << (5));   // toggle LED pin
            ms_delay(800);

        }

      return 0;
    }

You can checkout a copy of the whole project from my [GitHub
repository](https://github.com/g7uvw/NUCLEOF401RE_Blink)
