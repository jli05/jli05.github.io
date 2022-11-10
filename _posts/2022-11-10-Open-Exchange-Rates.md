---
title: Open Exchange Rates End-of-Day Timestamps
layout: post
---

We can see [Open Exchange Rates](https://openexchangerates.org) switched to 00:00 UTC+0 timestamp for daily data since mid-2015.

```python
>>> datetime.fromtimestamp(1340035200)
datetime.datetime(2012, 6, 18, 17, 0)
>>> datetime.fromtimestamp(1341936000)
datetime.datetime(2012, 7, 10, 17, 0)
>>> datetime.fromtimestamp(1371592843)
datetime.datetime(2013, 6, 18, 23, 0, 43)
>>> >>> datetime.fromtimestamp(1434668408)
datetime.datetime(2015, 6, 19, 0, 0, 8)
>>> datetime.fromtimestamp(1623974397)
datetime.datetime(2021, 6, 18, 0, 59, 57)
```
