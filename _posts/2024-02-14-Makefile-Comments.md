---
title: Makefile Comments
layout: post
---

Given the Makefile `tmp.mk`,

```makefile
.PHONY: all

# Project Summary
# ---------------

all:
	# display current time
	date
```

Note only comments in a recipe get echoed,

```
$ make -f tmp.mk 
# display current time
date
Wed Feb 14 14:12:11 UTC 2024
```

```
$ make -sf tmp.mk 
Wed Feb 14 14:12:20 UTC 2024
```
