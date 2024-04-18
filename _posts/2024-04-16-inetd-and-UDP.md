---
title: inetd and UDP
layout: post
---

`inetd` is a daemon that conviently launches TCP/UDP services upon incoming IP packets, redirecting the socket to the stdio (file descriptors 0, 1, 2) of specified user's code for launching the service. FreeBSD has a documentation page on `inetd`.

Let's see a few examples. Below the configuration file `$HOME/etc/inetd.conf` configures one TCP service and two UDP services,

```
21000 stream tcp nowait alpine python3 python3 /home/alpine/bin/lower.py
27000 dgram udp wait alpine python3 python3 /home/alpine/bin/udpclient.py
28000 dgram udp wait alpine python3 python3 /home/alpine/bin/udpclient2.py
```

Run in the shell

```sh
inetd etc/inetd.conf
```

to start the `inetd` daemon with the given configuration.

As configured, the first is a TCP service. The user code `$HOME/bin/lower.py` does standard I/O agnostic of the internet data:

```python
# convert each line in input to lower case

from sys import stdin

for line in stdin:
    print(line.lower(), end='')
```

One may test with

```sh
printf 'wEjLu\nUikO\n'|nc 127.0.0.1 21000
```

With UDP service, the UDP socket will be passed as user's stdin, but the user code cannot work as agnostically as the code behind the TCP service does. One has to recreate the UDP socket in order to do I/O.

The first `$HOME/bin/udpclient.py` receives tuple data as "2024-04-01,GS.US,58" on each line and insert the tuples into a database table:

```python

from socket import socket
from sys import stdin
import sqlite3

with socket(fileno=stdin.fileno()) as sock:
    # bytes to str
    received = sock.recv(1024).decode('utf8')

    # convert to records
    lines = [_.strip() for _ in received.split()]
    records = [_.split(',') for _ in lines]
    records = [(time, name, float(value))
                for time, name, value in records]

    # write to db
    with sqlite3.connect('/home/alpine/fin.db') as conn:
        conn.executemany('INSERT INTO ts VALUES (?, ?, ?)', records)
```

where `$HOME/fin.db` is created with schema:

```sql
CREATE TABLE IF NOT EXISTS ts (
  time TEXT,
  name TEXT,
  value REAL,
  PRIMARY KEY(time, name)
);
```

Test the UDP service with

```sh
printf '2024-04-13,GS.US,450.3\n2024-04-13,AAPL.US,140.25\n'|nc -u 127.0.0.1 27000
```

The second UDP service illustrates how to do output on UDP. Unlike the TCP service, simply `print()` won't work, one has to use `socket.sendto()` on UDP. Note unless `socket.connect()` had been called, a UDP socket can receive data from any peer thus has no fixed peer. Generally calling `socket.getpeername()` on an UDP socket would have an exception raised. But we can read out the peer's name (host and port) when receiving a particular message with `socket.recvfrom()`.

`$HOME/bin/udpclient2.py`:

```python

from socket import socket
from sys import stdin

with socket(fileno=stdin.fileno()) as sock:
    recv, peer = sock.recvfrom(1024)
    recv = recv.decode('utf8')
    sock.sendto(recv.upper().encode('utf8'), peer)
```

Check the system log for example `/var/log/messages`, `inetd` may write error messages from the services there.
