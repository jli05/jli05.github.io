---
title: Use Popen as Context Manager Preferably
layout: post
---

It is preferred to use `Popen()` in Python `subprocess` lib as context manager, to properly close all running subprocesses. Otherwise subprocesses may still be running when the main process exits. According to the documentation,

> on exit, standard file descriptors are closed, and the process is waited for.

For example, to process on output of piped commands and be able to pre-emptively terminate the pipeline while properly closing all subprocesses:

```python
from subprocess import Popen, PIPE

with Popen(<cmd1>, stdout=PIPE, text=True) as p1:
    with Popen(<cmd2>, stdin=p1.stdout, stdout=PIPE, text=True) as p2:
        with Popen(<cmd3>, stdin=p2.stdout, stdout=PIPE, text=True) as p3:
            # process on p3.stdout

# outside the nested contexts all subprocesses have been terminated
```
