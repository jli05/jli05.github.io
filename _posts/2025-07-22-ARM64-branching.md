---
title: ARM64 branching (jump) instruction
layout: post
---

This article accompanies Lesson 7 CMP, Lesson 8 Branching of the [ARM64 assembly tutorial](https://www.youtube.com/playlist?list=PLn_It163He32Ujm-l_czgEBhbJjOUgFhg) of LaurieWired.

ARM64 is quite similar to ARM32 in branching instructions. The [AArch64 control transfer](https://devblogs.microsoft.com/oldnewthing/20220815-00/?p=106975) blog post gave a full summary of the branching instructions in ARM64.

## Compare numbers
`branch.s`:

```asm
.global _start

.text
_start:
    mov x2, #4
    mov x3, #5
    cmp x2, x3
    bgt label1    // if x2 greater than x3, go to label1
    ble label2    // if x2 less than or equal to x3, go to label2

label1:
    mov x0, #1
    mov x8, #0x5d
    svc #0

label2:
    mov x0, #2
    mov x8, #0x5d
    svc #0
``` 

As `x2` = 4 < `x3` = 5, we can see the execution jumped to `label2`,

```sh
$ as -o branch.o branch.s
$ gcc -o branch branch.o -nostdlib -static
$ ./branch; echo $?
2
```

## Branching on result of logical operations
Other than arithmetically comparing numbers, we can perform AND, ORR, EOR logical operations between them, and branch (jump) based on the result.

`branch2.s`:

```asm
.global _start

.text
_start:
    mov x2, #0xa          // 1010 in binary
    mov x3, #0x5          // 0101 in binary
    and x1, x2, x3        // x1 would be 0000 in binary
    cbnz x1, label2       // if x1 is not zero, go to label2

label1:
    mov x0, #1
    mov x8, #0x5d
    svc #0

label2:
    mov x0, #2
    mov x8, #0x5d
    svc #0
```

As `x1` would be full zero after the AND operation, it would go straight on to execute the code at `label1`.

```sh
$ as -o branch2.o branch2.s
$ gcc -o branch2 branch2.o -nostdlib -static
$ ./branch2; echo $?
1
```

## References
ARM64 assembly tutorial, LaurieWired, [https://www.youtube.com/playlist?list=PLn_It163He32Ujm-l_czgEBhbJjOUgFhg](https://www.youtube.com/playlist?list=PLn_It163He32Ujm-l_czgEBhbJjOUgFhg).

AArch64 control transfer, Raymond Chen, [https://devblogs.microsoft.com/oldnewthing/20220815-00/?p=106975](https://devblogs.microsoft.com/oldnewthing/20220815-00/?p=106975).
