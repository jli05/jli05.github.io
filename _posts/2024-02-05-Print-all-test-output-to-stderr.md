---
title: "Print all test output to stderr, success or not"
layout: post
---

Within a project directory, given `tests/test_tmp.py`,

```python
from unittest import TestCase

class TestTmp(TestCase):
    def test(self):
        self.assertEqual(4, 4)
```

in shell we redirect `stdout` output to `/dev/null`, we will find effectively all output has been printed to `stderr`, even in case of success.

```sh
% python3 -m unittest tests/test_tmp.py 1>/dev/null; echo $?
.
----------------------------------------------------------------------
Ran 1 test in 0.000s

OK
0
```

If we changes the assert line to

```python
    ...
        self.assertEqual(5, 4)
    ...
```

again all output has been written to `stderr`.

```sh
% python3 -m unittest tests/test_tmp.py 1>/dev/null; echo $?
F
======================================================================
FAIL: test (tests.test_tmp.TestTmp)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/Users/jencirlee/src/zeta/tests/test_tmp.py", line 6, in test
    self.assertEqual(5, 4)
AssertionError: 5 != 4

----------------------------------------------------------------------
Ran 1 test in 0.000s

FAILED (failures=1)
1
```
