# GameDroid
---

A GameBoy emulator for Android. This is the term project for COMP 3004 at Carleton University.

Repository structure:
* `dev_logs/` contains weekly development logs for each team member
* `docs/` contains documents related to the project. This is mostly documentation of GameBoy hardware to refer to while implementing the emulator. Unless otherwise stated, this material is by other authors and is used purely as reference material
* `GameDroid/` contains the application's source code

# CPU instructions to implement (single byte)

|      | x0       | x1      | x2       | x3      | x4        | x5      | x6       | x7      | x8        | x9      | xA       | xB      | xC       | xD     | xE       | xF    |
|------|----------|---------|----------|---------|-----------|---------|----------|---------|-----------|---------|----------|---------|----------|--------|----------|-------|
|**x0**|          |         |          |         |           |         |          |         |           |ADD HL,BC|          |         |          |        |          |       |
|**x1**|STOP 0    |         |          |         |           |         |          |         |           |ADD HL,DE|          |         |          |        |          |       |
|**x2**|          |         |          |         |           |         |          |         |           |ADD HL,HL|          |         |          |        |          |       |
|**x3**|          |         |          |         |           |         |          |         |           |ADD HL,SP|          |         |          |        |          |       |
|**x4**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**x5**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**x6**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**x7**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**x8**|ADD A,B   |ADD A,C  |ADD A,D   |ADD A,E  |ADD A,H    |ADD A,L  |ADD A,(HL)|ADD A,A  |ADC A,B    |ADC A,C  |ADC A,D   |ADC A,E  |ADC A,H   |ADC A,L |ADC A,(HL)|ADC A,A|
|**x9**|SUB B     |SUB C    |SUB D     |SUB E    |SUB H      |SUB L    |SUB (HL)  |SUB A    |SBC A,B    |SBC A,C  |SBC A,D   |SBC A,E  |SBC A,H   |SBC A,L |SBC A,(HL)|SBC A,A|
|**xA**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**xB**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**xC**|          |         |          |         |           |         |ADD A,d8  |         |           |         |          |         |          |        |ADC A,d8  |       |
|**xD**|          |         |          |         |           |         |SUB d8    |         |           |         |          |         |          |        |SBC A,d8  |       |
|**xE**|          |         |          |         |           |         |          |         |ADD SP,r8  |         |          |         |          |        |          |       |
|**xF**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |


# CPU instructions to implement (prefix CB)

|      | x0       | x1      | x2       | x3      | x4        | x5      | x6       | x7      | x8        | x9      | xA       | xB      | xC       | xD     | xE       | xF    |
|------|----------|---------|----------|---------|-----------|---------|----------|---------|-----------|---------|----------|---------|----------|--------|----------|-------|
|**x0**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**x1**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**x2**|          |         |          |         |           |         |          |         |SRA B      |SRA C    |SRA D     |SRA E    |SRA H     |SRA L   |SRA (HL)  |SRA A  |
|**x3**|          |         |          |         |           |         |          |         |SRL B      |SRL C    |SRL D     |SRL E    |SRL H     |SRL L   |SRL (HL)  |SRL A  |
|**x4**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**x5**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**x6**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**x7**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**x8**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**x9**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**xA**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**xB**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**xC**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**xD**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**xE**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |
|**xF**|          |         |          |         |           |         |          |         |           |         |          |         |          |        |          |       |


* [Original opcode table](http://pastraiser.com/cpu/gameboy/gameboy_opcodes.html) with more info (such as flags modified and cycle count).
* [Opcode summary](http://gameboy.mongenel.com/dmg/opcodes.html)

# Unsupported cartridge types

| Number | Description                 |
|--------|-----------------------------|
| 0x03   | MBC1+RAM+BATT               |
| 0x05   | MBC2                        |
| 0x06   | MBC2+BATT                   |
| 0x0B   | MMM01                       |
| 0x0C   | MMM01+RAM                   |
| 0x0D   | MMM01+RAM+BATT              |
| 0x0F   | MBC3+RTC+BATT               |
| 0x10   | MBC3+RTC+RAM+BATT           |
| 0x11   | MBC3                        |
| 0x12   | MBC3+RAM                    |
| 0x13   | MBC3+RAM+BATT               |
| 0x15   | MBC4                        |
| 0x16   | MBC4+RAM                    |
| 0x17   | MBC4+RAM+BATT               |
| 0x1B   | MBC5+RAM+BATT               |
| 0x1C   | MBC5+RUMBLE                 |
| 0x1D   | MBC5+RUMBLE+RAM             |
| 0x1E   | MBC5+RUMBLE+RAM+BATT        |
| 0x20   | MBC6                        |
| 0x22   | MBC7+SENSOR+RUMBLE+RAM+BATT |
| 0xFC   | POCKET CAMERA               |
| 0xFD   | BANDAI TAMA5                |
| 0xFE   | HuC3                        |
| 0xFF   | HuC1+RAM+BATT               |

Major features:
* Nothing (yet)

Supported MBCs:
* MBC1
* MBC5

Minimum Android version: 4.0.3 (Ice Cream Sandwich)
