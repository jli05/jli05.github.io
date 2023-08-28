---
title: Have crond write to syslog
layout: post
---

```sh
sudo yum install cronie
sudo vim /lib/systemd/system/crond.service
```

then add `-s` to the line of `crond` in `crond.service`,

```
ExecStart=/usr/sbin/crond -n -s $CRONDARGS
```

Lastly,

```sh
sudo systemctl daemon-reload
```
