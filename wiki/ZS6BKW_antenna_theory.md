---
title: ZS6BKW antenna theory
permalink: wiki/ZS6BKW_antenna_theory/
layout: wiki
---

Introduction
------------

Wire antennas with open wire feeder matching stubs have interested me
for a number of years; from the ubiquitous G5RV and the optimized ZS6BKW
designs, to doublets and similar antennas, the wire feeder is an
integral part of the design. The feeder forms a transmission line
transformer, matching the impedance of the antenna's feed point to that
of the RF source.

The ZS6BKW multi-band antenna
-----------------------------

The ZS6BKW is the multiband antenna I use at home and at the Kelvedon
Hatch secret nuclear bunker special even station - GB0SNB, so it is the
one I will investigate a the operation of. The antenna is constructed
from 27.5m of wire (2 x 13.75 elements) fed with either 13.3m of 300 Ohm
or 12.2m of 450 Ohm open wire feeder. Good matching to a 50 Ohm RF
source (SWR &lt; 2) is achieved on the following bands: 40, 20, 17, 12,
10, and 6 meters. Use of an ATU allows 80m and 15m to be matched too.
How does a short length of open wire feeder manage this? Read on.

Mathematics
-----------

Antenna design and analysis really requires mathematics, you can try to
short cut the maths by experimentation, but you're never going to really
develop anything new or any understanding of what's going on without at
least a little maths. The Telegrapher's Equations describe the behaviour
of voltages and current on a transmission line as a function of time and
distance from the source (for a lossless line)

$$\\frac{\\partial}{\\partial x} V(x,t) =
-L \\frac{\\partial}{\\partial t} I(x,t)$$

$$\\frac{\\partial}{\\partial x} I(x,t) =
-C \\frac{\\partial}{\\partial t} V(x,t)$$

It can be shown (but I'm not doing it here) that in the case of a
sinusoidal signal on a transmission line with characteristic impedance
*Z*<sub>0</sub> terminated with a load impedance *Z*<sub>*L*</sub>, the
input impedance *Z*<sub>*i**n*</sub> of the line is given by:

$$Z\_{in}(d) = \\frac{Z\_L cos\\beta d + jZ\_0 sin \\beta d}{Z\_0 cos \\beta d + j Z\_L sin \\beta d}$$
 where <math>\\beta = \\frac{2\\pi}{\\lambda}
