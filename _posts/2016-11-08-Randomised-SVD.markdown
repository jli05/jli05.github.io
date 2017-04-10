---
title: Randomised SVD 
layout: post
---

<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

## Krylov Method
If one is going to do SVD on a matrix $A\in\mathbb{R}^{m\times n}$ to discover the top-$k$ eigenvalues and left eigenvectors, where both $m$ and $n$ are very big, it will be impossible to do it on a single machine.

One way to do it is to multiply it by a random Gaussian matrix $\Pi=\mathcal{N}(0,1)^{n\times k}$, if we do QR decomposition on $A\Pi$, the $m\times k$ Q matrix are approximately the top-$k$ left eigenvectors of $A$. When the gaps between the eigenvalues are small, we could perform QR decomposition on $\left(AA^{\top}\right)^q A\Pi$ to obtain $Q$.

Once we have $Q$, we do eigendecomposition on $Q^{\top}AA^{\top}Q$,

$$Q^{\top}AA^{\top}Q=USU^{\top},$$

and return the $QU$ as the top-$k$ left eigenvectors of $A$, $S^{1/2}$ as the associated eigenvalues.

## Convergence
Halko & Tropp pioneered a thorough analysis of the convergence by expectation. Musco & Musco proved the PAC bound.

The complexity is $O\left(k^2 n\right)$ and it takes $q\sim O\left(\log\frac{n}{\epsilon}\right)$ iterations to bound the error with 99/100 probability,

$$\lVert A-ZZ^{\top}A\rVert_{\text{norm}}\le(1+\epsilon)\lVert A-A_k\rVert_{\text{norm}},$$

where $A_k$ is the projection of $A$ onto the space spanned by its top $k$ eigenvectors, $\lVert\cdot\rVert_{\text{norm}}$ is either the Frobenius norm or Spectral norm.

## Parallelization
We could distribute the $m$ rows of $A$ over a cluster and perform the multiplication of each row with the test matrix. After we collect the $m\times k$ result matrix, normally the QR decomposotion could be performed on a single machine.

## Improved Stability for the Power Step
After we multiply by the random Gaussian matrix $A\Pi$ we could do QR decomposition on it to get the Q basis matrix $Q^{(0)}$.

Then at each $i$-th power step, $i=1,2,\ldots$,  we could first compute $\left(AA^{\top}\right)Q^{(i-1)}$, do the QR decomposition on the result to get the Q basis matrix $Q^{(i)}$ and use it for the next step.

## References
Halko, Nathan, Per-Gunnar Martinsson, Joel A. Tropp, Finding structure with randomness: Probabilistic algorithms for constructing approximate matrix decompositions, 2010

Musco, Cameron, Christopher Musco, Randomized Block Krylov Methods for Stronger and Faster Approximate Singular Value Decomposition, 2015 
