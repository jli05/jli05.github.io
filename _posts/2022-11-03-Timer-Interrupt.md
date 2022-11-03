---
title: Timer Interrupt
layout: post
---

> On a multi-CPU machine, does each CPU have its own timer, or does the
whole system have one only timer?

For RISC-V CPUs (and probably most other types of CPU), each CPU has its
own timer generating interrupts for just that CPU.

> When the timer(s) interrupt, could you give again (very roughly) the
sequence of function calls in function of the current Supervisor/User
mode on each CPU?

For xv6, a RISC-V timer interrupt on a CPU is first delivered in machine
mode to timervec (in kernel/kernelvec.S). timervec asks the CPU to
produce a "software interrupt" and then returns from the timer
interrupt. The CPU then delivers the software interrupt to supervisor
mode in the same way as other traps (system calls, device interrupts,
and exceptions). Thus, if the CPU was in user mode at the time of the
software interrupt, the CPU delivers the interrupt to the trapoline
code; if in supervisor mode, to kernelvec.

[MIT 6.1810 Operating System Engineering](https://pdos.csail.mit.edu/6.1810/)
