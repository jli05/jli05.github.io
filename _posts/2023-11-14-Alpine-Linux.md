---
title: Alpine Linux
layout: post
---

[Alpine Linux](https://www.alpinelinux.org) is a small and functional Linux distribution. Most shell commands are provided by [BusyBox](https://www.busybox.net/), as `ls`, `rm`, etc, but also `crond`, `sendmail`, `inetd`, etc.

Most documentation is on [https://wiki.alpinelinux.org/wiki/Main_Page](https://wiki.alpinelinux.org/wiki/Main_Page).

At the moment, [cloud image](https://www.alpinelinux.org/cloud/)  is available for AWS. The ssh login user name is `alpine` by default. After the first install on a t4g.nano instance with 2 vCPUs, 0.5 GB memory, 390 MB memory is free for use, in contrast to the installation of Amazon Linux 2023 on the same instance, which leaves 300 MB memory free for use.

Use `doas` for `sudo`, `apk` for `yum` or `apt` as software package manager.

```sh
doas apk update
doas apk upgrade
apk search
doas apk add
```

```sh
doas poweroff
doas reboot
```

The background services (aka init system) are managed by [OpenRC](https://github.com/OpenRC/openrc). For example, to start the `crond` daemon for the crontab service,

```sh
rc-service crond status
doas rc-service crond start
```

but at reboot the last line above won't restart the `crond` service automatically. Do

```sh
doas rc-update add crond default
```

to schedule `crond` to be started at every reboot; or simply

```sh
rc-update
```

to check all scheduled services at various runlevels.
