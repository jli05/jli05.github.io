---
title: Solve X A = B on CUDA for Positive Semi-Definite A
layout: post
---

<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

If we're going to solve 

$$AX=B$$ 

with $A$, $B$ both dense, it is straightforward to call `cusolverDn<t>potrf()` and `cusolverDn<t>potrs()`. However what if we're going to solve 

$$XA=B,$$ 

where $X,B\in\mathbb{R}^{m\times n}$, $A\in\mathbb{R}^{n\times n}$?

We could first call `cusolverDn<t>potrf()` to do the Cholesky decomposition such that 

$$A=U^{\dagger}U,$$

then call the BLAS-3 function `cublas<t>trsm()` twice, the first time to solve

$$X' U = B,$$

the second time

$$X U^{\dagger} = X'.$$

We could find example code below.

<script src="https://gist.github.com/jli05/e045d2a2b2dadfef1abeee89ac9e8145.js"></script>
