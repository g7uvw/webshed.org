---
title: FUNcube-Dongle-Linux
permalink: wiki/FUNcube-Dongle-Linux/
layout: wiki
---

Getting a FUNcube Dongle running on Linux
-----------------------------------------

<img src="FUNcube-Dongle.jpg" title="My FUNcube Dongle, with an SMA &lt;-&gt; BNC converter attached" alt="My FUNcube Dongle, with an SMA &lt;-&gt; BNC converter attached" width="350" />

This is how I got my FUNcube Dongle (FCD from here on in) working under
Linux on a Samsung N130 Netbook PC. The Linux distro installed is Ubuntu
11.04, but instructions should be very similar for any recent Linux
distro.

First of all insert the FCD into a USB port and determine that it is
detected:

` $ lsusb`  
` Bus 005 Device 003: ID 04d8:fb56 Microchip Technology, Inc.`

The FCD uses a Microchip PIC to handle USB connectivity, so the lsusb
command lists the FCD as a Microchip Technology, Inc. device. As long as
the hex identifier, 04d8:fb56, is listed correctly then the FCD is being
detected and not confused with some other device also connected to USB.

At this point it is a good idea to download the [latest
version](https://sourceforge.net/projects/qthid/files/) of the [QTHID
FCD control
software](http://www.oz9aec.net/index.php/funcube-dongle/qthid-fcd-controller)

Next check how ALSA (Advanced Linux Sound Architecture) is identifying
the FCD (The FCD pretends to be a soundcard)

` $ cat /proc/asound/cards`  
` 0 [Intel          ]: HDA-Intel - HDA-Intel`  
`                         HDA-Intel at 0xf044000 irq 43`  
` 1 [default       ]: USB-Audio - FUNcube Dongle V1.0 at usb-0000:00:id.3.1`

Basically this means that ALSA is calling the built in soundcard “hw:0”
and the FCD “hw:1”, this is all the information we need to