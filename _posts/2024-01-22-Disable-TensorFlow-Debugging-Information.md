---
title: Disable TensorFlow debugging information
layout: post
---

Set `TF_CPP_MIN_LOG_LEVEL` environment variable as in

```sh
TF_CPP_MIN_LOG_LEVEL=3 python3 -m unittest ...
```
