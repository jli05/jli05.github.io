---
title: Uniqueness of CP Decomposition of Tensors 
layout: post
---

<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

## Uniqueness of CP Decomposition
From Sidiropoulos 2000, for an order-$d$ $n\times\cdots\times n$ tensor $T$, suppose there're $d$ $n\times f$ matrices $A$, $B$, ..., $\Phi$, $A=\begin{pmatrix}a_1&\ldots&a_f\end{pmatrix}$, ..., $\Phi=\begin{pmatrix}\phi_1&\ldots&\phi_f\end{pmatrix}$, such that 

$$T=\sum_{i=1}^f a_i\cdots\phi_i.$$

This decomposition is unique up to scaling and permutation of columns in $A$, ..., $\Phi$ if

$$k_A+\cdots+k_{\Phi}\ge 2f+(d-1),$$

where $k_{\cdot}$ is the Kruskal rank of a matrix.

As a special case, if the Kruskal ranks are all equal, then each one has to satisfy

$$k_{\cdot}\ge\frac{2}{d}f+\left(1-\frac{1}{d}\right).$$

## Kruskal Rank of Matrix
The Kruskal rank of $A$

$$k_A=\max_r\lbrace r: \text{every }r\text{ columns are linearly indepedent}\rbrace.$$

## References
Sidiropoulos, Nicolas D., and Rasmus Bro, On the uniqueness of multilinear decomposition of $N$-way arrays, 2000











