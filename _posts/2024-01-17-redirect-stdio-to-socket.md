---
title: Redirect stdin stdout to TCP Socket
layout: post
---

[http://cs.baylor.edu/~donahoo/practical/CSockets/textcode.html](http://cs.baylor.edu/~donahoo/practical/CSockets/textcode.html)

Change `TCPEchoServer-Fork-SIGCHLD.c` line 48-49 from

```c
    HandleTCPClient(clntSock);
    exit(0);              /* Child process done */
```

to

```c
    dup2(clntSock, 0);
    dup2(clntSock, 1);
    execlp(...);
```
