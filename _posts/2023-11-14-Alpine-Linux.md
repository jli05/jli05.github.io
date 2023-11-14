---
title: Alpine Linux
layout: post
---

At the moment, cloud image is available for AWS. The ssh login user name is `alpine` by default.

Use `doas` for `sudo`, `apk` for `yum` or `apt` as software package manager.

```sh
doas apk update
doas apk upgrade
apk search
doas apk add
doas poweroff
doas reboot
```

The `crond` daemon need be started for the crontab service to be run,

```sh
rc-service crond status
doas rc-service crond start
```

but at reboot the last line above won't restart the `crond` service automatically. Do

```sh
doas rc-update add crond default
```

to schedule `crond` to be started at every reboot.

Do

```sh
rc-update
```

to check all scheduled services at various runlevels.
