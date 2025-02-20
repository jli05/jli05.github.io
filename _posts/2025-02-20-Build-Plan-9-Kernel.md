---
title: Build Plan 9 Kernel
layout: post
---

`/sys/src/9/pc` is the 32-bit kernel. `/sys/src/9/pc64` is the 64-bit kernel.

```sh
cd /sys/src/9/pc
bind -a -c /tmp .
mk
```
