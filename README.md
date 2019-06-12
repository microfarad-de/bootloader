# Arduino Bootloader with Watchdog Timer Support

This repository contains a customized version of the Arduino bootloader firmware with watchdog timer (WDT) support.

The stock Arduino bootloader introduces a couple of seconds of delay during boot in order to provide the Arduino IDE 
with enough time to start uploading a new sketch. This has the follwoing disadvantages:

* The Arduino waits for a couple of seconds before starting to execute the main sketch upon initial power on.
* The Arduino gets stuck in an endless loop follwoing a watchdog timer expiry. The delay causes the WDT to expire before
the main sketch is able to load and perform a WDT reset.

With this custom bootloader, the aforementioned delay is introduced only if the reboot reason is due to pulling the external
reset pin to ground. A delay is no longer introduced during initial power on or a reboot due to a watchdog timer expiry, which
brings the follwoing benefits:

* The Arduino boots-up instantly after initial power on.
* The Arduino is no longer stuck in an endless loop following a WDT expiry.

The bootloader modification consists of defining `WATCHDOG_MODS` macro which activates the required functinality already implemented
into the stock bootloader.

This modification has been tested on an ATmega328P based Arduino Pro Mini and would supposedly work on any ATmega168/ATmega328 
based arduino board.

## Installation on MAC OS X

This has been tested with Arduino IDE version 1.8.8.

Please follow these steps:
* Install the Arduino IDE
* Install XCode from the App Store
* Open Terminal and go to the following path: `/Applications/Arduino.app/Contents/Java/hardware/arduino/avr/bootloaders/atmega`
* Make a backup copy of and replace the following file with the version from this repository: `ATmegaBOOT_168.c`

For ATMega328P at 16MHz:
* Make a backup copy and remove the following file: `ATmegaBOOT_168_atmega328.hex`
* Execute: `make atmega328`

For ATMega328P at 8MHz:
* Make a backup copy and remove the following file: `ATmegaBOOT_168_atmega328_pro_8MHz.hex`
* Execute: `make atmega328_pro8` 

Burn the bootloader:
* Connect your Arduino board with your PC via an in-system programmer (ISP) 
* Start your Arduino IDE and select your ISP model inside: Tools > Programmer
* Select your board model inside: Tools > Board
* Select your processor model inside: Tools > Processor
* Burn the bootloader by selecting: Tools > Burn Bootloader

