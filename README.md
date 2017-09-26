# 82c55 based IDE interface for the RC2014 computer

## The board
This is a 82c55 based IDE interface for the RC2014 Z80 8-bit computer. IDE uses a 16 bit data bus, so you need some kind of parallel interface to connect to an 8 bit computer.
Uses the same pin mapping as the ECB Disk IO v3 and design heavily cribbed from that. (https://www.retrobrewcomputers.org/lib/exe/fetch.php?media=boards:ecb:diskio-v3:ecb_diskio_v3_-_schematics_-_color_1.0.pdf)

## What can you use this for?

* Connecting an IDE hard drive to your RC2014 ;) and using it in RomWBW or in programs built with z88dk.

## Features

* 40 pin IDE for normal 3.5" hard drives
* 44 pin IDE header for 2.5" laptop hard drives
* Pins for supplying external 5v for the laptop HD if necessary
* IO port selectable via DIP switch

## Jumpers

Pin 1 is identified by having square pad.

### JP1 - JP4
Described on the back of the board. Basically some of the pins in the 82c55 C port have special purpose if you are using the board in mode 2 for something other than being an IDE adapter. Usually you'd just pick the defaults of pin 1-2.

### JP5
Use external power from J1

pins|description
----|------------
1-2 | Bus power
2-3 | External power

## Outputs

J2 - 40 pin IDE
J3 - 44 pin IDE

## Bill Of Materials

Reference| Value
---------|------
P1|1x39 right angled pin header
U1|82c55 (PLCC version) and PLCC44 socket
U3|74HCT04 (DIP)
J2|2x20 shrouded right angled pin header
D1|LED of your choosing ;)
R1|270R through hole resistor (or value suitable for chosen LED)
U2|74HCT688 (DIP)
RN1|10K commoned SIP resistor array (7 resistors, 8 pin)
C1|0.1uF ceramic capacitor
C2|0.1uF ceramic capacitor
C3|0.1uF ceramic capacitor
J3|IDE connector 2x22 Shrouded 2mm pitch connector (difficult to get! - can be ommitted if you're going to use 3.5" HDs)
SW1|6 switch DIP switch
J5|1x03 pin header straight, 2.54mm pitch
J1|1x02 pin header straight, 2.54mm pitch
JP1|1x03 pin header straight, 2.54mm pitch
JP2|1x03 pin header straight, 2.54mm pitch
JP3|1x03 pin header straight, 2.54mm pitch
JP4|1x03 pin header straight, 2.54mm pitch
R3|10K through hole resistor
R4|10K through hole resistor
R2|10K through hole resistor
