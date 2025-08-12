---
title: ARM64 floating point arithmetic
layout: post
---

## Project Overview
In previous posts, normally at last of execution we ask the Linux `exit()` to return an integer result. But in this post we'll do pointing point arithmetic, so we have to change the game plan -- we'll use the C function `printf()` to print out the floating number.

We write out `add.h` and `main.c`:

`add.h`:

```c
float add(float a, float b);
```

`main.c`:

```c
#include "add.h"
#include <stdio.h>

int main() {
    printf("%f\n", add(3.4, -2.7));
    return 0;
}
```

what remains is to write an `add.s`, and eventually link it with `main.c` to produce the executable file.

## The adding function in assembly
Recall that all constants are preceded by `#`. `0x` denotes heximal, otherwise decimal. So `#0x10` is the heximal 10, equivalent to decimal 16. `#12` is just the decimal 12.

`add.s`:

```asm
.global add

.text
add:
    sub sp, sp, #0x10       // set sp to sp - 16 bytes
    str s0, [sp, #12]       // store s0 first param at sp + 12 bytes
    str s1, [sp, #8]        // store s1 second param at sp + 8 bytes
    sub sp, sp, #0x10       // set sp to sp - 16 bytes once more
    str lr, [sp, #8]        // store link register lr at sp + 8 bytes
    str fp, [sp]            // store frame pointer fp at sp
    mov fp, sp              // set frame pointer fp to sp

    fadd s0, s0, s1         // floating-point add
                            // store result in s0

    ldr fp, [sp]            // restore frame pointer fp from sp
    ldr lr, [sp, #8]        // restore link register lr from sp + 8 bytes
    add sp, sp, #0x10       // set sp to sp + 16 bytes
                            // now, the params s0, s1 at function entry
                            // should be stored at sp + 8 bytes, sp + 16 bytes
                            // but we don't restore them
    add sp, sp, #0x10       // set sp to sp + 16 bytes once more
                            // fully unwind stack used by this function
    ret
```

### Floating point registers
64-bit floating point registers are named `d0`, `d1`, ... Their lower 32-bit parts are named `s0`, `s1`, ...

The C `float` type is typically 32-bit, or occupying 4 bytes. That's why in the code above, the sp + 8 bytes to sp + 15 bytes on the stack are used to store the two parameters at entry `s0` and `s1`, while leaving sp to sp + 7 bytes empty:

```asm

    sub sp, sp, #0x10       // set sp to sp - 16 bytes
    str s0, [sp, #12]       // store s0 first param at sp + 12 bytes
    str s1, [sp, #8]        // store s1 second param at sp + 8 bytes

```

### Stack use
The stack grows downward: its pointer's address decreases as more data is pushed onto it. Before the `fadd` floating point operation, the stack would have been prepared and look like

```
s0      4 bytes
s1      4 bytes
[empty 8 bytes]
lr      8 bytes
old fp  8 bytes    <-- sp
```

In the link register lr is stored the return address for the `add()` function: somewhere inside the `main()` function.

The frame pointer fp momentarily points to the current stack pointer sp during the life of the `fadd` operation, before being restored to the old frame pointer, in preparation for returning to the `main()` function.

## Make the code run
We assemble the `add.s` into a binary file `add.o`:

```sh
$ as -o add.o add.s
```

then compile the remaing C files, and link with `add.o` to produce the final executable file `main`:

```sh
$ gcc -o main main.c add.o
```

We run it:

```sh
$ ./main
0.700000
```

## References
Arm Compiler armasm User Guide. On [https://developer.arm.com](https://developer.arm.com), search for "armasm user guide". In the result list, find the latest version of "Arm Compiler armasm User Guide".

ARM64 function calls, [/2025/08/04/arm64-func.html](/2025/08/04/arm64-func.html).

Introduction to Aarch64 architecture, 8. The Stack, [https://hrishim.github.io/llvl_prog1_book/stack.html](https://hrishim.github.io/llvl_prog1_book/stack.html).
