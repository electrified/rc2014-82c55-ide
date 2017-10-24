# 82c55 based IDE interface for the RC2014 computer

![Picture of the board](./board_r2.jpg?raw=true)

## The board
This is a 82c55 based IDE interface for the RC2014 Z80 8-bit computer. IDE uses a 16 bit data bus, so you need some kind of parallel interface to connect to an 8 bit computer.
Uses the same pin mapping as the ECB Disk IO v3 and design heavily cribbed from that. (https://www.retrobrewcomputers.org/lib/exe/fetch.php?media=boards:ecb:diskio-v3:ecb_diskio_v3_-_schematics_-_color_1.0.pdf)

## What can you use this for?

* Connecting an IDE hard drive to your RC2014 ;) and using it in RomWBW or in C programs built with z88dk.

## Features

* 40 pin IDE for normal 3.5" hard drives
* 44 pin IDE header for 2.5" laptop hard drives
* Pins for supplying external 5v for the laptop HD if necessary
* IO port selectable via DIP switch
Connecting an IDE hard drive to your RC2014 ;) and using it in RomWBW or in programs built with z88dk.

## Getting started with RomWBW
You'll need to rebuild RomWBW with PPIDE support, by enabling the following configuration:

If you're using Scott Baker's fork of RomWBW, the easiest thing to do is modify those settings in ```/Source/HBIOS/Config/plt_smb.asm```

```
PPIDEENABLE     .EQU    TRUE    
PPIDEMODE       .EQU    PPIDEMODE_DIO3  
PPIDETRACE      .EQU    1              
PPIDE8BIT       .EQU    FALSE    
```

This is the configuration mode for the DiskIO v3, and it expects the board to be at I/O address 0x20, so set the A5 switch "on" and all the others off.

## Getting started with z88dk
Support has only recently landed  so you'll need a very recent pull of z88dk.
The 82c55 driver, IDE layer and DiskIO are only present within z88dk's RC2014 newlib target. (Not the classic library). 

Phillip Stevens has packaged ChaN's FatFS as a z88dk library, this can be used with above z88dk IDE driver to write programs that can access FAT filesystems.

See https://github.com/feilipu/z88dk-libraries/tree/master/ff, and the programs in the example directory.

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
Use external power from J1. Rev 2 of the board only has 2 pins, jumper for bus power.

pins|description
----|------------
1-2 | Bus power
2-3 | External power

## Outputs

* J2 - 40 pin IDE
* J3 - 44 pin IDE

## Bill Of Materials

Reference| Value|Rapid part no|RS part no|
---------|------|-------------|----------|
P1|1x39 right angled pin header
U1|CS82C55AZ (PLCC version)||921-5567
U1|PLCC44 socket|22-0350|
U3|74HCT04 (DIP)
J2|2x20 shrouded right angled pin header|19-0047|
D1|LED of your choosing ;)
R1|270R through hole resistor (or value suitable for chosen LED)
U2|74HCT688 (DIP)
RN1|10K commoned SIP resistor array (7 resistors, 8 pin)|63-0125|
C1|0.1uF ceramic capacitor
C2|0.1uF ceramic capacitor
C3|0.1uF ceramic capacitor
J3|2.5" IDE connector 2x22 Shrouded 2mm pitch connector
SW1|6 switch DIP switch|80-0338
J5|1x02 pin header straight, 2.54mm pitch
J1|1x02 pin header straight, 2.54mm pitch
JP1|1x03 pin header straight, 2.54mm pitch
JP2|1x03 pin header straight, 2.54mm pitch
JP3|1x03 pin header straight, 2.54mm pitch
JP4|1x03 pin header straight, 2.54mm pitch
R3|10K through hole resistor
R4|10K through hole resistor
R2|10K through hole resistor

## Construction tips
* Pin 20 of both the 40 pin and 44 pin IDE connector is usually missing as a "key" and is blocked off on many cables. It's best to remove pin 20 from both connectors before soldering them.
* If you're using a 2.5" and powering it from the bus, ensure you have a sufficient power supply. As an example, my 2.5" Seagate Momentum drive is specced to pull up to 2.5A on startup
* If you are using the board as an IDE contoller, keep the JP1-4 in their default position of 2-3
* The cutout on the boxed IDE connectors should be towards the RC2014 bus connector in the case of the 44 pin connector, and facing the user in the case of the 40 pin connector.


