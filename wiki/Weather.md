---
title: Weather
permalink: wiki/Weather/
layout: wiki
---

Some weather graphs.
====================

This page is more for my own general interest then anything else, it's
just a collection of weather data from
[sensors](/wiki/RaspberryPI_Multiple_DS1820 "wikilink") I've installed at my
home. I've added a DT11 humidity sensor and a BMP085 pressure sensor to
the data collection system. They are all connected directly to the rPI's
GPIO pins for now, but I'll soon move them over to a dedicated
micro-controller, so as to reduce the load on the rPI (bit-banging
1-wire and the strange encoding the DHT11 sensor uses locks up the rPI
at times).

Hourly
======

<html>
<center>
<img src="http://webshed.org/box/inside-outside.png">

</center>
</html>
<html>
<center>
<img src="http://webshed.org/box/humidity.png">

</center>
</html>
<html>
<center>
<img src="http://webshed.org/box/pressure.png">

</center>
</html>

