---
title: Start Plan 9 in qemu
layout: post
---

## Download the image
Download for example `9front-<build>.amd64.iso.gz` from [https://9front.org/iso/](https://9front.org/iso/), then decompress with

```sh
gzip -d 9front-<build>.amd64.iso.gz
```

## Start Plan 9 in qemu
By default the vnc in qemu listens on IPv6, but many home routers do not work on IPv6 yet. We re-configure vnc to listen on IPv4. For the `<ipv4>` below, the private IPv4 address (of the virtual machine in AWS) or `0.0.0.0` would suffice.

```sh
$ qemu-system-x86_64 -drive format=raw,file=9front-11091.amd64.iso -vnc <ipv4>:0,password=on -monitor stdio
QEMU 8.0.5 monitor - type 'help' for more information
(qemu) set_password vnc <password>
(qemu) q
```

The `<ipv4>:0` would map to port 5900, `<ipv4>:1` port 5901, so on and so forth.

With a VNC client, connect to `vnc://<public_ipv4>:5900`. The `<public_ipv4>` is the public IPv4 address of the machine running Plan 9. Type in the password configured in qemu monitor.
