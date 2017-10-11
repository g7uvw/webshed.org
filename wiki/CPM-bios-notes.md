---
title: CPM-bios-notes
permalink: wiki/CPM-bios-notes/
layout: wiki
---

Some notes on the +3 Spectrum CP/M BIOS

There are two jump tables in RAM, the first resides at 0xf968. It mainly
points to the later table. <code>

    f968: c3 03 fa          jp $fa03    ;jump to WBOOT

    f96b: c3 83 f5          jp $f583         

    f96e: c3 06 fa          jp $fa06    ;jump to CONST

    f971: c3 83 f5          jp $f583           

    f974: c3 09 fa          jp $fa09    ;jump to CONIN        

    f977: c3 83 f5          jp $f583           

    f97a: c3 0c fa          jp $fa0c    ;jumps to CONOUT

    f97d: c3 83 f5          jp $f583           

    f980: c3 0f fa          jp $fa0f    ;jumps to LIST     

    f983: c3 83 f5          jp $f583           

</code> The second table starts at 0xfa00. CP/M 3 lables are in
brackets. Proven jumps have an asterisk. <code>

    fa00: c3 0f ba          jp $ba0f    ;(BOOT)

    fa03: c3 63 fa          jp $fa63    ;(WBOOT)

    fa06: c3 da fb          jp $fbda    ;(CONST)

    fa09: c3 f2 fb          jp $fbf2    ;(CONIN)

    fa0c: c3 bb fb          jp $fbbb    ;(CONOUT*)

    fa0f: c3 b6 fb          jp $fbb6    ;(LIST)

    fa12: c3 b1 fb          jp $fbb1    ;(PUNCH)

    fa15: c3 ed fb          jp $fbed    ;(READER)

    fa18: c3 7d bb          jp $bb7d    ;(HOME)

    fa1b: c3 fe fb          jp $fbfe    ;(SELDISK)

    fa1e: c3 80 bb          jp $bb80    ;(SETTRACK)

    fa21: c3 85 bb          jp $bb85    ;(SETSEC)

    fa24: c3 8a bb          jp $bb8a    ;(SETDMA)

    fa27: c3 03 fc          jp $fc03    ;(READ)

    fa2a: c3 08 fc          jp $fc08    ;(WRITE)

    fa2d: c3 c8 fb          jp $fbc8    ;(LISTST)

    fa30: c3 95 bb          jp $bb95    ;(SECTRAN)

    fa33: c3 cd fb          jp $fbcd    ;(CONOST)

    fa36: c3 d5 fb          jp $fbd5    ;(AUXIST)

    fa39: c3 c3 fb          jp $fbc3    ;(AUXOST)

    fa3c: c3 ad fb          jp $fbad    ;(DEVTBL)

    fa3f: c3 a8 fb          jp $fba8    ;(DEVINI)

    fa42: c3 fa fb          jp $fbfa    ;(DRVTBL)

    fa45: c3 97 bb          jp $bb97    ;(MULTI-IO)

    fa48: c3 06 bc          jp $bc06    ;(FLUSH)

    fa4b: c3 c2 fa          jp $fac2    ;(MOVE)

    fa4e: c3 b0 fb          jp $fbb0    ;(TIME)

    fa51: c3 0a fb          jp $fb0a    ;(SELMEM)

    fa54: c3 91 bb          jp $bb91    ;(SETBNK)

    fa57: c3 bd fa          jp $fabd    ;(XMOVE)

    fa5a: c3 2c fb          jp $fb2c    ;(USERF)

    fa5d: c3 00 00          jp $0000    ;(RESERVED1)

    fa60: c3 00 00          jp $0000    ;(RESERVED2)

</code> It can be seen that this table contains entries for all the
specified CP/M3 system calls. The reason for the first jump table is not
immediately clear - it is used by the system as a breakpoint on 0xf97a
will trigger when a character is to be written to the screen.
