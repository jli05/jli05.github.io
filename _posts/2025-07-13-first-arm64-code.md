---
title: The first ARM64 assembly program
layout: post
---

This article accompanies the Lesson 1 "Exit syscall" of LaurieWired in the [ARM assembly tutorial](https://www.youtube.com/playlist?list=PLn_It163He32Ujm-l_czgEBhbJjOUgFhg).

## Set up the toolchain
Launch a virtual machine with Graviton (ARM) core on Amazon Web Services (AWS). Install a 64-bit Linux on it and the gcc toolchain.

## The first assembly calling exit()

`hello.s`:

```asm
// the _start routine will be visible to external programs 
.global _start

// this is the section of code in text
.section .text

// start of routine _start
_start:
    // we are going to call Linux exit()
    // via software interrupt
    // the x0 register stores the first parameter
    // the x8 register stores the number of the Linux function to call

    // first param for exit(), the return value decimal 42
    // constants are preceded by hash sign
    mov x0, #42

    // x8: 0x5d for the function exit()
    // refer to the Linux System Call Table reference below
    // for function numbers
    // constants are preceded by hash sign
    // heximal numbers are preceded by 0x
    mov x8, #0x5d

    // trigger software interrupt
    // the param following the svc word does not matter
    svc #0
```


To assemble the text file of assembly instructions into binary code and run it,

```sh
$ as -o hello.o hello.s    # assemble into an object file
$ gcc -o hello hello.o -nostdlib   # link into an executable file
$ ./hello; echo $?      # returns decimal 42
42
```

To disassemble the binary code back to the assembly instructions in text,

``` sh
$ objdump -d hello

hello:     file format elf64-littleaarch64


Disassembly of section .text:

00000000000001ec <_start>:
 1ec:	d2800540 	mov	x0, #0x2a                  	// #42
 1f0:	d2800ba8 	mov	x8, #0x5d                  	// #93
 1f4:	d4000001 	svc	#0x0
```

For the same code on Apple Silicon, refer to [Intro to 64bit ARM Assembly](https://www.youtube.com/watch?v=3ixTKrE8lv8) by Nick Thompson.

## References
ARM assembly tutorial, @LaurieWired, [https://www.youtube.com/playlist?list=PLn_It163He32Ujm-l_czgEBhbJjOUgFhg](https://www.youtube.com/playlist?list=PLn_It163He32Ujm-l_czgEBhbJjOUgFhg)

Linux System Call Table, [https://www.chromium.org/chromium-os/developer-library/reference/linux-constants/syscalls/#arm64-64-bit](https://www.chromium.org/chromium-os/developer-library/reference/linux-constants/syscalls/#arm64-64-bit)

Intro to 64bit ARM Assembly: from basics to party tricks [https://www.youtube.com/watch?v=3ixTKrE8lv8](https://www.youtube.com/watch?v=3ixTKrE8lv8)
