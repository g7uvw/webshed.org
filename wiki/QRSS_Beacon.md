---
title: QRSS Beacon
permalink: wiki/QRSS_Beacon/
layout: wiki
---

G7UVW QRSS Beacon
-----------------

After seeing lots of information and designs and grabbed QRSS signals
I've decided to have ago at getting a beacon on the air for some tests.
The 10.140 Mhz xtal was supplied by Chris G8OCV and the xtal oven was a
rally find a couple of years ago.

**Oscillator**  
The oscillator is pretty much a straight clone of [Hans Summers' junk
box QRSS
beacon](http://www.hanssummers.com/radio/qrssjb/phase1/index.htm) but
with a smaller inductor in series with the xtal and a proper AM radio
varicap (taken from a dead radio) instead of an LED for frequency
shifting.

The whole thing is built on a small scrap of veroboard so that it will
fit inside the aluminum box of my xtal oven. I decided to oven the whole
oscillator instead of just the xtal for better frequency stability.

<img src="Qrss-osc-board.jpg" title="fig:Oscillator - click for larger" alt="Oscillator - click for larger" width="300" />
<img src="Qrss-osc-board-oven.jpg" title="fig:Oscillator - click for larger" alt="Oscillator - click for larger" width="300" />

The white stuff at the bottom of the oven block is white-tak just to
electrically insulate the board from the metal block and hold the board
fairly solidly.

**Oven**  
The xtal oven I'm using is an old Pye ovened frequency standard. I think
I paid about £3 for it at a radio rally. It wasn't a 10Mhz standard as
I'd hoped and a straight xtal swap didn't work with the existing
oscillator, so it was replaced with a 10Mhz rubidium standard from ebay.
After sitting unused for some time on the shelf, it has been re-purposed
for this project.

The oven itself is just a box fitted with two BD134 transistors as
heaters. There is a simple 741 based circuit that compares a thermistor
and a potentiometer and adjusts the transistor current to heat the box
and thermistor. Temperature control is pretty good after about a 20min
warm up.

**First ~~light~~ RF**  
Fed with 8 volts from the internal supply of the oven, the oscillator
starts up at 10.140240 Mhz and drifts downwards to 10.140105.3 as the
oven heats up. This final frequency holds stable to within 0.1 Hz for at
least 3 hours.  
<img src="Qrss-first-rf.jpg" title="fig:Oscillator signal - click for larger" alt="Oscillator signal - click for larger" width="300" />

The signal from the oscillator isn't a pure sinewave, so there is some
need for bandpass filtering to remove harmonics before this goes on air.
