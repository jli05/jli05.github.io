---
title: "Microarchitecture Security: The Spectre Affair"
layout: post
---

[https://riscv.org/blog/2024/08/featured-work-microarchitecture-security-the-spectre-affair/](https://riscv.org/blog/2024/08/featured-work-microarchitecture-security-the-spectre-affair/)

The code 

```c
if (a0 < max_len) {
  uint_32 s = data[a0];
  a0 = arr[s << 6];
}
```

The corresponding assembly code

```assembly
bge a0, max_len, end     # if (a0 < max_len)
add a0, a0, &data        # address of data[a0]
lw s, 0(a0)              # store data[a0] into s
slli s, s, 6             # shift left logical immediate s << 6
add s, s, &arr           # address of arr[s << 6]
lw a0, 0(s)              # store arr[s << 6] into a0
```

Reference: [RISC-V Assembly Language](/2023/09/28/RISC-V-Assembly-Language.html) 

When `a0` is out of bound, the secret in `data[a0]` may get written onto the cache when running the subsequent line `s << 6`.
