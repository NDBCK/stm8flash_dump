stm8flash_dump
==============

This software is an adaptation of [stm8flash](https://github.com/vdudouyt/stm8flash).
This version adds a "test" modus to check if the STM8 device is Read Out Protected (ROP) and when it is not protected it dumps all memory regions to seperate files.
All in a single run.

Compiled exe for windows is included.


Usage
-----

```
stm8flash -c stlinkv2 -p <partname> -t <filename>
```


The supported file types are Intel Hex, Motorola S-Record and Raw Binary. The type is detected by the file extension.
 I had the best results using ".hex" (intel hex) format.

Why?
----

Voltage glitching with the original STM8flash is more difficult.
	*There is an extra reset genreated in the original program because of the high speed modus
	*For every memory region you want to read in the original program, you need to restart
	*Checking if the voltage glitch is succesfull is not straightforward

Reasoning
---------

When trying to read the option bytes (memory address 0x4800), the response is 0x71 when the STM8 is protected.
If the ROP (Read Out Protection) is glitched, the real option bytes are returned. If so, the software dumps all memory regions to seperate files

```
Example:
stm8flash.exe -c stlinkv2 -p stm8s003f3 -t dump.hex
If the protection is glitched or not in place following files are dumped:
dump_OPT.hex		Option bytes
dump_PROM.hex		Eeprom data
dump_RAM.hex		Current RAM data
dump_ROM.hex		Flash data
```


Compromises
-----------
Because I don't have a stlinkv1 programmer (and it is less common), this programmer was removed.
ESPstlink functionallity was also removed to make it build on windows (using MSYS2 MINIGW and libusb).

High speed mode was disabled from original program, because this introduces and extra reset for the microcontroller.