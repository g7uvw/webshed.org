---
title: Arduino Low Power Temp Humidity
permalink: wiki/Arduino_Low_Power_Temp_Humidity/
layout: wiki
---

Low Power Arduino Temperature & Humidity Logging Sensor Platform
----------------------------------------------------------------

I was contacted by a small city archive to provide a temperature and
humidity logging system for their store rooms. Heritage items are kept
under standardized conditions in storage to prevent (or at least retard)
deterioration, pest damage and mold growth; ensuring those conditions
are maintained over long periods of time is essential. Commercial data
logging systems exist, but are prohibitively expensive for small
institutions.

The requirement was for a number of data loggers recording ambient
temperature & humidity values every 10 minutes (user configurable). A
low-power RF link on 2.4 GHz was investigated for remote reporting
sensors, but the RF environment in the archive was inhospitable - thick
walls, archive spread over several floors and strong co-channel
interference from building WiFi installation. Instead, it was decided to
record sensor readings directly on the sensor nodes for periodic
collection.

### Sensors

I initially planned to use a combination of DS18B20 1-wire temperature
sensor and SHT21 humidity sensor on the data logger, both parts I'm very
familiar with. I was unable to source sufficient SHT21s, so had to fall
back on another humidity sensor - the DHT22. I'd originally considered
the DHT22 to be too low specification for this application, until I
found an [article](http://www.kandrsmith.org/RJS/Misc/hygrometers.html)
testing their performance - this, along with some testing of my own
convinced me to use them. The DHT22 includes a temperature sensor,
removing the need for a DS18B20 and reducing the bill of materials.

### Processor Hardware

For the sake of rapid prototyping and code-reuse, the Arduino platform
was used to interface the sensors with the recording media (an SD card).
A PCB was designed and about to be sent for fabrication when I became
aware of cheaply available clones of both the Arduino UNO platform and a
combined real-time clock & SD card interface shield. These could be
purchased for less than the cost of fabricating and stuffing one custom
PCB; four were ordered for testing.

### Hardware Power Consumption

The clone Arduino UNO and RTC / SD card boards drew ~65 mA at 5V DC when
programmed to write incrementing numbers to a file on a 4 gigabyte
Kingston class-4 SDHC card. This level of power consumption gave a
calculated battery life of ~2 days if powered from three good quality
alkaline AA cells - dire. Removing the power LEDs from both boards
reduced the current to 20mA, a big improvement but still an order of
magnitude higher than I'd like.

As I was injecting power directly into the 5V DC port on the Arduino, I
didn't require the on-board voltage regulator any more, I could remove
this and save a few mA of quiescent current - total current draw was
down to 16mA. I could probably have saved more current by removing the
USB interface circuitry too, but this was still required.

### Software Power Consumption

The data loggers will only need to record a few times per hour, the rest
of the time they can power down to save the batteries. It was also
decided to use the SDFAT library to access the SD card rather than the
SD library, because the SD library was reported to leave the SPI bus
clock active after finishing accessing the SD card, this leaves the card
awake and consuming power.