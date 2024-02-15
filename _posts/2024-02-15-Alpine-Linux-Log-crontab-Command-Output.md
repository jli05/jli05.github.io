---
title: Alpine Linux Log crontab Command Output
layout: post
---

Unlike [cronie-crond](https://github.com/cronie-crond/cronie), the Alpine Linux `crond` crontab daemon won't log output of commands in crontab. One way to get around this is to do instead

```crontab
* * * * * sh -c "cmd 2>&1 | sed -e 's/^/CMDOUT (/' -e 's/$/)/' | logger -t CROND[$(cat /run/crond.pid)]"
```
