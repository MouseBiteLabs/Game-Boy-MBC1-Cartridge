# Game Boy MBC1 Cartridge

This is my design of a flashable MBC1-based cartridge for the Game Boy. The MBC1 mapper was used in many of the earlier Game Boy games, such as the Super Mario Land series, the Donkey Kong Land series, Metroid II, and one of my personal favorites - the original Link's Awakening. 

This circuit board should cover most, if not all, MBC1 games. The features are as follows:

- Able to make games that use 4 Mbit of ROM and 256 Kbit of RAM, or 16 Mbit of ROM and 64 Kbit of RAM, depending on how you configure the jumpers
- Compatibility with all four of the popular Game Boy battery management ICs - MM1026, MM1134, BA6129, and BA6735 (though you may have to add a few extra parts)
- The option to add battery backup to the cartridge *without* the need of the original battery management ICs - perfect for MBC1 donors that didn't have batteries in them
- Lower battery consumption compared to some of the original cartridges
- Fully compatible with the GBxCart RW so you can transfer games and save files to and from the board

[image of board scan]
[image of assembled board]

All gerbers and source files can be found in this repo, as this project is fully open source. Technical documentation of the board can be found in the Technical folder.

## Disclaimer

I am not responsible for any damage you do to your self or your property. Attempt this project at your own risk.

## Board Characteristics and Order Information

The zipped folder contains all the gerber files for this board. The following options **must** be chosen when ordering boards for yourself.

- Thickness: 0.8mm
- Surface Finish: ENIG
- Gold Fingers: Yes, 30° chamfer

**I sell this board on Etsy, so you don't have to buy multiples from board fabricators:**

[link]

You can alternatively use the zipped folder at any board fabricator you like. You may also buy the board from PCBWay using this link (disclosure: I receive 10% of the sale value to go twoards future PCB orders of my own):

[link]

## Board Configurations

Talk about jumpers and part possibilities.

## Bill of Materials (BOM)

Your parts list will vary depending on the game you are trying to make. Note that C5 - C7 footprints are only included for edge cases that may require them; you can ignore them unless you run into issues.

| Reference Designators | Value/Part Number              | Package          | Description        | No save carts | Save carts with MM1134 or BA6735 | Save carts with MM1026 or BA6129 | Save carts without donor U4 chip | Source                                           |
| --------------------- | ------------------------------ | ---------------- | ------------------ | ------------- | -------------------------------- | -------------------------------- | -------------------------------- | ------------------------------------------------ |
| B1                    | CR2025                         | CR2025           | Backup Battery     |               | X                                | X                                | X                                | [https://mou.sr/3PLccol](https://mou.sr/3PLccol) |
| C1                    | 0.1uF                          | 0603             | Capacitor (MLCC)   | X             | X                                | X                                | X                                | [https://mou.sr/3ENc15O](https://mou.sr/3ENc15O) |
| C2                    | 0.1uF                          | 0603             | Capacitor (MLCC)   | X             | X                                | X                                | X                                | https://mou.sr/3ENc15O                           |
| C3                    | 10uF                           | 0603             | Capacitor (MLCC)   |               | X                                | X                                | X                                | [https://mou.sr/3mZtSkF](https://mou.sr/3mZtSkF) |
| C4                    | 0.1uF                          | 0603             | Capacitor (MLCC)   |               | X                                | X                                |                                  | https://mou.sr/3ENc15O                           |
| C5                    | 0.001uF                        | 0603             | Capacitor (MLCC)   |               |                                  |                                  |                                  | [https://mou.sr/3PpAXoM](https://mou.sr/3PpAXoM) |
| C6                    | 0.001uF                        | 0603             | Capacitor (MLCC)   |               |                                  |                                  |                                  | [https://mou.sr/3PpAXoM](https://mou.sr/3PpAXoM) |
| C7                    | 0.001uF                        | 0603             | Capacitor (MLCC)   |               |                                  |                                  |                                  | [https://mou.sr/3PpAXoM](https://mou.sr/3PpAXoM) |
| C8                    | 0.1uF                          | 0603             | Capacitor (MLCC)   |               | X                                | X                                | X                                | https://mou.sr/3ENc15O                           |
| C9                    | 0.1uF                          | 0603             | Capacitor (MLCC)   |               |                                  |                                  | X                                | https://mou.sr/3ENc15O                           |
| Q1                    | MMBT3904                       | SOT-23           | NPN BJT            |               |                                  | X                                | X                                | [https://mou.sr/3Rv7yfA](https://mou.sr/3Rv7yfA) |
| Q2                    | Si2301CDS                      | SOT-23           | P-Channel FET      |               |                                  |                                  | X                                | [https://mou.sr/3LvBdSb](https://mou.sr/3LvBdSb) |
| R1                    | 10k                            | 0603             | Resistor           |               | X                                | X                                | X                                | https://mou.sr/3riR7IH                           |
| R2                    | 10k                            | 0603             | Resistor           | X             | X                                | X                                | X                                | https://mou.sr/3riR7IH                           |
| R3                    | 10k                            | 0603             | Resistor           | X             | X                                | X                                | X                                | https://mou.sr/3riR7IH                           |
| R4                    | 10k                            | 0603             | Resistor           | X             | X                                | X                                | X                                | https://mou.sr/3riR7IH                           |
| R5                    | 10k                            | 0603             | Resistor           | X             | X                                | X                                | X                                | https://mou.sr/3riR7IH                           |
| R6                    | 10k                            | 0603             | Resistor           |               |                                  |                                  | X                                | https://mou.sr/3riR7IH                           |
| R7                    | 10k                            | 0603             | Resistor           |               |                                  | X                                | X                                | https://mou.sr/3riR7IH                           |
| R8                    | 10k                            | 0603             | Resistor           | X             | X                                | X                                | X                                | https://mou.sr/3riR7IH                           |
| R9                    | 10k                            | 0603             | Resistor           |               |                                  | X                                |                                  | https://mou.sr/3riR7IH                           |
| U1                    | 29F016, 29F032, 29F033         | TSOP-48, TSOP-40 | Flash EEPROM       | X             | X                                | X                                | X                                | AliExpress or eBay                               |
| U2                    | MBC1                           | SOP-24           | MBC1 Mapper        | X             | X                                | X                                | X                                | Donor MBC1 Game Boy cartridge                    |
| U3                    | AS6C6264, AS6C62256            | SOP-28           | SRAM               |               | X                                | X                                | X                                | [https://mou.sr/450klcY](https://mou.sr/450klcY) |
| U4                    | MM1026, MM1134, BA6129, BA6735 | SOIC-8           | Battery Management |               | X                                | X                                |                                  | Donor Game Boy cartridge                         |
| U5                    | TPS3840DL42                    | SOT-23-5         | Supervisory IC     |               |                                  |                                  | X                                | [https://mou.sr/46lxKxA](https://mou.sr/46lxKxA) |
| U6                    | LM66100DCKR                    | SC70-6           | Ideal Diode        |               |                                  |                                  | X                                | [https://mou.sr/450kfSE](https://mou.sr/450kfSE) |

## Revision History

### v1.0
- Release revision

## Resources

- Gekkio's site
- Jeff's site
- MGGC discord

## License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/80x15.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>. You are able to copy and redistribute the material in any medium or format, as well as remix, transform, or build upon the material for any purpose (even commercial) - but you **must** give appropriate credit, provide a link to the license, and indicate if any changes were made.

©MouseBiteLabs 2023
