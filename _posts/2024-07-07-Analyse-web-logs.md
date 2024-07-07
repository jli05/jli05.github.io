---
title: Analyse web logs
layout: post
---

## Print the top queries
```sh
cat <web logs> | sed -n 's/.*GET \/?\([^ ]*\).*/\1/p' | egrep '(^|&)q=' | sort | uniq -c | sort -rk 1 | head
```
