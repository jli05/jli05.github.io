---
title: Some NumPy Floating Point Behaviour
layout: post
---

For

```python
>>> from numpy import seterr, sum, nan, inf
>>> seterr(invalid='print')
{'divide': 'warn', 'over': 'warn', 'under': 'ignore', 'invalid': 'warn'}
>>> sum([inf, inf])
inf
>>> sum([nan, inf])
nan
>>> sum([-inf, inf])
Warning: invalid value encountered in reduce
nan
>>> sum([nan, -inf, inf])
nan
>>> sum([nan, inf, -inf])
nan
>>> sum([-inf, inf])
Warning: invalid value encountered in reduce
nan
```

@rkern explained in [#25532](https://github.com/numpy/numpy/issues/25532):

> My language was a little loose. Our system relies on catching the [floating point exception](https://en.wikipedia.org/wiki/IEEE_754#Exception_handling) (n.b. not a Python exception) that is raised by the FPU and storing a flag to handle as the user configured via seterr/errstate once the ufunc is done with the whole array.

> NaNs come in two flavors, quiet QNaNs and signaling SNaNs. When an operation on two non-NaN values results in a NaN, the "invalid result" floating point exception is sent and a QNaN gets returned. When a QNaN operates with a non-NaN value, the result is another QNaN but no new floating point exception is sent. When an SNaN operates with a non-NaN value, the result is a NaN (of some flavor; SNaNs are rarely used, so I'm not going to look up what the standard says should happen) and a floating point exception is sent. The behavior you are seeing is consistent with this understanding of when floating point exceptions are issued.

> When I said "All we get is the notification that something in the ufunc's calculation created a NaN, somewhere", I meant in the usual non-NaN+non-NaN case. The QNaN+non-NaN case is an interesting side case. I didn't intend for my sentence to be exhaustive. There are some more side cases that I won't enumerate here (e.g. some operations on NaNs won't give NaNs, etc.).
