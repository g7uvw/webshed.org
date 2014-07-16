---
title: RaspberryPI Multiple DS1820
permalink: wiki/RaspberryPI_Multiple_DS1820/
layout: wiki
tags:
 - Code
---

Multiple DS18x20 1-wire sensors on the Raspberry PI
===================================================

*Edited 16/07/2014 to show how to use new version of the code"*

“'Edited 22/12/2013 to add a link to github”'' *Edited 1/1/2013 to fix
buggy code, etc*

In a [previous article](/wiki/RaspberryPI_DS1820 "wikilink") I showed how to
use a 1-wire temperature sensor with the Raspberry PI with minimal
interface requirements. The nice thing with 1-wire sensors in that
multiple devices can share the same bus. You just wire the sensors in
parallel - all GND pins tired together, all DQ pins tied together and
all VCC pins (if you use them) tired together.  
The perl code developed for the previous article can only access a
single sensor. To read multiple sensors we need to fist get the device
IDs of all sensors on the bus, then read each sensor's data.  
On the rPI using the w1 kernel drivers, the file
“**/sys/bus/w1/devices/w1\_bus\_master1/w1\_master\_slaves**” contains a
list of all the device IDs detected on the 1-wire bus. We can read this
to get a list of IDs to iterate over, requesting data from each of
them.  
There was a “feature” in the previous version of the code (now removed
from this page), it didn't keep track of the 1-Wire ID of the sensors
when it queried them for data, it just assumed the first one that
answered would always be the first one that answered - a poor assumption
that can lead to data from the wrong sensor being written to the wrong
database slots. The new code (July 2014) addresses this isse and now
keep track of the device IDs.

To use the new code, you first need to run *detect.pl* this produces a
plain text file called *sensors.conf*, you can edit this to give the
sensors nice names and to add any offset or calibration values you need.
You can change the calibration / offset values any time you like, but
the names are fixed once you've created the database.

    #SENSORS.CONF example
    # we read this file, and make a new fixed file with all the device IDs and fixed indexes. You can also put
    # calibration information into the sensors.conf file after detect.pl has created it.
    # sensors.conf format is:
    # 
    # index , calibration factor    , 1-Wire Device ID
    # 0             ,0.0                                    , 10-000802b67ffc
    # 1             ,0.0                                    , 10-000802b65cc2
    # 2             ,0.0                                    , 10-000802b685ca
    # 3             ,0.0                                    , 10-000802b6689e
    # 4             ,0.0                                    , 10-000802b68df3
    # 5             ,0.0                                    , 10-000802b67687

    # or you can use descriptive names for the device ID, but again you'll have to edit the sensors.conf file to
    # add them after detect.pl has created sensors.conf for you.
    #
    # index             , calibration factor , 1-Wire Device ID
    # inside            ,0.0                    ,10-000802b67ffc
    # the sun           ,100.123                ,10-000802b65cc2
    # deepspace         ,-273.1                 ,10-000802b685ca
    # earth_core        ,6000                   ,10-000802b6689e
    # outside           ,-1.2                   ,10-000802b68df3
    # spare             ,0.0                    ,10-000802b67687

Once you've got a nice looking sensors.conf, you need to run
“makedb.pl”. YOou can edit “makedb.pl” to change the RRDTool database
settings, but the default values I have in there (collect data every
5min, make a few averages, collect data for a year) should be fine to
get started with. My cron job for reading the senors and updating the
database looks like this

    */5 * * * * cd /home/pi/rPI-multiDS18x20/example && /home/pi/rPI-multiDS18x20/example/gettemp.pl 
    */6 * * * * cd /home/pi/rPI-multiDS18x20/example && /home/pi/rPI-multiDS18x20/example/graph.sh 

We have to change directory to the place where the scripts and database
are kept first, then call the “gettemp.pl” script. I also update the
graphs in the cron-job too, that's the second line.

I couldn't think of a nice way to automatically generate a graph
plotting script based on the content of sensors.conf, so you'll have to
make your own or edit my one in the examples directory.

If you've any questions, or problems or just found a ice way to
programmatically generate a graph plotting script, drop me an email. My
address can be found under About Me at the top of the page

This code is now maintained at
[github](https://github.com/g7uvw/rPI-multiDS18x20)

Have fun.

<Category:Experiments> <Category:HowTo> <Category:RaspberryPI>
<Category:Projects> <Category:Electronics>
