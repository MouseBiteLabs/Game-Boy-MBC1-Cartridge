# Technical Design Document 

This write-up will serve as an attempt of explaining some of the features on this cartridge. I won't go into heavy detail about the functionality of the MBC1, but I will attempt to explain some higher level functions and the reasoning behind the circuit.

## Schematic

## Cart Edge Pins

Very briefly, I will categorize the different pins on the 32-pin cartridge edge connector.

- Pin 1 and pin 32 are VCC and GND, respectively.
- Pin 2 is the CLK or PHI pin. This is not used on the MBC1 cartridge.
- Pin 3 is the /WR pin, pin 4 is the /RD pin, pin 5 is the /CS pin. These signals partially determine when data is read from the ROM or when data is read/written to the RAM.
- Pins 6 through 21 are the address pins A0 through A15.
- Pins 22 through 29 are the data pins D0 through D7.
- Pin 30 is the /RST (or reset) pin. When this pin is low, the Game Boy will cease operation.
- Pin 31 is a generally unused pin in Game Boy cartridges, but here it is connected to the EEPROM's /WE pin for compatibility with the GBxCART RW.

## ROM and RAM

## MBC1

## Battery Management

### Reset IC (U4)

### Group B Components

### Group A Components
