---
title: RISC-V Assembly Instructions
layout: post
---

In reference to [https://github.com/riscv/riscv-isa-manual](https://github.com/riscv/riscv-isa-manual) "Unpriviledged ISA Manual",

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


