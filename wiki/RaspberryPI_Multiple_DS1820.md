---
title: RaspberryPI Multiple DS1820
permalink: wiki/RaspberryPI_Multiple_DS1820/
layout: wiki
---

Multiple DS18x20 1-wire sensors on the Raspberry PI  
=====================================================

In a previous article I showed how to use a 1-wire tempreature sensor
with the Raspberry PI with minimal interface requirements. The nice
thing with 1-wire sensors in that multiple devices can share the same
bus. You just wire the sensors in parallel - all GND pins tired
together, all DQ pins tied together and all VCC pins (if you use them)
tired together.  
The perl code developed for the previous article can only access a
single sensor. To read multiple sensors we need to fist get the device
IDs of all sensors on the bus, then read each sensor's data.  
On the rPI using the w1 kernel drivers, the file
“/sys/bus/w1/devices/w1\_bus\_master1/w1\_master\_slaves” contains a
list of all the device IDs detected on the 1-wire bus. We can read this
to get a list of IDs to iterate over, requesting data from each of
them.  

    #!/usr/bin/perl
    use warnings;
    &amp;check_modules;
    &amp;get_device_IDs;


    foreach $device (@deviceIDs)
    {
        $reading = &amp;read_device($device);
            if ($reading&nbsp;!= "9999")
        {
            push(@temp_readings,$reading);
        }
    }

    #update the database
    `/usr/bin/rrdtool update  /home/pi/temperature/multirPItemp.rrd N:$temp_readings[0]:$temp_readings[1]`;

    print "Temp 1 = $temp_readings[0]    Temp 2 = $temp_readings[1]\n";



    sub check_modules
    {
       $mods = `cat /proc/modules`;
    if ($mods =~ /w1_gpio/ &amp;&amp; $mods =~ /w1_therm/)
    {
     print "w1 modules already loaded \n";
    }
    else 
    {
    print "loading w1 modules \n";
        `sudo modprobe w1-gpio`;
        `sudo modprobe w1-therm`;
    } 
    }


    sub get_device_IDs
    {
    # The Hex IDs off all detected 1-wire devices on the bus are stored in the file
    # "w1_master_slaves"    

    # open file
    open(FILE, "/sys/bus/w1/devices/w1_bus_master1/w1_master_slaves") or die("Unable to open file");
     
    # read file into an array
     @deviceIDs = &lt;FILE&gt;;
     
     # close file 
     close(FILE);
    }

    sub read_device
    {
        #takes one parameter - a device ID
        #returns the temperature if we have something like valid conditions
        #else we return "9999" for undefined
        
        $readcommand = "cat /sys/bus/w1/devices/".$_[0]."/w1_slave 2&gt;&amp;1";
        $readcommand =~ s/\R//g;
        $sensor_temp = `$readcommand`;

        if ($sensor_temp&nbsp;!~ /No such file or directory/)
        {
            if ($sensor_temp&nbsp;!~ /NO/)
            {
               $sensor_temp =~ /t=(\d+)/i;
               $temperature = (($1/1000));
            }
            else
            {
                $ret = "9999";
            }
        }
        else
        {
            $ret = "9999"
        }
    }

This perl code writes data to a RRD database with two sensors - it
should be pretty straightforward to extend this to any number of
sensors.  
The RRD database is created with this script  

    #!/bin/bash
    rrdtool create multirPItemp.rrd  --step 300 \
    DS:in_temp:GAUGE:600:-30:50 \
    DS:out_temp:GAUGE:600:-30:50 \
    RRA:AVERAGE:0.5:1:12 \
    RRA:AVERAGE:0.5:1:288 \
    RRA:AVERAGE:0.5:12:168 \
    RRA:AVERAGE:0.5:12:720 \
    RRA:AVERAGE:0.5:288:365

  

