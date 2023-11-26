---
title: Alpine Linux busybox-extras
layout: post
---

Some internet utilities and daemons are moved into the `busybox-extras` package. After installing, one could check its content,

```sh
apk add busybox-extras
```

one could check its content

```sh
$ apk info busybox-extras
busybox-extras-1.36.1-r5 description:
Additional binaries of Busybox

busybox-extras-1.36.1-r5 webpage:
https://busybox.net/

busybox-extras-1.36.1-r5 installed size:
152 KiB
```

```sh
$ apk info -L busybox-extras
busybox-extras-1.36.1-r5 contains:
bin/busybox-extras
etc/busybox-paths.d/busybox-extras
```

so there is only executable file. We could list the functions therein

```sh
$ busybox-extras
...
Currently defined functions:
	arch, conspy, dnsd, dumpleases, fakeidentd, ftpd, ftpget, ftpput,
	httpd, inetd, readahead, tcpsvd, telnet, telnetd, tftp, tftpd, udhcpd
```


