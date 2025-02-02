## Chapter 7: Memory Map

The Commander X16 has 64 KB of ROM and 2,088 KB (2 MB[^1] + 40 KB) of RAM. Some of the ROM and RAM is always visible at certain address ranges, while the remaining ROM and RAM is banked into one of two address windows.

This is an overview of the X16 memory map:

|Addresses  |Description                                                       |
|-----------|------------------------------------------------------------------|
|$0000-$9EFF|Fixed RAM (40 KB minus 256 bytes)								   |
|$9F00-$9FFF|I/O Area (256 bytes)											   |
|$A000-$BFFF|Banked RAM (8 KB window into one of 256 banks for a total of 2 MB)|
|$C000-$FFFF|Banked ROM (16 KB window into one of 32 banks for a total of 512 KB) |

### Banked Memory

The currently enabled RAM and ROM banks can be configured by writing to zero page locations 0 and 1:

|Address  |Description              |
|---------|-------------------------|
|$0000    |Current RAM bank (0-255) |
|$0001    |Current ROM bank (0-31)  |

The currently set banks can also be read back from the respective memory locations. Both settings default to 0 on RESET. The upper three bits of location 1 are undefined.

### ROM Allocations

This is the allocation of the banks of banked ROM:

|Bank|Name   |Description                                            |
|----|-------|-------------------------------------------------------|
|0   |KERNAL |KERNAL operating system and drivers                    |
|1   |KEYBD  |Keyboard layout tables                                 |
|2   |CBDOS  |The computer-based CBM-DOS for FAT32 SD cards          |
|3   |GEOS   |GEOS KERNAL                                            |
|4   |BASIC  |BASIC interpreter                                      |
|5   |MONITOR|Machine Language Monitor                               |
|6   |CHARSET|PETSCII and ISO character sets (uploaded into VRAM)    |
|7   |CODEX  |CodeX16 Interactive Assembly Environment / Monitor     |
|8   |GRAPH  |Kernal graph and font routines                         |
|9   |DEMO   |Demo routines                                          |
|10  |AUDIO  |Audio routines                                         |
|11-31|–      |*[Currently unused]*                                   |

**Important**: The layout of the banks is still constantly changing.

### RAM Contents

This is the allocation of fixed RAM in the KERNAL/BASIC environment.

|Addresses  |Description                                                     |
|-----------|----------------------------------------------------------------|
|$0000-$00FF|Zero page                                                       |
|$0100-$01FF|CPU stack                                                       |
|$0200-$03FF|KERNAL and BASIC variables, vectors                             |
|$0400-$07FF|Available for machine code programs or custom data storage      |
|$0800-$9EFF|BASIC program/variables; available to the user                  |

The $0400-$07FF can be seen as the equivalent of $C000-$CFFF on a C64. A typical use would be for helper machine code called by BASIC.

#### Zero Page

|Addresses  |Description                            |
|-----------|---------------------------------------|
|$0000-$0001|Banking registers                      |
|$0002-$0021|16 bit registers r0-r15 for KERNAL API |
|$0022-$007F|Available to the user                  |
|$0080-$009C|Used by KERNAL and DOS                 |
|$009D-$00A8|Reserved for DOS/BASIC                 |
|$00A9-$00D3|Used by the Math library (and BASIC)   |
|$00D4-$00FF|Used by BASIC                          |

Machine code applications are free to reuse the BASIC area, and if they don't use the Math library, also that area.

#### Banking

This is the allocation of banked RAM in the KERNAL/BASIC environment.

|Bank |Description                                 |
|-----|--------------------------------------------|
|0    |Used for KERNAL/CBDOS variables and buffers |
|1-255|Available to the user                       |

(On systems with only 512 KB RAM, banks 64-255 are unavailable.)

During startup, the KERNAL activates RAM bank 1 as the default for the user.

### I/O Area

This is the memory map of the I/O Area:

|Addresses  |Description                  |
|-----------|-----------------------------|
|$9F00-$9F0F|VIA I/O controller #1        |
|$9F10-$9F1F|VIA I/O controller #2        |
|$9F20-$9F3F|VERA video controller        |
|$9F40-$9F41|YM2151 audio controller      |
|$9F42-$9F5F|Reserved                     |
|$9F60-$9FFF|External devices             |

---

[^1]: Current development systems have 2 MB of bankable RAM. Actual hardware is currently planned to have an option of either 512 KB or 2 MB of RAM.

