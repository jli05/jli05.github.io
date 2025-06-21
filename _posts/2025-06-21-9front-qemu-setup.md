---
title: Install Plan 9 in qemu
layout: post
---

In the [previous post](/2025/06/15/start-plan9-in-qemu.html) we booted the installation disk in qemu. But the installation disk was read-only. In this post we install Plan 9 to a new virtual disk so that it is writable. We will be able to save work on it.

1. Create a qcow2 disk of a certain size: `qemu-img create -f qcow2 disk.qcow2 1024m`. The installation may take up about 500 megabytes. 
2. Boot with the Plan 9 installation disk `-drive format=raw,file=...` and the qcow2 disk attached `-drive format=qcow2,file=disk.qcow2`. Refer to the [previous post](/2025/06/15/start-plan9-in-qemu.html) for detail.
3. In Plan 9, type `inst/start` to start the installation. Refer to [https://www.youtube.com/watch?v=PjVpB3SpAfQ](https://www.youtube.com/watch?v=PjVpB3SpAfQ) for the process.
4. When installed, stop the virtual machine. Start qemu again with only the qcow2 disk `-drive format=qcow2,file=disk.qcow2` attached.

```sh
$ qemu-system-x86_64 -drive format=qcow2,file=disk.qcow2 -vnc 0.0.0.0:0,password=on -monitor stdio
QEMU 8.0.5 monitor - type 'help' for more information
(qemu) set_password vnc <password>
(qemu) q
```
