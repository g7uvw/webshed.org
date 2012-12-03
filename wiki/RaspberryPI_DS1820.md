---
title: RaspberryPI DS1820
permalink: wiki/RaspberryPI_DS1820/
layout: wiki
---

How to use the DS1820 / DS18s20 / DS18b20 with the Raspberry PI
---------------------------------------------------------------

So you've got a shiny new rPI and want to connect it to some sensors -
the 1-wire bus is a good way to do so. A bunch of devices can be
connected to just three pins on the rPI GPIO connector.

For this example I'm connecting a temperature sensor, a DS1820 (but the
configuration is identical for other parts in the same series: DS18s20 &
DS18b20). The DS1820 VDD and GND pins are connected to the rPI 3.3V and
GND pins respectively. The DQ pin is connected to GPIO-4, a 4k7 resistor
connected between DQ and VDD is required to enable the sensor.  
  
I built my breakout interface directly onto a plastic pin header for
testing purposes, but this is hardly the best way to do it.  
  
28 modprobe wire  
29 sudo modprobe wire  
31 sudo modprobe w1-therm  
55 modprobe w1-gpio  
56 sudo modprobe w1-gpio  
63 history |grep mod  
  
<img src="GPIOs.png" title="fig:GPIOs.png" alt="GPIOs.png" width="200" />
<img src="DS1820.png" title="fig:DS1820.png" alt="DS1820.png" width="200" />
