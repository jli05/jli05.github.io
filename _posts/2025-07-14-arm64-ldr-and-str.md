---
title: ARM64 LDR (load register) and STR (store register) instructions
layout: post
---

This article accompanies Lesson 3 "LDR, STR" of LaurieWired in the [ARM assembly tutorial](https://www.youtube.com/playlist?list=PLn_It163He32Ujm-l_czgEBhbJjOUgFhg).

## Documentation for ARM32 and ARM64 assembly
On [https://developer.arm.com](https://developer.arm.com), search for "armasm user guide". In the result list, one may find 

Arm Compiler armasm User Guide. Version: 6.6.5. Recommended

Version 6.6.5 is the latest version as of writing. Click on it. The LDR and STR instructions are documented under the section "A64 Data Transfer Instructions".

## LDR (load register)
LDR reads a word (32-bit) at specified address. We will look at two versions of `ldr.s` that does the same thing.

### version 1
```asm
.global _start

.text
_start:
    // load content of var2, the decimal 6
    ldr x0, var2
    mov x8, #0x5d
    svc #0

.data
// a word is 32-bit
// or 8-digit in heximal mode
var1: .word 5     // decimal 5
var2: .word 6     // decimal 6
```

The code in this article has to be linked with `-static` flag, or an error will be produced. To run it in the shell:

```sh
$ rm ldr.o; as -o ldr.o ldr.s
$ gcc -o ldr ldr.o -nostdlib -static
$ ./ldr; echo $?
6
```

If we dissemble it in the shell:

```sh
$ objdump -d ldr

ldr:     file format elf64-littleaarch64


Disassembly of section .text:

00000000004000b0 <_start>:
  4000b0:	58080080 	ldr	x0, 4100c0 <var2>
  4000b4:	d2800ba8 	mov	x8, #0x5d                  	// #93
  4000b8:	d4000001 	svc	#0x0
```

We suppose at the address 0x4100c0 is the number decimal 6. How to prove it?

We first print the symbol table in the executable `ldr`:

```sh
$ readelf -s ldr

Symbol table '.symtab' contains 15 entries:
   Num:    Value          Size Type    Bind   Vis      Ndx Name
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND 
     1: 00000000004000b0     0 SECTION LOCAL  DEFAULT    1 .text
     2: 00000000004100bc     0 SECTION LOCAL  DEFAULT    2 .data
     3: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS ldr.o
     4: 00000000004000b0     0 NOTYPE  LOCAL  DEFAULT    1 $x
     5: 00000000004100c0     0 NOTYPE  LOCAL  DEFAULT    2 var2
     6: 00000000004100bc     0 NOTYPE  LOCAL  DEFAULT    2 var1
     7: 00000000004100c4     0 NOTYPE  GLOBAL DEFAULT    2 _bss_end__
     8: 00000000004100c4     0 NOTYPE  GLOBAL DEFAULT    2 __bss_start__
     9: 00000000004100c4     0 NOTYPE  GLOBAL DEFAULT    2 __bss_end__
    10: 00000000004000b0     0 NOTYPE  GLOBAL DEFAULT    1 _start
    11: 00000000004100c4     0 NOTYPE  GLOBAL DEFAULT    2 __bss_start
    12: 00000000004100c8     0 NOTYPE  GLOBAL DEFAULT    2 __end__
    13: 00000000004100c4     0 NOTYPE  GLOBAL DEFAULT    2 _edata
    14: 00000000004100c8     0 NOTYPE  GLOBAL DEFAULT    2 _end
```

It reads `var2` is at address 0x4100c0, in section 2 (under column Ndx); `var1` is at address 0x4100bc, in the same section 2. We now print out the section 2, or the section `.data` of `ldr`:

```
$ readelf -x2 ldr         # or readelf -x .data ldr

Hex dump of section '.data':
  0x004100bc 05000000 06000000                   ........
```

We see at address 0x004100bc is some content `0x05000000`, at the next address which would be 0x004100bc + 0x00000004 = 0x004100c0 is some content `0x06000000`.

But `0x05000000` is many times larger than the decimal 5, or heximal 0x5? We have to talk about the order in which the processor records a number, "endianness". If we search for "aarch64 little endian", we may find:

> AArch64, which is the 64-bit architecture for ARM, typically uses little-endian format by default. This means that the least significant byte is stored at the smallest memory address.

What it says is that from address 0x004100bc to 0x004100bf, to record the number 0x00000005, going from the most significant to the least significant byte (left to right), the four bytes 0x00, 0x00, 0x00, 0x05 would go into addresses 

| The Address  | Recorded Byte |
| ------- | ------- |
| 0x004100bc | 0x05 |
| 0x004100bd | 0x00 |
| 0x004100be | 0x00 |
| 0x004100bf | 0x00 |

which explains starting from the address 0x004100bc, why the content looks to be 0x05000000.

### version 2
We look at an variety of `ldr.s`:

```asm
.global _start

.text
_start:
    ldr x1, =var2
    ldr x0, [x1]
    mov x8, #0x5d
    svc #0

.data
var1: .word 5
var2: .word 6
```

It would still return decimal number 6:

```sh
$ rm ldr.o; as -o ldr.o ldr.s
$ gcc -o ldr ldr.o -nostdlib -static
$ ./ldr; echo $?
6
```

Let's look at the dissembled `ldr`:

```sh
$ objdump -d ldr

ldr:     file format elf64-littleaarch64


Disassembly of section .text:

00000000004000b0 <_start>:
  4000b0:	58000081 	ldr	x1, 4000c0 <_start+0x10>
  4000b4:	f9400020 	ldr	x0, [x1]
  4000b8:	d2800ba8 	mov	x8, #0x5d                  	// #93
  4000bc:	d4000001 	svc	#0x0
  4000c0:	004100cc 	.word	0x004100cc
  4000c4:	00000000 	.word	0x00000000
```

The line `ldr x1, 4000c0` would load the (32-bit) word at address 0x4000c0 into register `x1`. Several lines down, `4000c0: 004100cc`: at 0x4000c0 is the word 0x004100cc, which will be read into `x1`.

The next line `ldr x0, [x1]` would read the word found at address 0x004100cc into `x0`. Let's check if `0x004100cc` is an address in the section `.data`.

```sh
$ readelf -x .data ldr

Hex dump of section '.data':
  0x004100c8 05000000 06000000                   ........

```

So at address 0x004100c8 is some content 0x05000000, at the next address which would be 0x004100c8 + 0x00000004 = 0x004100cc is some content 0x06000000. For what we explained about endianness, that would be heximal number 0x6, or decimal number 6.

### conclusion
We see from the above code,
- plain `var2` refers to an address (or a "**pointer**" in C programming language's jargon), at which the number 6 is stored;
- `=var2` is a **pointer** to the address at which number 6 is stored ("**pointer to a pointer**").

## STR (store register)
STR stores the content in a register to a specified address. The ARM64 code is not much different from the ARM32 one in Laurie's lesson.

```asm
.global _start

.text
_start:
    ldr x2, =var1
    ldr x3, =var2
    mov x1, #5
    str x1, [x3]
    ldr x0, [x3]
    mov x8, #0x5d
    svc #0

.data
var1: .word 6
var2: .word 7
```

To run it,

```sh
$ rm str.o; as -o str.o str.s
$ gcc -o str str.o -nostdlib -static
$ ./str; echo $?
5
```
