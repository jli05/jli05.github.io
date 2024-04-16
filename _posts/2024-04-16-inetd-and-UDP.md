---
title: inetd and UDP
layout: post
---

`inetd` is a daemon that conviently launches TCP/UDP services, redirecting internet data stream/grams to the stdio of user's code. For example, to configure a TCP service and a UDP service, `$HOME/etc/inetd.conf`:

```
21000 stream tcp nowait alpine python3 python3 /home/alpine/bin/lower.py
27000 dgram udp wait alpine python3 python3 /home/alpine/bin/udpclient.py
```

with `$HOME/bin/udpclient.py`:

```python

from socket import socket
from sys import stdin
import sqlite3

sock = socket(fileno=stdin.fileno())

received = sock.recv(1024).decode('utf8')
with sqlite3.connect('/home/alpine/fin.db') as conn:
	lines = [_.strip() for _ in received.split()]
	records = [_.split(',') for _ in lines]
	records = [(time, name, float(value))
		       for time, name, value in records]
	conn.executemany('INSERT INTO ts VALUES (?, ?, ?)', records)
```

and `$HOME/fin.db` created by a `create_db.sql`:

```sql
CREATE TABLE IF NOT EXISTS ts (
  time TEXT,
  name TEXT,
  value REAL,
  PRIMARY KEY(time, name)
);
```


Start `inetd` with

```sh
inetd etc/inetd.conf
```

then test the UDP service with

```sh
printf '2024-04-13,GS.US,450.3\n2024-04-13,AAPL.US,140.25\n'|timeout .001 nc -u 127.0.0.1 27000
```
