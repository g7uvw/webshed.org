---
title: FUNcube-Dongle-Linux
permalink: wiki/FUNcube-Dongle-Linux/
layout: wiki
tags:
 - Projects
 - Radio
 - Funcube_Dongle
---

Getting a FUNcube Dongle running on Linux
-----------------------------------------

<img src="FUNcube-Dongle.jpg" title="My FUNcube Dongle, with an SMA &lt;-&gt; BNC converter attached" alt="My FUNcube Dongle, with an SMA &lt;-&gt; BNC converter attached" width="350" />

This is how I got my FUNcube Dongle (FCD from here on in) working under
Linux on a Samsung N130 Netbook PC using Quisk SDR software. The Linux
distro installed is Ubuntu 11.04, but instructions should be very
similar for any recent Linux distro.

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
and check this this can communicate with and control the FCD. I found I
had to start the qthid software with extra permissions to access USB,
this is easily achieved

`  $ sudo ./qthid`

<img src="Qthid.png" title="QTHID v3 Software running under Linux" alt="QTHID v3 Software running under Linux" width="750" />

  
As long as the software says “FCD is active” then all is well, skip to
the next step - setting up ALSA. If QTHID says “FCD not detected” but
the FCD is listed by the lsusb command above, then you may need to
update the firmware on your FCD. IN which case, get the older 2.2
version of the QTHID software (this can talk to older FCD firmwares) and
use it to upload the latest firmware from the main FCD website. The
following, contributed by **Robin Gape G8DQX** explains how to update
the FCD firmware.

> **Firmware update from 18b**  
> This procedure assumes that the procedures above have been performed,
> and that the FCD is recognised.
>
> -   Download the new firmware image (18f) from the FCD website, and
>     save the file export18f.bin in a known location (right click and
>     select “Save link as...” or similar in the browser)
> -   Start QTHID (version 2.1 or 2.2)
>
> <img src="GUI_Ubuntu_QTHID.png" title="QTHID v2.2 Software required to upgrade older FCD firmwares" alt="QTHID v2.2 Software required to upgrade older FCD firmwares" width="400" />
>
> -   Plug in the FCD whose firmware is to be updated
> -   Note that the presence of the FCD is acknowledged by QTHID
>
> <span style="color:red">Do not proceed if the step above is not
> successful</span>
>
> -   Click the “Switch to bootloader” button
> -   The presence icon will change colour to orange, with the text “FCD
>     bootloader”
> -   Click the “Update firmware” button, and select the firmware image
>     file saved above
> -   The firmware will now be updated.
> -   This takes a long time, several minutes.
>
> <span style="color:red">Be very patient, even if nothing appears to be
> happening</span>
>
> -   Eventually a dialogue box appears with the text “Firmware
>     successfully written”
> -   Click on the OK button
> -   And switch back to normal mode with the “Switch to application”
>     button.
> -   When this fails, unplug and replug the FCD
> -   QTHID will now show “FCD is active”
> -   QTHID version 3 may now be installed & used to control all the
>     parameters of the FCD

Next check how ALSA (Advanced Linux Sound Architecture) is identifying
the FCD (The FCD pretends to be a soundcard)

` $ cat /proc/asound/cards`  
` 0 [Intel          ]: HDA-Intel - HDA-Intel`  
`                         HDA-Intel at 0xf044000 irq 43`  
` 1 [default       ]: USB-Audio - FUNcube Dongle V1.0 at usb-0000:00:id.3.1`

This means that ALSA is calling the built in soundcard “hw:0” and the
FCD “hw:1”, this is all the information we need to configure Quisk.

Get and install Quisk as documented
[here](http://james.ahlstrom.name/quisk/). Quisk requires a
configuration file to enable it to communicated with the FCD, this is
mine:

`  #Filename quisk_funcube.py`  
`  sample_rate = 96000                  #ADC Hardware sample rate`  
`  name_of_sound_capt = `“`hw:1`”`    #Determined from ALSA`  
`  name_of_sound_play = `“`hw:0`”`    #Determined from ALSA`  
`  channel_i = 0`  
`  channel_q = 1`

Start quisk

` $ ./quisk.py -c quisk_funcube.py`

<img src="QuiskSDR.png" title="QuiskSDR.png" alt="QuiskSDR.png" width="750" />

  
The Quisk software isn't fully polished or able to control the FCD
directly, so you need to run it in conjunction with QTHID to control the
FCD. Set Quisk frequency to a nice round number and then it's easy to
calculate the offsets needed for tuning with QTHID.
