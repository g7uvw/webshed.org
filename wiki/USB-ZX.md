---
title: USB-ZX
permalink: wiki/USB-ZX/
layout: wiki
---

What it is
----------

This is the working name of my Z80 hardware debugger targeting the
Sinclair Spectrum. In essence, this device is a multiface-alike with IO
directed to a PC via USB, rather than using the native IO (screen /
keyboard) of the spectrum.

The PC &gt; Spectrum interfacing is provided by a PIC18F4550
microcontroller, this interfaces to 16k of static RAM (can be mapped
into the spectrum memory map starting at address 0), 64k of SPI
interface EEPROM (non-pageable from Spectrum, used to store debuggers
and other code to download to the Spectrum), and an SPI IO expander
(Address lines go via this, data and control go direct from PIC).

The interface can generate NMI, directly read and write anywhere in the
Spectrum memory and IO space, and page in its own RAM to the lower 16k
of Spectrum memory. The hardware responds to two hardware ports from the
Spectrum side of things; 0x9F (old multiface address) pages the
interface in and out (in on read, out on write) and port 0x3F is used
for communication between interface and Spectrum.

How to use it for debugging
---------------------------

(Making this up as I go at the moment - I know what I want, just working
out how to make it happen)

The Z80 has no hardware breakpoints. We can use a software breakpoint
but using a 1 byte opcode that jumps to a known location. These would be
the RSTx opcodes then. On the Spectrum at the RSTx opcodes point into
ROM and all but RST0 are used for routine entry points. RST0 just points
to the reset routine and is essentially unused.

We shall steal RST0 for our breakpoint then. To set a BP we save the
current databyte at the BP address, and poke 0xC7 (RST0) in its place.
The hardware upon detecting an opcode fetch from address 0 then pages
out the Spectrum ROM and pages in the debugging ROM. Upon detecting an
opcode fetch outside the lower 16k of memory the Spectrum ROM is paged
back in.

This does mean that it is impossible to debug the Spectrum ROM (BPs
can't be set in ROM) and debugging the lower 16k in 64k RAM mode will
require some fancy hardware ticks that will not be in the first version
of the CPLD code.

How it works
------------

Upon startup the interface is prevent from paging in by a latch which
starts up unset, this latch must be set by the host PC for the interface
to begin operation.

**RST0**  
Upon an opcode fetch from address 0 (A-bus = 0 + M1 going low) pull
ROM\_CS low and page in RAM to address 0

**Port access**  
Upon a read from port 0x9F (A-bus = 0x9f + IORQ going low + RD going
low) pull ROM\_CS low and page in RAM to address 0  
Upon a write to port 0x9F (A-bus = 0x9f + IORQ going low + WR going low)
release ROM\_CS

~~**Memory access**  
If paged in and opcode fetch from far RAM (Address &gt;16k)
(A-bus=01xxxxxxxxxxxxxx + M1 going low) release ROM\_CS~~ Bad idea - too
inflexible.

**Direct memory access from host PC**  
TBD
