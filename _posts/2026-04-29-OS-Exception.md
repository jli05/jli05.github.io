---
title: "OS in 1,000 lines: Exception in RISC-V64"
layout: post
---

This is a companion note to [OS in 1,000 lines: Exception](https://operating-system-in-1000-lines.vercel.app/en/08-exception) originally for RISC-V32.

When `kernel_entry` calls `handle_trap`, `a0` would be the first parameter to `handle_trap`, which would be interpreted as a pointer to a `trap_frame` pointer.

In RISC-V64, `kernel_entry` should still be aligned to 4 bytes. From [https://github.com/riscv/riscv-isa-manual](https://github.com/riscv/riscv-isa-manual), click on the snapshot link to the priviledged instructions, then go to the section "Supervisor Trap Vector Base Address (stvec) Register" under the chapter "Supervisor-level ISA".

When the program faults, the `sepc` points to the line

```c
   __asm__ __volatile__("unimp");
```

which may not be 4-byte aligned, but if one disassembles `kernel.elf` with

```sh
llvm-objdump -d kernel.elf
```

the address of `kernel_entry` the exception handler is indeed 4-byte aligned.
