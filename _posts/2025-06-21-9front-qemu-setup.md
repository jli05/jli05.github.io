---
title: Install Plan 9 in qemu
layout: post
---

In the [previous post](/2025/06/15/start-plan9-in-qemu.html) we booted the installation disk in qemu. But the installation disk was read-only. In this post we install Plan 9 to a new virtual disk so that it is writable. We will be able to save work on it.

1. Create a writeable qcow2 disk: `qemu-img create -f qcow2 disk.qcow2 1024m`. The installation may take up about 500 megabytes. 
2. Start qemu with the Plan 9 installation disk `qemu-system-x86_64 -hda disk.qcow2 -cdrom plan9.iso -smp 2 -m 1g`. As the default memory is quite small, we increase the number of CPU cores to 2 and memory to 1 gigabytes.
3. In Plan 9, type `inst/start` to start the installation. Refer to [https://www.youtube.com/watch?v=PjVpB3SpAfQ](https://www.youtube.com/watch?v=PjVpB3SpAfQ) for the process.
4. After installation, use Plan 9 with `qemu-system-x86_64 -hda disk.qcow2 -smp 2 -m 1g`.

As to the `-hda` and `-cdrom`, they will expand to `-drive` options. Refer to the qemu [system invocation](https://www.qemu.org/docs/master/system/invocation.html) manual for notes on the `-drive` option.

When installing Plan 9 remotely (inside some cloud), add '-vnc' option. Refer to the [previous post](/2025/06/15/start-plan9-in-qemu.html) for detail.

```sh
$ qemu-system-x86_64 -hda disk.qcow2 -smp 2 -m 1g -vnc 0.0.0.0:0,password=on -monitor stdio
QEMU 8.0.5 monitor - type 'help' for more information
(qemu) set_password vnc <password>
(qemu) q
```
