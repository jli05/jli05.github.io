---
title: RISC-V Assembly Language
layout: post
---

Registers for 32-bit RISC-V:

| Register Name   | Register Number   |            Use               |
| --------------- | ----------------- | ---------------------------- |
| `zero`          | `x0`              | Constant value 0             |
| `ra`            | `x1`              | Return address               |
| `sp`            | `x2`              | Stack pointer                |
| `gp`            | `x3`              | Global pointer               |
| `tp`            | `x4`              | Thread pointer               |
| `t0-2`          | `x5-7`            | Temporary variables          |
| `s0/fp`         | `x8`              | Saved register / Frame pointer |
| `s1`            | `x9`              | Saved register               |
| `a0-1`          | `x10-11`          | Function arguments / Return values |
| `a2-7`          | `x12-17`          | Function arguments           |
| `s2-11`         | `x18-27`          | Saved registers              |
| `t3-6`          | `x28-31`          | Temporary variables          |




Below are the unpriviledged assembly instructions for 32-bit RISC-V, in reference to [https://github.com/riscv/riscv-isa-manual](https://github.com/riscv/riscv-isa-manual) "Unpriviledged ISA Manual".

|  RISC-V Assembly      |  Description         |    Operation        |
| --------------------- | -------------------- | ------------------- |
| `add s0, s1, s2`      | Add                  | `s0 = s1 + s2`      |
| `sub s0, s1, s2`      | Subtract             | `s0 = s1 - s2`      |
| `addi t3, t1, -10`    | Add immediate        | `t3 = t1 - 10`      |
| `mul t0, t2, t3`      | 32-bit multiply      | `t0 = t2 * t3`      |
| `div s9, t5, t6`      | Division             | `s9 = t5 / t6`      |
| `rem s4, s1, s2`      | Reminder             | `s4 = s1 % s2`      |
| `and t0, t1, t2`      | Bit-wise AND         | `t0 = t1 & t2`      |
| `or t0, t1, t5`       | Bit-wise OR          | `t0 = t1 | t5`      |
| `xor s3, s4, s5`      | Bit-wise XOR         | `s3 = s4 ^ s5`      |
| `andi t1, t2, 0xFF`   | Bit-wise AND immediate | `t1 = t2 & 0xFF`  |
| `ori t0, t1, 0x2C`    | Bit-wise OR immediate | `t0 = t1 | 0x2C`   |
| `xori s3, s4, 0xAB`   | Bit-wise XOR immediate | `s3 = s4 ^ 0xAB`  |
| `sll t0, t1, t2`      | Shift left logical   | `t0 = t1 << t2`     |
| `srl t0, t1, t5`      | Shift right logical  | `t0 = t1 >> t5`     |
| `sra s3, s4, s5`      | Shift right arithmetic | `s3 = s4 >>> s5`  |
| `slli t1, t2, 30`     | Shift left logical immediate | `t1 = t2 << 30` |
| `srli t0, t1, 5`      | Shift right logical immediate | `t0 = t1 >> 5` |
| `srai s3, s4, 31`     | Shift right arithmetic immediate | `s3 = s4 >>> 31` |
| `lw s7, 0x2C(t1)`     | Load word            | `s7 = memory[t1 + 0x2C]` |
| `lh s5, 0x5A(s3)`     | Load half-word       | `s5 = SignExt(memory[s3 + 0x5A]_{15:0})` |
| `lb s1, -3(t4)`       | Load byte            | `s1 = SignExt(memory[t4 - 3]_{7:0})` |
| `sw t2, 0x7C(t1)`     | Store word           | `memory[t1 + 0xuC] = t2` |
| `sh t3, 22(s3)`       | Store half-word      | `memory[s3 + 22]_{15:0} = t3_{15:0}` |
| `sb t4, 5(s4)`        | Store byte           | `memory[s4 + 5]_{7:0} = t4_{7:0}` |
| `beq s1, s2, L1`      | Branch if equal      | `if (s1 == s2), PC = L1` |
| `bne t3, t4, Loop`    | Branch if not equal  | `if (s1 != s2), PC = Loop` |
| `blt t4, t5, L3`      | Branch if less than  | `if (t4 < t5), PC = L3` |
| `bge s8, s9, Done`    | Branch if greater than or equal | `if (s8 >= s9), PC = Done` |
| `li s1, 0xABCDEF12`   | Load immediate       | `s1 = 0xABCDEF12`    |
| `la s1, A`            | Load address         | `s1 = Memory address where variable A is stored` |
| `nop`                 | No operation         | No operation         |
| `mv s3, s7`           | Move                 | `s3 = s7`            |
| `not t1, t2`          | Not (invert)         | `t1 = ~t2`           |
| `neg s1, s3`          | Negate               | `s1 = -s3`           |
| `j Label`             | Jump                 | `PC = Label`         |
| `jal L7`              | Jump and link        | `PC = L7; ra = PC + 4` |
| `jr s1`               | Jump register        | `PC = s1`            |
