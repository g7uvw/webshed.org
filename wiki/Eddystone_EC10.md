---
title: Eddystone EC10
permalink: wiki/Eddystone_EC10/
layout: wiki
---

Repair of an Eddystone Model EC10
---------------------------------

The EC10 is one of Eddystone's very early solid state communications
receivers. Using Germanium transistors throughout, it covers 550 kHz to
30 MHz in five ranges. The radio is a single conversion superhet, with a
BFO for SSB and CW reception and a switchable audio filter for CW. I
have a mark-2 version, essentially the same as the original, with the
addition of a fine tuning control.

**Basic Specs:**  
Frequency coverage - 550 kHz to 30 MHz (in five bands)  
Intermediate Frequency - 720 kHz  
Sensitivity - &lt;5uV for 15dB s/n ratio  
Image rejection - 50dB at 2.0 MHz, 20dB at 20 MHz  

------------------------------------------------------------------------

The EC10 I now own had been 'previously enjoyed'. At some point in its
history the audio PA had died, and a Velleman kit amp had been installed
instead, this too was dead when I took possession.

**Audio PA**:  
The first obvious fault was a large char-mark on the PCB in the audio PA
stage. Inspection of the circuit diagram and comparison with another
EC10 showed that the immolated part was R48 - a 5 Ohm 3 Watt resistor in
the emitters of the PA transistors. Not having a suitable part to hand,
this resistor was replaced by four series-parallel 4.7 Ohm 1/2 Watt
parts.

**IF Strip**:  
With the audio PA working, but no station heard, it was time to work
back from the PA. With the audio filter and BFO both off, next back from
the PA is the IF strip - specifically TR5 and TR4, both OC171 devices. A
preemptive snip of the case/shield wire on both OC171s (in these parts,
the Collector or Emitter often shorts to the case and down to ground)
proved ineffective. A quick check of the voltage around TR5 showed that
its emitter and collector were shorted. The OC171 was stolen from the
BFO to replace TR5 - success! The radio began to receive.

**The BFO**:  
I now had a working radio with a dead BFO - somewhat limiting if you
want to listen to more than AM stations. An attempt to use an OC71 (more
of an audio transistor than anything) in place of the purloined OC171
failed. Zero oscillation. Having no other germanium transistors to hand,
I dug into the spares bin for a PNP Silicon transistor instead. The
2N3906 is a pretty general purpose part, sibling the the 2n3904 (NPN)
beloved of QRP constructors. A straight forward swap of the OC71 for the
2N3906 did not work. Silicon transistors need about 0.6 Volts to turn on
compared to germanium's 0.3-0.4 Volts. The easiest fudge for this is to
replace R35, a 47K Ohm part, by a 22K Ohm resistor.  
----  
Next up will be to re-align the set and perhaps switch out all the
germanium transistors in the RF and IF sections for their silicon
counterparts.
