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

Ensuring current coming out of the battery is as minimal as possible while the Game Boy is turned off is crucial for long-lasting save data. The higher the current, the lower the battery life! But we need to do this without breaking compatibility with normal operation. So here are the things we need to keep in mind:

1) SRAM must have continuous power to retain save data.

2) Data on the SRAM must be accessible during normal gameplay.

3) The number of battery-powered current-consuming components should be minimized, and those that are powered must use as little current as possible.

4) Operation while the voltage is ramping down after turning off the power switch must be restricted to prevent junk data from being written to cartridge RAM.

Original MBC1 carts manage all of this with U4, which was typically MM1026 or the equivalent BA6129. Later Game Boy carts used more advanced versions of these chips - the MM1134 or the equivalent BA6375 - that had an extra control line for easier SRAM control. I'll go over the function here, and for convenience, I'll be using the MM numbers when talking about them. I have also created a modern replacement for these components, in the event a donor cartridge you're using for the MBC1 chip does not include a backup battery. But first, a bit more background information. 

### SRAM Operational Background

For most Game Boy games, there were two main sizes of SRAM chips used - 64 Kbit and 256 Kbit (further refered to as just 64K or 256K). The pinouts of these two types of chips are nearly identical: 

![image](https://github.com/MouseBiteLabs/Game-Boy-MBC1-Cartridge/assets/97127539/63e3da82-bc68-4ffa-b1d4-43e77c5ee111)

The big differences are the number of address pins and the number of chip enable pins. The 64K chip has 13 address pins and two chip enables (/CE and CE2), and the 256K chip has 15 address pins and one chip enable (just /CE). The additional address pins on the 256K SRAM gives it more memory, but one of the chip enable pins had to be sacrificed to accommodate this. This is kind of important, because the chip enable pins influence two main things - they tell the chip when data is being accessed, *and* how much current the chip is drawing during standby. For 64K SRAM chips, that's pretty easy - use one chip enable pin for data access, and the other for data retention mode. But for 256K SRAM, you have to do both of those functions at once. Look at the requirements for the low "data retention current" on the AS6C6264 datasheet and the AS6C62256 datasheet:

![image](https://github.com/MouseBiteLabs/Game-Boy-MBC1-Cartridge/assets/97127539/7b344738-187d-4a9a-a395-5fcaa480575d)

For 64K, /CE needs to be at VCC, *or* CE2 needs to be at GND for the current to be minimized. For 256K, /CE needs to be at VCC, full stop (in this case, VCC is whatever voltage appears on the SRAM's VCC pin, so when power is off that's the battery voltage). That's not super convenient if the power is off - something needs to make the /CE pin go to VCC, which means whatever that something is better not be pulling a lot of current, lest we lower our battery life.

On original MBC1 cartridges, games that have 64K SRAM used the MBC1 for controlling the /CE pin for data access, and U4 (MM1026) for keeping CE2 at GND when power was off. But for 256K SRAM, that CE2 pin wasn't available anymore. Instead, the MBC1 was powered via of the battery as well, and had internal logic to keep /CE logic high when disabled. The MM1026 has a /CE output as well that will pull the output up to the battery voltage, however it *does not* have a way to allow the /CE pin to be accessed during normal gameplay. So the MBC1 needs to take care of both functions, but since it's powered from the battery when power is off, that means the battery life would be impacted. A necessary evil, without adding extra parts to the board or using a different kind of battery management chip. I don't know how much current the MBC1 pulls from the battery for games that had 256K SRAM, but I imagine it probably wasn't a whole lot - I would still expect 64K SRAM games to last longer though.

On my custom MBC1 board, I implement a method of accessing the RAM /CE pin *and* keeping it in a low power state, without needing the MBC1 to be powered by the battery, but introducing a few extra parts. I'll talk about that in a later section. But this SRAM background explains points 1 through 3 above. Onto point 4.

### Power-Off Considerations 

When you turn the power switch off on a DMG, the voltage on the VCC supply will ramp down. However, the DMG CPU requires 5 V to operate properly. A sufficiently lower voltage supplying the CPU may cause erroneous operation on the I/O pins of the CPU, even for a brief period of time. This can corrupt data stored on the SRAM of the game cartridge, say if the CPU accidentally writes junk data to part of the memory. This is bad! 

In order to prevent this, cartridges that have battery-backed SRAM on them use the battery management chip U4 to shut down operation of the CPU on pin 30 of the cart edge - the /RST (or /RESET) line. Asserting this to GND will stop all operation on the I/O pins, preventing corruption from occuring. According to the MM1026 datasheet, the /RESET output and CS output pins will be GND whenever voltage on the VCC pin is below 4.2V.

*Please note that the MM1026 has a /RESET output pin, but this is not the same as the Game Boy CPU /RESET input, the MBC1 /RESET input, or the cart edge /RST pin 30! I will call the MM1026's pin 2 the "Reset output pin" from now on for clarity.*

![image](https://github.com/MouseBiteLabs/Game-Boy-MBC1-Cartridge/assets/97127539/93550ae7-36ab-4ff0-8d3d-8d426dd7a917)

But that's not all the MM chip does. It also is used to pull the MBC1's /RESET input to GND as well. This is especially important for original Game Boy MBC1 carts with 256K SRAM that needed the MBC1 to be powered from the battery, as keeping this pin off will (assumedly) keep current draw of the MBC1 to a minimum. Furthermore, pulling this pin to GND also causes the MBC1 to assert the SRAM's /CE pin high to keep the SRAM in a low power state.

### Open Collector Requirements on CPU /RESET Input

One minor but important note - there are actually two outputs on the MM1026 that go low when power is below 4.2 V. One of them is open collector (pin 2, the "Reset output"), and one is instead driven high by an internal transistor or pulled low by an internal pull-down (pin 3, the "CS output"). This is important because it means *you cannot safely use the CS output pin to control the Game Boy's /RESET line.*

![image](https://github.com/MouseBiteLabs/Game-Boy-MBC1-Cartridge/assets/97127539/0cda9c53-7df3-498b-8542-f712198cce23)

On the DMG, the CPU's /RESET input is also connected to half of the power switch. When the switch is fully turned off, the /RESET input is pulled to GND directly.

![image](https://github.com/MouseBiteLabs/Game-Boy-MBC1-Cartridge/assets/97127539/601c3760-1945-438b-b4e6-52c71d4de8ae)

This means that to ensure no damage is done to the battery management chip, any output connected to the Game Boy's /RESET input must be **open collector**. Open collector outputs can either float their output, or (in this case) pull it to GND - reference the "Pin 2 Reset Output" schematic above. That means pulling it to GND with the power switch won't damage anything internal to the chip, it will just safely bypass the pin's function.

But, in the case of pin 3 on the MM1026, the "CS output" pin, imagine a scenario where the power switch was turned off, but the capacitors on the board have not yet discharged down to 0V. This means there'd still be some residual voltage on the VCC net. But, the CS output pin is connected to VCC via the transistor internal to the chip, and if VCC hasn't dropped far enough, then this would still be pulled high to VCC. So if you had connected pin 3 to the /RESET input on the Game Boy, in this scenario, there would be a path from VCC on the MM1026's supply pin, *through* the transistor on pin 3, through the power switch on the DMG, to GND - short circuiting the capacitors on the board. This means that for a moment, the MM1026 internal transistor driving pin 3 would experience a surge of current. Since the output is not designed to do that, it could cause damage to the part. So let's avoid that situation!

In conclusion, any output connected to the /RESET net on the Game Boy *must* be open collector.

*Funny enough, on the GBC, they added a chip that pulls the /RESET line to GND through an open collector output whenever the voltage on VCC drops below 3.5 V, instead of using the power switch to ground the input. If the DMG had this, maybe Nintendo wouldn't have needed these MM chips but could use something a bit simpler. But we must design cartridges with DMG compatibility in mind!* 

### Using an Original MM-series Chip for Battery Management

For reference, here is a pinout diagram of the MM1026 and MM1134.

[picture] 

### Creating a Battery Bus

In order for the SRAM to retain data, it must be powered at all times. If it loses power, even for a few microseconds, all the data can be erased. But, we don't want to have to rely on battery power while the cartridge is on; we want the Game Boy to power the SRAM in that situation. The MM chip caters to this with its two input voltage pins - pin 8 for the cart power VCC, and pin 4 for the battery power VBATT. Pin 6, VOUT, is the combination of these two inputs. VOUT is supplied by whichever input is higher, without interruption of power.

### Discrete Replacement of U4

#### Creating a Battery Bus

We can use a simple diode-or supply to achieve this, but diodes drop a bit of voltage as current flows through them. It's much better to use an ideal diode set up.

### Reset IC (U4)

### Group B Components

### Group A Components

## Estimating Battery Life

You can get a very rough estimate of battery life by measuring the voltage in millivolts across the battery-series resistor R1. Just use Ohm's Law to find the current: I = V / R where V is the voltage in millivolts you measure and R is the resistance of R1 in ohms. If you use these units, the current will be in milliamps. 

Then, find the milliamp-hour rating of your selected battery (preferrably from a datasheet). For example, the Renata CR2025 battery in the BOM are rated for 165 mAh. Take this number and divide by the milliamps calculated above, to get a rough estimate of the number of hours the battery can supply when the console is turned off (when it is on, the Game Boy powers the cart so the battery is unused). Divide by 24 and then 365 (or just divide by 8760) and you will get a number of years. 

So for an overall equation to determine the very approximate number of years the battery will survive:

Years = Resistance of R1 (ohms) * Battery capacity (mAh) / Voltage (mV) / 8760 

For an example: an MBC1 cartridge made according to the BOM where R1 is 10 kΩ (or 10000 Ω), using a CR2025 rated for 165 mAh, and a voltage of 10 mV measured across the terminals of R1, yields 10000 * 165 / 10 / 8760 = 18.84 years of battery survival. This number can be different due to changing environmental conditions, self-discharge of the battery, and also actual capacity of the battery, so a more conservative estimate would probably be about 15 years.
