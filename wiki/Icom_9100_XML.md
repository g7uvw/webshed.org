---
title: Icom 9100 XML
permalink: wiki/Icom_9100_XML/
layout: wiki
---

Icom 1900 RIGCAT XML file
-------------------------

There's an issue with the standard rigcat xml supplied for the Icom 9100
radio - it will not work above 60MHz. The issue is the max frequency is
set in the XML file to 60000000 Hz.

Here's my working version:
![IC9100.xml.zip](IC-9100.xml.zip "fig:IC9100.xml.zip")
