---
title: Clean generated data files
layout: post
---

If we gradually clean out data files more than one month old, first delete the old files, then delete directories left empty after the first step.

```sh
find $HOME/$DIR -mtime +31 -type f -delete; find $HOME/$DIR -type d -empty -delete
```

One could put it in crontab, for example,

```
@weekly find $HOME/$DIR -mtime +31 -type f -delete; find $HOME/$DIR -type d -empty -delete
```
