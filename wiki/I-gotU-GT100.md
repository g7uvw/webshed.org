---
title: I-gotU-GT100
permalink: wiki/I-gotU-GT100/
layout: wiki
tags:
 - Projects
 - Computer
---

i-gotU GT100 GPS Travel Tracker
-------------------------------

**UPDATE 2011:** I have not had more time to work on this project, but
there is an OpenSource client for talking to the i-gotU GT100 available
from [igotu2gpx](https://launchpad.net/igotu2gpx)

The i-gotU GT100 Travel Tracker is a tiny (47 x 29 x 12 mm) GPS receiver
intended as a waypoint logger for geotagging photographs. It can also
function as a GPS receiver for use with standard mapping software on a
PC.

Only windows software is supplied with the device, plugging the GT100
into an OS X or linux machine does not create a tty entry in /dev for
pseudo-rs232 communication. Connecting to a windows machine and dumping
the first 7 frames on the USB bus produces the following, after which
the device will start outputting NEMA data when opened as a com-port on
windows.

------------------------------------------------------------------------

**We query the device and get the 0x12 byte Device Descriptor.**

    ++PACKET 1++
    URB Header (length: 80)
    SequenceNumber: 1
    Function: 000b (GET_DESCRIPTOR_FROM_DEVICE)
    CONTROL_TRANSFER    12 01 01 01 00 00 00 08 0x00000000
    URB Header (length: 80)
    SequenceNumber: 1
    Function: 0008 (CONTROL_TRANSFER)
    PipeHandle: 81b68fb4

    SetupPacket:
    0000: 80 06 00 01 00 00 12 00                           //command sent to device

    TransferBuffer: 0x00000012 (18) length                  //18 bytes returned by device
    0000: 12 01 01 01 00 00 00 08 f7 0d 00 09 00 00 01 02 
    0010: 00 01 
        bLength            : 0x12 (18)                      //data length
        bDescriptorType    : 0x01 (1)                       
        bcdUSB             : 0x0101 (257)
        bDeviceClass       : 0x00 (0)
        bDeviceSubClass    : 0x00 (0)                  //composite device
        bDeviceProtocol    : 0x00 (0)
        bMaxPacketSize0    : 0x08 (8)
        idVendor           : 0x0df7 (3575)                  //Mobile Action Technology
        idProduct          : 0x0900 (2304)                  //GT-100
        bcdDevice          : 0x0000 (0)
        iManufacturer      : 0x01 (1)
        iProduct           : 0x02 (2)
        iSerialNumber      : 0x00 (0)
        bNumConfigurations : 0x01 (1)                       //one config 

What should happen next is that the host requests 9 bytes of the
Configuration Descriptor to determine its size. What actually happens is
this:

    '''Another 4 bytes are requested & returned'''
        ++PACKET 2++
    URB Header (length: 80)
    SequenceNumber: 2
    Function: 000b (GET_DESCRIPTOR_FROM_DEVICE)
    CONTROL_TRANSFER    1c 03 4d 00 0x00000000
    URB Header (length: 80)
    SequenceNumber: 2
    Function: 0008 (CONTROL_TRANSFER)
    PipeHandle: 81b68fb4

    SetupPacket:
    0000: 80 06 01 03 09 04 04 00                           //cmd to device


    TransferBuffer: 0x00000004 (4) length                   //4 bytes returned
    0000: 1c 03 4d 00                                       //not decoded yet

    '''Another 28 bytes - containing the vendor name are returned'''
        ++PACKET 3++
    URB Header (length: 80)
    SequenceNumber: 3
    Function: 000b (GET_DESCRIPTOR_FROM_DEVICE)
    CONTROL_TRANSFER    1c 03 4d 00 6f 00 62 00 0x00000000
    URB Header (length: 80)
    SequenceNumber: 3
    Function: 0008 (CONTROL_TRANSFER)
    PipeHandle: 81b68fb4

    SetupPacket:
    0000: 80 06 01 03 09 04 1c 00                           //cmd to device

    TransferBuffer: 0x0000001c (28) length                  //28 bytes returned

     1c 03 4d 00 6f 00 62 00 "..M.o.b."                     //vendor name
     69 00 6c 00 65 00 20 00 "i.l.e. ."
     41 00 63 00 74 00 69 00 "A.c.t.i."
     6f 00 6e 00             "o.n."


    '''Another 4 bytes '''
        ++PACKET 4++    
    URB Header (length: 80)
    SequenceNumber: 4
    Function: 000b (GET_DESCRIPTOR_FROM_DEVICE)
    CONTROL_TRANSFER    18 03 47 00 0x00000000
    URB Header (length: 80)
    SequenceNumber: 4
    Function: 0008 (CONTROL_TRANSFER)
    PipeHandle: 81b68fb4

    SetupPacket:
    0000: 80 06 02 03 09 04 04 00                           //cmd to device

    TransferBuffer: 0x00000004 (4) length                   //4 bytes returned
    0000: 18 03 47 00                       

    '''24 more bytes with the device name'''
        ++PACKET 5++
    URB Header (length: 80)
    SequenceNumber: 5
    Function: 000b (GET_DESCRIPTOR_FROM_DEVICE)
    CONTROL_TRANSFER    18 03 47 00 54 00 31 00 0x00000000
    URB Header (length: 80)
    SequenceNumber: 5
    Function: 0008 (CONTROL_TRANSFER)
    PipeHandle: 81b68fb4

    SetupPacket:
    0000: 80 06 02 03 09 04 18 00                           //cmd to device

    TransferBuffer: 0x00000018 (24) length                  //24 bytes returned

     18 03 47 00 54 00 31 00 "..G.T.1."                     //device name
     30 00 30 00 2f 00 47 00 "0.0./.G."
     54 00 32 00 30 00 30 00 "T.2.0.0."

------------------------------------------------------------------------

**Now we request the Configuration Descriptor**

    ++PACKET 6++
    URB Header (length: 80)
    SequenceNumber: 6
    Function: 000b (GET_DESCRIPTOR_FROM_DEVICE)
    CONTROL_TRANSFER    09 02 19 00 01 01 03 80 0x00000000
    URB Header (length: 80)
    SequenceNumber: 6
    Function: 0008 (CONTROL_TRANSFER)
    PipeHandle: 81b68fb4

    SetupPacket:
    0000: 80 06 00 02 00 00 00 02 
    bRequest: 06  
      GET_DESCRIPTOR
    Descriptor Type: 0x0002                             //get device configurations
      CONFIGURATION

                                                        //25 byte config table
    TransferBuffer: 0x00000019 (25) length              //from device    
    0000: 09 02 19 00 01 01 03 80 32 09 04 00 00 01 ff 00 
    0010: 00 00 07 05 81 03 10 00 01 
        bLength            : 0x09 (9)
        bDescriptorType    : 0x02 (2)                   //configuration
        wTotalLength       : 0x0019 (25)
        bNumInterfaces     : 0x01 (1)                   //1 interface
        bConfigurationValue: 0x01 (1)                   //1 config type
        iConfiguration     : 0x03 (3)
        bmAttributes       : 0x80 (128)
        MaxPower           : 0x32 (50)                  //100mA

**Finally we select a configuration**

    ++PACKET 7++        
    URB Header (length: 60)
    SequenceNumber: 7
    Function: 0000 (SELECT_CONFIGURATION)
    Configuration Descriptor:                           //from config table just
    bLength: 9 (0x09)                                   //read by previous request
    bDescriptorType: 2 (0x02)
    wTotalLength: 25 (0x0019)
    bNumInterfaces: 1 (0x01)
    bConfigurationValue: 1 (0x01)
    iConfiguration: 3 (0x03)
    bmAttributes: 128 (0x80)
      0x80: Bus Powered
    MaxPower: 50 (0x32)
      (in 2 mA units, therefore 100 mA power consumption)

    Number of interfaces: 1
    Interface[0]:
      Length: 0x0024
      InterfaceNumber: 0x00
      AlternateSetting: 0x00
      Class             = 0x04        //*shrug* not defined at www.usb.org/developers/defined_class
      SubClass          = 0x79        //ditto
      Protocol          = 0x43        //ditto
      InterfaceHandle   = 0x00000001
      NumberOfPipes     = 0x00000001
      Pipe[0]:
        MaximumPacketSize = 0x0001
        EndpointAddress   = 0x00
        Interval          = 0x00
        PipeType          = 0x25e3904       //      !!! INVALID !!!
        PipeHandle        = 0x00000200
        MaxTransferSize   = 0x00001000
        PipeFlags         = 0x00
        
        //reply from device
    SELECT_CONFIGURATION        0x00000000
    URB Header (length: 60)
    SequenceNumber: 7
    Function: 0000 (SELECT_CONFIGURATION)
    Configuration Descriptor:
    bLength: 9 (0x09)
    bDescriptorType: 2 (0x02)
    wTotalLength: 25 (0x0019)
    bNumInterfaces: 1 (0x01)
    bConfigurationValue: 1 (0x01)
    iConfiguration: 3 (0x03)
    bmAttributes: 128 (0x80)
      0x80: Bus Powered
    MaxPower: 50 (0x32)
      (in 2 mA units, therefore 100 mA power consumption)

    Number of interfaces: 1
    Interface[0]:
      Length: 0x0024
      InterfaceNumber: 0x00
      AlternateSetting: 0x00
      Class             = 0xff
      SubClass          = 0x00
      Protocol          = 0x00
      InterfaceHandle   = 0x81ba1228
      NumberOfPipes     = 0x00000001
      Pipe[0]:
        MaximumPacketSize = 0x0010
        EndpointAddress   = 0x81
        Interval          = 0x01
        PipeType          = 0x03
          UsbdPipeTypeInterrupt
        PipeHandle        = 0x81ba1240
        MaxTransferSize   = 0x00001000
        PipeFlags         = 0x00
