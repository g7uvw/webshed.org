---
title: ESR meter
permalink: wiki/ESR_meter/
layout: wiki
---

An Equivalent Series Resistance Meter
-------------------------------------

Some time ago I was trying to repair a switched mode power supply (SMPS)
in my oscilloscope. I'd been quoted £800 for a new PSU, so trying to fix
it first was well worth my while. It's pretty common for electrolytic
capacitors to develop faults in an SMPS; specifically they develop an
higher than normal internal resistance. So while the capacitor may hold
a charge and measure as the correct capacitance, it will not behave
correctly in a filter or PSU circuit. The ESR of a large high voltage,
high capacity electrolytic capacitor should be fractions of an Ohm,
smaller capacitors have ESRs of a few Ohms typically. As the capacitor
degrades in use the ESR can easily climb to several hundred times the
normal value, while exhibiting no changes to voltage and capacitance
ratings

Suspecting the capacitors, and not having a way to measure their series
resistance, I set out to design and build an ESR meter. We can't measure
ESR with a normal DC Ohm meter, because the DC voltage would charge the
capacitor. An AC signal is used, so that it passes straight though the
capacitor with minimal reactive losses. As a ball-park figure, I decided
my ESR meter design would operate at around 25kHz, where a 100μF cap has
a reactance of about 60 mOhm (a 1μF cap has a reactance of about 6 Ohms
at this frequency). I also decided to keep the test voltage below about
0.6 V, so as not to forward bias any semiconductors in the circuit under
test. I want to be able to test capacitors in circuit where possible.
