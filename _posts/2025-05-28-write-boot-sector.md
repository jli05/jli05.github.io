---
title: 'Write a boot sector to say "Hello!"'
layout: post
---

[BIOS](https://en.wikipedia.org/wiki/BIOS) stands for Basic Input Output System. On an IBM PC compatible machine, it provides basic routines for input/output, and governs the booting process till an Operating System takes over. This system has been somehow superceded by UEFI. But for its simplicity, we will write a boot sector that displays "Hello!" on Intel or AMD's i386 or x86_64 architecture.

## How does IBM PC and compatible boot?
On an IBM PC compatible machine, once the [Power-On Self Test](https://en.wikipedia.org/wiki/Power-on_self-test) (POST) is run, the BIOS selects a boot device, then copies the first sector from the device, into physical memory at memory address 0x7C00, and transfers control to that boot sector. On other systems, the process may be quite different.

## A Boot Sector in `nasm` assembly language
Different assemblers may have different syntax. Below is written for `nasm`. Its documentation can be found at [https://www.nasm.us/xdoc/2.16.03/html/nasmdoc0.html](https://www.nasm.us/xdoc/2.16.03/html/nasmdoc0.html).

`boot_sect.asm`:

```asm
[org 0x7c00]     ; load to address 0x7c00 in memory
                 ; where the boot load sector should reside
                 ; the last byte of the sector will be at 0x7dff in memory

mov ah, 0xe      ; teletype output routine for the BIOS interrupt call

mov al, 'H'      ; print each letter
int 0x10         ; interrupt calling the Basic Input Output System (BIOS)
mov al, 'e'
int 0x10
mov al, 'l'
int 0x10
mov al, 'l'
int 0x10
mov al, 'o'
int 0x10
mov al, '!'
int 0x10
mov al, 0dh      ; carriage return
int 0x10
mov al, 0ah      ; line feed
int 0x10

jmp $            ; jump to the current address, loop infinitely

                 ; fill the sector with 0 bytes up to the 510th byte
times 510-($-$$) db 0

db 0x55, 0xaa    ; last two magic bytes for the boot sector: 0x55, 0xaa
                 ; to make it 512 bytes in total for one sector
```

Numbers starting with `0x` are heximal (0-9, a-f, for 0-15 by a single letter). In `nasm`, a heximal number can be written in various ways: preceded by `0x`, or ending by `h`.

`0x0e`, `0xe`, `0eh`

are all one same number, but `eh` is not allowed, as it confuses with potential register name `eh`.

The line

`int 0x10`

is for accessing video services. Depending on the value in the register `ah`, it performs different jobs. For a complete table of the interrupts provided by BIOS, refer to [https://en.wikipedia.org/wiki/BIOS_interrupt_call](https://en.wikipedia.org/wiki/BIOS_interrupt_call).


## Compile the assembly file into binary for the boot sector
Run in the shell

```sh
nasm -f bin -o boot_sect.bin boot_sect.asm
```

The parameter `-f bin` is for producing raw binary file. `-o boot_sect.bin` indicates the file to be produced.

We can dump the content of `boot_sect.bin` with

```sh
od -A d -t x1 boot_sect.bin
```

The parameter `-A d` displays address in decimal number, `-t x1` displays each byte in heximal number.

![dump boot_sect.bin](/assets/2025-05-boot-sector/dump-decimal-address.png)

We can see the sector on the disk corresponds to address 0000-0511 (in decimal numbers). That is 512 bytes in total. When loaded into memory, the first byte will reside at address 0x7c00 in the memory, the second at 0x7c01 in the memory, so on and so forth. The last byte will be loaded at 0x7dff in the memory. The next unoccupied address will be 0x7e00. Below is a chart of memory use for i386 or x86_64 architecture.

![PC memory](/assets/2025-05-boot-sector/pc-memory.png)

Writing a Simple Operating System -- from Scratch, by Nick Blundell, pp 14.

## Let's play with the boot sector!

