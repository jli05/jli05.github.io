---
title: Stop a Service to Autostart at Bootup
layout: post
---

```sh
sudo systemctl disable crond.service
sudo systemctl list-unit-files --type=service
```
