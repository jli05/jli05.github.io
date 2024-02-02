---
title: Python3 `sqlite3` module
layout: post
---

If the result is empty, it returns empty list; but if it is not empty, it returns list of tuple.

`pandas` `read_sql()` returns a DataFrame in any case.

```python
>>> import sqlite3
>>> with sqlite3.connect('empty.db') as conn:
...     r = conn.execute('select * from tab limit 1').fetchall()
... 
>>> r
[]
>>> with sqlite3.connect('non_empty.db') as conn:
...     r = conn.execute('select date from tab limit 1').fetchall()
... 
>>> r
[('1997-07-02',)]
```
