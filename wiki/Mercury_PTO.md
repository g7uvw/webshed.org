---
title: Mercury PTO
permalink: wiki/Mercury_PTO/
layout: wiki
tags:
 - Projects
 - Radio
 - Electronics
 - Experiments
---

A silly idea - a mercury tuned oscillator! (permeability tuned oscillator)
--------------------------------------------------------------------------

Some time ago I started to build Steve Webber's
[MMR40](http://kd1jv.qrpradio.com/ARRLHBC/ARRL_MMR40.html) radio, which
uses a PTO (permeability tuned oscillator). I got the PTO working and
then thought about ways to improve it before I finished building the
rig.

At the time I had access to some very finely graduated syringes and I
pondered a plunger type tuning arrangement for the PTO. The frequency
could then be read off the graduations of the syringe.

I wound a coil on a cheap plastic 5ml syringe from the lab and measured
the inductance with the syringe empty and with it full of water,
saturated NaCl and saturated CuSO4 solutions. No significant change in
inductance was measured, but I made a note to try mercury should I me
able to obtain some. Being a metal, mercury should have some noticeable
effect and being liquid at normal temperatures it will work nicely in a
syringe.

I finally obtained enough mercury from broken thermometers and relays to
try out the idea of a mercury PTO.

------------------------------------------------------------------------

### The Coil

![](Hg_pto_coil.jpg "fig:Hg_pto_coil.jpg") The coil used in all these
experiments is 42 turns on a 15mm OD polythene syringe body. There is
nothing “magic” about these values, the number of turns is just enough
to fill half the available space, leaving some clear viewing space to
see how full the syringe is.

------------------------------------------------------------------------

### The Oscillator

A recent article by [George Dobbs, G3RJV](http://www.g3rjv.org.uk/)
introduced me to the Franklin Oscillator using a pair of J-FETs in
astable coupling with a tuned circuit to set the oscillation frequency.
It seems a fool proof design, even with my oscillation killing skills I
managed to get it to work first try. ![](Franklin-osc.png "fig:")

------------------------------------------------------------------------

### Results

With the syringe empty, the oscillator ran at 4.886 MHz and its
inductance was 11.8 uH. Half filling the syringe and keeping the mercury
below the coil raised the oscillation frequency to 4.892 MHz. With just
enough mercury to completely fill the volume of the coil, the
oscillation frequency increased to 7.565 MHz.

The syringes I used didn't allow for much in the way of fine control, so
I don't have any repeatable data for fractional fillings of the coil
volume, and hence measurements of tuning linearity.

### Measurements of Q

To assess the effect mercury has on the Q of the coil (a question asked
by Alan Yates, [VK2ZAY](http://vk2zay.net) on twitter), I resonated the
coil both empty and mercury filled with an 11pF ceramic capacitor (bench
sweeping) and swept the circuit with my mRS miniVNA.

~~With the coil empty, the circuit formed a notch filter at ~14 MHz with
a 3dB bandwidth of 1363.3 kHz and a Q of 10. Filling the coil with
mercury raised the centre frequency of the filter to 20.4 MHz and
reduced the bandwidth to 390 kHz improving the Q to 22.~~

I was unhappy with my initial measurements of Q for the coil and Alan
suggested that the values I obtained were rather low. To improve the
data, I removed two shorted turns from the ends of the coil and used a
high quality 390 pF polystyrene RF capacitor instead of a crappy old
ceramic. I also experimented with partial fills of mercury to see how
the system behaves as more mercury is introduced to the coil. The
results are shown in the graph: the mercury lowers the Q of the coil,
decreases the insertion loss of the filter and moves the centre
frequency of the filter higher.

![](Hg_PTO_Q.png "fig:Hg_PTO_Q.png")  
![](Hg_pto.jpg "fig:Hg_pto.jpg")
