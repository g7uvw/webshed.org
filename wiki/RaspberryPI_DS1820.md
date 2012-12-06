---
title: RaspberryPI DS1820
permalink: wiki/RaspberryPI_DS1820/
layout: wiki
---

How to use DS18x20 1-wire temperature sensors with the Raspberry PI
-------------------------------------------------------------------

The Raspberry PI (rPI)has several different serial buses brought out on
its GPIO (General Purpose Input/Output) pins, including SPI and I2C,
however there is no 1-Wire interface. Luckily, in modern Linux Kernels
there is a driver module for bit-banging a 1-Wire interface on a single
GPIO pin. In recent Raspbian “wheezy” releases GPIO-4 is the pi used.
The rest of this article will describe how to use this module and
connect some 1-Wire temperature sensors to the rPI.  
I built my breakout interface directly onto a plastic pin header for
testing purposes, but this is hardly the best way to do it.  

------------------------------------------------------------------------

Hardware
--------

  
To interface a DS18x20 sensor to the rPI, the bare minimum hardware you
need is a single 4.7K resistor, the DS18x20 sensor and a plug that will
fit the rPI GPIO pins. The wiring diagram is show below, along with a
some photographs of the parts I soldered together for this experiment. I
used a now-obsolete DS1820 sensor, but modern Ds18s20 and DS18b20 will
work the same way (and offer more features and more precise temperature
readings).  
Software: The 1-Wire drivers are not loaded by default when the rPI
boots. You can load them with the following commands from a command
prompt sudo modprobe wire sudo modprobe w1-gpio sudo modprobe w1-therm

Connect the sensor hardware to the rPI and check it is detected by
seeing if a device is listed on the 1-Wire bus. pi@raspberrypi:~$ cat
/sys/bus/w1/devices/w1\_bus\_master1/w1\_master\_slave\_count 1 This
will print the number of sensors detected, 1 - for my hardware at the
moment. You can get the sensor ID (a hexadecimal string stored in ROM on
the sensor chip) by reading the w1\_master\_slaves file:
pi@raspberrypi:~$ cat
/sys/bus/w1/devices/w1\_bus\_master1/w1\_master\_slaves 10-00080234149b

A quick check for correct operation of the sensor is to “read” the
sensor file, you'll need the hex ID of the sensor from earlier commands
pi@raspberrypi:~$ cat /sys/bus/w1/devices/10-00080234149b/w1\_slave 37
00 4b 46 ff ff 07 10 1e : crc=1e YES 37 00 4b 46 ff ff 07 10 1e t=27312

The number after 't=' is the tempreature in mili-degrees Celcius.

Unfortunately, at the time of wiring this article (December 2012) the
very useful OWFS (OneWire File System) software is incompatible with the
w1-gpio kernal drivers, so you can't yet use the nice owfs tools to
explore and retrive data from sensors connected to the rPI this way.

<img src="GPIOs.png" title="fig:GPIOs.png" alt="GPIOs.png" width="200" />
<img src="DS1820.png" title="fig:DS1820.png" alt="DS1820.png" width="200" />
