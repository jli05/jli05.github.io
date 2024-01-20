---
title: Python Socket send Flushing
layout: post
---

If one redirected stdin stdout to a TCP socket ([/2024/01/17/redirect-stdio-to-socket.html](/2024/01/17/redirect-stdio-to-socket.html)) for the Python code below as example that relies on receiving an EOF (end-of-file) to finish writing output:

`lower.py`:

```python
with open(0) as f:
    for line in f:
        print(line.lower(), end='')
```

Per the Python [Socket Programming HOWTO](https://docs.python.org/3/howto/sockets.html), use `socket.shutdown()` to send a 0 byte to signal the end-of-file, or to flush the `send`,

```python
>>> from socket import socket, AF_INET, SOCK_STREAM, SHUT_WR
>>> s = socket(AF_INET, SOCK_STREAM)
>>> s.connect(('<server_ip_address>', server_port))
>>> s.sendall(b'UI')
>>> s.shutdown(SHUT_WR)
>>> s.recv(2048)
b'ui'
```
