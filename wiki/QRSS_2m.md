---
title: QRSS 2m
permalink: wiki/QRSS_2m/
layout: wiki
tags:
 - QRSS
---

144 MHz QRSS Beacon / MEPT
--------------------------

A rally purchase earlier in 2010 turned out to be a telemetry or data
transmitter on 160-ish MHz. The interesting part of the board was the
12x frequency multiplier and PA.

<img src="Tele-tx.jpg" title="Transmitter with RF shield off and 12 MHz crystal tacked on" alt="Transmitter with RF shield off and 12 MHz crystal tacked on" width="800" />

The RF section of the board has four connections to the digital section.
These include +5v for the oscillator and frequency multiplier, a second
+5v supply for the PA transistor, a modulation input and a signal to
switch on or off the first frequency tripper to kill the RF output
without stopping the oscillator. Ground is common to both sections of
the board.

<img src="RF-stage.jpg" title="RF stage of the transmitter. Comprised of an Oscillator, frequency multiplier stage and a PA " alt="RF stage of the transmitter. Comprised of an Oscillator, frequency multiplier stage and a PA " width="800" />

Conversion to 144 MHz was surprisingly easy; the oscillator crystal was
swapped for a 12 MHz computer grade part and wired in series with a
10-60pF trimmer cap to allow fine frequency adjustment. From then on it
was just a case of peaking the sections of the frequency multiplier and
adjusting the filter for lowest harmonic levels.

The initial power output was rather low (&lt;10mW into 50Ohm), so I took
advantage of the separate PA supply and increased the PA transistor
voltage to 8v; this brought the output up to a more respectable 100mW at
the expense of increased harmonics. Adjustment for minimum harmonics
brought the signal at 144 MHz down to 75mW with all but the 3rd
harmonics at least 30dB below this.

For initial tests, the carrier is just keyed on and off (5s on, 3s off)
with a 555 timer on the tripper control line.

<img src="144MEPT.jpg" title="The boxed 144MHz QRSS MEPT with 555 timer controller " alt="The boxed 144MHz QRSS MEPT with 555 timer controller " width="800" />

First Light
-----------

First on-air signal report was from M1GEO at the astounding distance of
2.5 miles.
<img src="M1geo1.jpg" title="fig:First copy by M1GEO " alt="First copy by M1GEO " width="617" />  
Later on, Colin G6AVK had a listen and captured my signal drifting a
little.
<img src="G6avk-2m.jpg" title="fig:First copy by G6AVK " alt="First copy by G6AVK " width="617" />  

Frequency stability modification
--------------------------------

Although crystals are usually considered pretty stable frequency
sources, there is a correlation of oscillation frequency with
temperature, a few degrees C temperature change can easily translate
into a hundred Hz or more of frequency shift; indeed, this is the basis
for some thermometers. In this application I'm multiplying up the
crystal frequency, so I also multiply up any shifts due to heating or
cooling of the crystal, 100 Hz shift at 12 MHz becomes 1.2 kHz at 144
MHz, which is way beyond the nominal 100Hz band claimed by QRSS. A means
of keeping the crystal at a constant temperature is required.  
A trawl around eBay brought up a heating clip for a crystal, this passes
a small amount of current though a Positive Temp Coefficient Thermistor,
causing self heating and a positive resistance change that limits the
current though the PTC, the result is a pretty stable temperature for a
given voltage across the PTC. At 12v, the heater draws approx 75mA and
settles down to about 65 degrees C.  
With the heater fitted the frequency drifts upwards quite dramatically
for about 60s until the heater settles down, then the frequency
stability is +/- 2Hz with the board sitting in the open on the bench.

<img src="Xtal-ptc-clip.jpg" title="fig: Murata BG330N Xtal heater" alt=" Murata BG330N Xtal heater" width="300" />
<img src="Xtal-ptc-clip-back.jpg" title="fig:Murata BG330N Xtal heater, showing PTC disc." alt="Murata BG330N Xtal heater, showing PTC disc." width="300" />
<img src="Xtal-ptc-clip-fitted.jpg" title="fig: The heater clip fitted to the crystal in the transmitter and wired into the supply voltage" alt=" The heater clip fitted to the crystal in the transmitter and wired into the supply voltage" width="300" />
