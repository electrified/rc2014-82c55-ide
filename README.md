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

## Getting started with RomWBW
Wayne Warthen has incorporated support for the RC2014 into [mainline RomWBW](https://github.com/wwarthen/RomWBW). Using this is preferred to Scott Baker's fork as there have been various improvements since Scott forked off. Support for the IDE adapter can be enabled by setting the following in your configuration and rebuilding:

```
PPIDEENABLE     .EQU    TRUE
```

Read the RomWBW readme (`Source/ReadMe.txt`) if you are unfamiliar with the process of creating your own configuration file/ building etc.

The other settings have already been set to the appropriate values in `Source/HBIOS/cfg_rc.asm`, so all is required is to enable PPIDE.

The board I/O address should be set to 0x20; set the A5 switch "on" and all the others off.

### Scott Baker's RomWBW fork
If you're using Scott's fork of RomWBW, the easiest thing to do is modify those settings in ```/Source/HBIOS/Config/plt_smb.asm```

```
PPIDEENABLE     .EQU    TRUE    
PPIDEMODE       .EQU    PPIDEMODE_DIO3  
PPIDETRACE      .EQU    1              
PPIDE8BIT       .EQU    FALSE    
```

This is the configuration mode for the DiskIO v3, and again it expects the I/O address to be set to 0x20

Note that there is a bug in the PPIDE support in this version, meaning real IDE hard drives (not CompactFlash devices) may not be detected. This has been fixed in mainline RomWBW (see https://github.com/wwarthen/RomWBW/issues/2). If you are experiencing this problem, commenting out https://github.com/sbelectronics/RomWBW/blob/master/Source/HBIOS/ppide.asm#L1008 "CALL	PPIDE_SETFEAT" should fix it, however using mainline RomWBW is preferred.

## Getting started with z88dk
Support has only recently landed  so you'll need a very recent pull of z88dk.
The 82c55 driver, IDE layer and DiskIO are only present within z88dk's RC2014 newlib target. (Not the classic library). 

Phillip Stevens has packaged ChaN's FatFS as a z88dk library, this can be used with above z88dk IDE driver to write programs that can access FAT filesystems.

See https://github.com/feilipu/z88dk-libraries/tree/master/ff, and the programs in the example directory.

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

Reference| Value|Rapid part no|RS part no|Digi-key part no|
---------|------|-------------|----------|----------------|
P1|1x39 right angled pin header|||732-5350-ND|
U1|CS82C55AZ (PLCC version)||921-5567|CS82C55AZ-ND|
U1|PLCC44 socket|22-0350||AE10057-ND|
U3|74HCT04 (DIP)|||296-1605-5-ND|
J2|2x20 shrouded right angled pin header|19-0047||732-5401-ND|
D1|LED of your choosing ;)|||160-1130-ND|
R1|270R through hole resistor (or value suitable for chosen LED)|||CF14JT270RCT-ND|
U2|74HCT688 (DIP)|||296-2131-5-ND|
RN1|10K commoned SIP resistor array (7 resistors, 8 pin)|63-0125||4608X-1-103LF-ND|
C1|0.1uF ceramic capacitor|||BC2665CT-ND|
C2|0.1uF ceramic capacitor|||BC2665CT-ND|
C3|0.1uF ceramic capacitor|||BC2665CT-ND|
J3|2.5" IDE connector 2x22 Shrouded 2mm pitch connector|||WM18567-ND|
SW1|6 switch DIP switch|80-0338||CT2066-ND|
J5|1x02 pin header straight, 2.54mm pitch|||732-5315-ND|
J1|1x02 pin header straight, 2.54mm pitch|||732-5315-ND|
JP1|1x03 pin header straight, 2.54mm pitch|||732-5316-ND|
JP2|1x03 pin header straight, 2.54mm pitch|||732-5316-ND|
JP3|1x03 pin header straight, 2.54mm pitch|||732-5316-ND|
JP4|1x03 pin header straight, 2.54mm pitch|||732-5316-ND|
R3|10K through hole resistor|||CF14JT10K0CT-ND|
R4|10K through hole resistor|||CF14JT10K0CT-ND|
R2|10K through hole resistor|||CF14JT10K0CT-ND|
||Jumpers|||952-2882-ND|

## Construction tips
* Pin 20 of both the 40 pin and 44 pin IDE connector is usually missing as a "key" and is blocked off on many cables. It's best to remove pin 20 from both connectors before soldering them.
* If you're using a 2.5" and powering it from the bus, ensure you have a sufficient power supply. As an example, my 2.5" Seagate Momentum drive is specced to pull up to 2.5A on startup
* If you are using the board as an IDE controller, keep the JP1-4 in their default position of 2-3
* The cutout on the boxed IDE connectors should be towards the RC2014 bus connector in the case of the 44 pin connector, and facing the user in the case of the 40 pin connector.


