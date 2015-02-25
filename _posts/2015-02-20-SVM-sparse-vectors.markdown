---
title: SVM on Sparse Vectors
layout: post
---

tags: SVM, "sparse vectors"

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

<p>The objective function is
\[\min_{w}\frac{1}{2}\lVert w\rVert^2+C\sum_i L(X_i,y_i),\]
where $L(X_i,y_i)=\max\left\{0,1-y_i(w\cdot X_i+b)\right\}$.</p>

<p>By doing SGD, for a given $i$,
\[w'\leftarrow w-\eta\left(w+C\frac{\partial L(X_i,y_i)}{\partial w}\right).\]
A better way to do it is 
\begin{align}
w_1&\leftarrow w-\eta C\frac{\partial L(X_i,y_i)}{\partial w},\\
w_2&\leftarrow (1-\eta)w_1
\end{align}
We could see
\[w_2\leftarrow w-\eta\left(w+C\frac{\partial L(X_i,y_i)}{\partial w}\right)+\eta^2 C\frac{\partial L(X_i,y_i)}{\partial w}.\]
</p>
