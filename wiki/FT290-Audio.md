---
title: FT290-Audio
permalink: wiki/FT290-Audio/
layout: wiki
tags:
 - Projects
 - Radio
 - Electronics
 - Repair
---

Yaseu FT290R multimode 2m transceiver audio fix
-----------------------------------------------

A fairly common fault on the FT290R is that the speaker amplifier chip
dies, this is Q1027 in the schematic diagram, partnumber uPC575C2. The
radio service manual tells you to suspect this device if there are no
sounds coming from the speaker, it does not tell you how to test this
chip. The simplest test I use is to connect a small test audio amp to
pin 1 of the chip and turn the radio on. If all is well with the rest of
the radio then you should hear signals on the test amp. Connect the test
amp to pin 7 of the chip and if the chip is dead, you'll hear nothing.

I sourced a replacement uPC575C2 for my FT290R from ebay, however it
arrived dead. I decided to build a seperate amplifier module instead and
use that. A single TBA820m amplifier chip didn't provide enough gain, so
I added a single transistor preamp with a gain of about 10.

<img src="Ft290-audio-amp.png" title="Ft290-audio-amp.png" alt="Ft290-audio-amp.png" width="800" />

The decoupling capacitors, C8 and C9 are definitely needed, the TBA820m
runs incredibly hot without them, but doesn't seem to go into
oscillation.

The input signal and power lines are taken from the uPC575C2 chip pads.
The internal speaker of the radio was re-wired directly to the amplifier
module, losing the external speaker connection (for now). The amplifier
module sits in the battery bay, batteries not being used.
