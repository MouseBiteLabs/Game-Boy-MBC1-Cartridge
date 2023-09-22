# Game Boy MBC1 Cartridge

This is my design of a flashable MBC1-based cartridge for the Game Boy. The MBC1 mapper was used in many of the earlier Game Boy games, such as the Super Mario Land series, the Donkey Kong Land series, Metroid II, and one of my personal favorites - the original Link's Awakening. 

This circuit board should cover most, if not all, MBC1 games. The features are as follows:

- Able to make games that use 4 Mbit of ROM and 256 Kbit of RAM, or 16 Mbit of ROM and 64 Kbit of RAM, depending on how you configure the jumpers
- Compatibility with all four of the popular Game Boy battery management ICs - MM1026, MM1134, BA6129, and BA6735 (though you may have to add a few extra parts)
- The option to add battery backup to the cartridge *without* the need of the original battery management ICs - perfect for MBC1 donors that didn't have batteries in them
- Lower battery consumption compared to some of the original cartridges
- Fully compatible with the <a href="https://www.gbxcart.com/">GBxCart RW</a> so you can transfer games and save files to and from the board

[image of board scan]
[image of assembled board]

All gerbers and source files can be found in this repo, as this project is fully open source. Technical documentation of the board can be found in the Technical folder.

## Disclaimer

I am not responsible for any damage you do to your self or your property. I do not guarantee design compatibility. Attempt this project at your own risk.

## Board Characteristics and Order Information

The zipped folder contains all the gerber files for this board. The following options **must** be chosen when ordering boards for yourself.

- Thickness: 0.8mm
- Surface Finish: ENIG
- Gold Fingers: Yes, 30° chamfer

**I sell this board on Etsy, so you don't have to buy multiples from board fabricators:**

[link]

You can alternatively use the zipped folder at any board fabricator you like. You may also buy the board from PCBWay using this link (disclosure: I receive 10% of the sale value to go twoards future PCB orders of my own):

[link]

The board is also listed on OshPark as well. **Be sure to get them in 0.8mm thickness if you order from here.**

[link]

## Board Configurations

The board comes with four sets of jumper pads for solder bridges. SJ1 and SJ2 require you to solder bridge the middle pad either to the top pad or the bottom pad. SJ3 and SJ4 are configured by either leaving them alone or bridging them with solder. Here are the situations where you need to add solder bridges.

### ROM/RAM Size Selection (SJ1 and SJ2)

You must configure these pads for every game you make. Do not leave them empty. <a href="https://catskull.net/gb-rom-database/">You can find a list of games here with their respective ROM and RAM sizes.</a>

- Solder bridge SJ1 and SJ2 from their middle pads to the bottom pads for games that have larger RAM sizes (up to 4 Mb of ROM and up to 256 Kb of RAM).

- Solder bridge SJ1 and SJ2 from their middle pads to the top pads for games that have larger ROM sizes (up to 16 Mb of ROM and up to 64 Kb of RAM).

- SJ1 and SJ2 must be soldered in the same direction.

[picture]

### Making Games Without a U4 Donor Chip (SJ3)

The main function of U4 is battery management. It manages power from the battery to keep save data on the SRAM even when the Game Boy is off, shuts off components as power drops to prevent erroneous operation, and makes sure the SRAM is in a low-power state when running on battery power.

U4 can only be obtained from an existing Game Boy cartridge (or, potentially bought secondhand from eBay or AliExpress). U4 can be either MM1026 (or the equivalent BA6129) or MM1134 (or the equivalent BA6735). But, if you don't have one of these chips, you can use other parts that are commercially available to replace the function of these chips.

If you are not using U4, and instead using other parts for battery management, you must bridge SJ3. **But, this includes games that have no SRAM at all!** If your game does not have any save functionality, and you don't have U4 populated, you must bridge SJ3 too.

### Using an MM1134 or BA6735 for U4 (SJ4)

Bridge the jumper SJ4 if you have either an MM1134 or BA6735 for U4, specifically.

## Bill of Materials (BOM)

Your parts list will vary depending on the game you are trying to make, and what chips you have for the battery management (if any). Note that C5 - C7 footprints are only included for edge cases that may require them; you can ignore them unless you run into issues.

Please carefully review the parts you need for the board you are trying to make. Do not add any parts to your build that don't appear in the column for the game you are making. This means you *cannot* populate every component on the board at the same time.

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

## Things to Remember

- For battery management, use either U4 *or* U5 and U6 and supporting components. **Do not** use U4, U5, and U6 all on one board. They will interfere with each other.
- Kb is kilo**bits** and Mb is mega**bits**. Sometimes you will find game ROM and RAM sizes defined in terms of KB or kilo**bytes** and MB or mega**bytes**. You can convert Kb and Mb to KB and MB by dividing Kb or Mb by 8. For example, 256 Kb = 32 KB.
- You only need to provide ROM and RAM chips that have at least *or greater* the size of the game you are trying to make. That means you can use a 256Kb SRAM chip for a game that only requires 64Kb!

## Revision History

### v1.1
- Widen SRAM footprint for easier soldering
- Changed silkscreen for clarity

### v1.0
- Release revision

## Resources and Acknowledgements

- <a href="http://www.devrs.com/gb/files/hardware.html">Jeff Frohwein's GameBoy Tech Page</a>
- <a href="https://gbhwdb.gekkio.fi/">Game Boy Hardware Database</a>
- <a href="https://catskull.net/gb-rom-database/">Nintendo Gameboy Game List</a>
- <a href="https://wiki.tauwasser.eu/view/MBC1">Tauwasser's Wiki</a>
- <a href="https://www.ti.com/lit/ds/symlink/lm66100.pdf?HQS=dis-dk-null-digikeymode-dsf-pf-null-wwe&ts=1694502124931&ref_url=https%253A%252F%252Fwww.ti.com%252Fgeneral%252Fdocs%252Fsuppproductinfo.tsp%253FdistId%253D10%2526gotoUrl%253Dhttps%253A%252F%252Fwww.ti.com%252Flit%252Fgpn%252Flm66100">LM66100 Datasheet</a>
- <a href="https://www.alldatasheet.com/datasheet-pdf/pdf/99104/MITSUBISHI/MM1026.html">System Reset IC Datasheet</a>
- Board outline from <a href="https://tinkerer.us/projects/homebrew-gameboy-cartridge.html">Dillon Nichols's Homebrew Gameboy Cartridge project</a>
- Thank you to <a href="https://github.com/Gekkio">gekkio</a> for their deep Game Boy knowledge resources, and for collaboration in demystifying some of the design choices on Game Boy cartridges
- Thanks to the awesome members of the <a href="https://moddedgameboy.club/">Modded Gameboy Club</a> for their feedback and support during the entire project development

## License

<a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-sa/4.0/80x15.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/4.0/">Creative Commons Attribution-ShareAlike 4.0 International License</a>. You are able to copy and redistribute the material in any medium or format, as well as remix, transform, or build upon the material for any purpose (even commercial) - but you **must** give appropriate credit, provide a link to the license, and indicate if any changes were made.

©MouseBiteLabs 2023
