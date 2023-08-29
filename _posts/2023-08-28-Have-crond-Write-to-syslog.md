---
title: Have crond write to syslog
layout: post
---

```sh
sudo yum install cronie
sudo vim /lib/systemd/system/crond.service
```

then add the `-s` option to the `crond` start-up command in `crond.service`,

```
ExecStart=/usr/sbin/crond -n -s $CRONDARGS
```

Lastly back in the shell,

```sh
sudo systemctl daemon-reload
```
