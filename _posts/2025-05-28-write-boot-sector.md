---
title: 'Write a boot sector to say "Hello!"'
layout: post
---


## How does IBM PC and compatible boot?
On an IBM PC compatible machine, once the [Power-On Self Test](https://en.wikipedia.org/wiki/Power-on_self-test) (POST) is run, the [BIOS](https://en.wikipedia.org/wiki/BIOS) selects a boot device, then copies the first sector from the device into physical memory at memory address 0x7C00, and transfers control to that boot sector.

## A boot sector in assembly code
Below is written for the `nasm` assembler. Its documentation can be found at [https://www.nasm.us/xdoc/2.16.03/html/nasmdoc0.html](https://www.nasm.us/xdoc/2.16.03/html/nasmdoc0.html).

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

Numbers starting with `0x` are heximal (0-9 and a-f, representing the decimal 0-15 by a single letter). In `nasm`, a heximal number can be written in various ways: preceded by `0x`, or ending by `h`.

`0x0e`, `0xe`, `0eh`

are all one same number, but `eh` is not, as it confuses with potential register name `eh`.

The interrupt call

`int 0x10`

is for accessing video services. Depending on the value in the register `ah`, it performs different jobs, such as displaying a letter or setting a colour. Refer to [https://en.wikipedia.org/wiki/BIOS_interrupt_call](https://en.wikipedia.org/wiki/BIOS_interrupt_call) for a complete table of the interrupt calls.


## Compile the assembly file into binary for the boot sector
The assembly text file need be converted into a binary file to occupy the boot sector. Run in the shell

```sh
nasm -f bin -o boot_sect.bin boot_sect.asm
```

The parameter `-f bin` specifies format of output: a raw binary file, `-o boot_sect.bin` indicates the file name for output.

We can dump the content of `boot_sect.bin` with

```sh
od -A d -t x1 boot_sect.bin
```

The parameter `-A d` displays address in decimal number, `-t x1` displays each byte in heximal number. The dump looks like

```
0000000 b4 0e b0 48 cd 10 b0 65 cd 10 b0 6c cd 10 b0 6c
0000016 cd 10 b0 6f cd 10 b0 21 cd 10 b0 0d cd 10 b0 0a
0000032 cd 10 eb fe 00 00 00 00 00 00 00 00 00 00 00 00
0000048 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
*
0000496 00 00 00 00 00 00 00 00 00 00 00 00 00 00 55 aa
0000512
```

We can see the sector corresponds to address 0000-0511 (in decimal numbers) on the disk: 512 bytes in total. When loaded into memory, the first byte will reside at address 0x7c00 in the memory, the second at 0x7c01 in the memory, so on and so forth. Below is the use of the first 1 megabyte memory on i386 or x86_64 architecture.

![PC memory](/assets/2025-05-boot-sector/pc-memory.png)

Writing a Simple Operating System -- from Scratch, by Nick Blundell, pp 14.

According to Wikipedia, the memory 0xc0000-0xf0000 is used by "option ROM modules" during the [system startup](https://en.wikipedia.org/wiki/BIOS#System_startup) process.

## What does "carriage return" or "line feed" do separately?
How to "run" the boot sector? We need an emulator that simulates the i386 or x86_64 startup process as on physical hardware. [qemu](https://www.qemu.org/) is a commonly used one. After installation, run in the shell

```sh
qemu-system-i386 -nographic -drive format=raw,file=boot_sect.bin,if=floppy
```

or

```sh
qemu-system-x86_64 -nographic -drive format=raw,file=boot_sect.bin,if=floppy
```

One will see the following screen. The cursor is placed correctly, after first printing the 13th (0dh) ASCII letter carriage return then the 10th (0ah) ASCII letter line feed. In the side bar at the top of [ASCII](https://en.wikipedia.org/wiki/ASCII) one would find the chart for the 256 letters.

![booting screen with carriage return and line feed](/assets/2025-05-boot-sector/boot-cr-lf.png)

If we change the assembly, only print the carriage return, we will see the cursor goes to the first letter but stays on the same row.

![booting screen with only carriage return](/assets/2025-05-boot-sector/boot-cr.png)

If we change the assembly, only print the line feed, we will see the cursor goes to the next line but does not go to the first letter position.

![booting screen with only line feed](/assets/2025-05-boot-sector/boot-lf.png)

## Magic number of the boot sector
Did you note the assembly code writes two bytes at the end of the boot sector?

```asm
db 0x55, 0xaa
```

`db` for "declare byte". A boot sector has to terminate with these two bytes. If we change them to `0xbb, 0xaa`, the booting will fail as below.

![booting screen for boot sector with wrong magic number](/assets/2025-05-boot-sector/wrong-magic-number.png)
