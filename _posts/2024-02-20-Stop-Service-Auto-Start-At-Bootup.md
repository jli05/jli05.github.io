---
title: Stop a Service Autostart at Bootup on systemd
layout: post
---

```sh
sudo systemctl disable crond.service
sudo systemctl list-unit-files --type=service
```
