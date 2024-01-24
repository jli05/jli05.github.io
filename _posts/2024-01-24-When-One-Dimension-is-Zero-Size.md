---
title: When one dimension of array is zero size
layout: post
---

```python
>>> from numpy import ones
>>> ones((5, 0))
array([], shape=(5, 0), dtype=float64)
>>> ones((5, 0)) @ ones((5, 0)).T
array([[0., 0., 0., 0., 0.],
       [0., 0., 0., 0., 0.],
       [0., 0., 0., 0., 0.],
       [0., 0., 0., 0., 0.],
       [0., 0., 0., 0., 0.]])
>>> ones((3, 1)) @ ones((3, 1)).T
array([[1., 1., 1.],
       [1., 1., 1.],
       [1., 1., 1.]])
```
