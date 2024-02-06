---
title: Per-user crontab PATH very restrictive
layout: post
---

With cronie ([https://github.com/cronie-crond/cronie](https://github.com/cronie-crond/cronie)) as daemon, 

In the per-user crontab, `HOME` is set to the user's home directory, `PATH` is set to `/usr/bin:/bin` by default.
