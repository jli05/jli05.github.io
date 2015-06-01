---
title: A Simplest word2vec Model
layout: post
---

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

word2vec is recently patented by Google. Not sure what's the possible consequence of it.

<p>Given the context word/input word $w_I$, the probability for output word $w_j$ is
\[p\left(w_j|w_I\right)=y_j=\frac{\exp\left(u_j\right)}{\sum_{j'=1}^{V}\exp\left(u_{j'}\right)}=\frac{\exp\left({v'_{w_j}}^T v_{w_I}\right)}{\sum_{j'=1}^{V}\exp\left({v'_{w_{j'}}}^T v_{w_I}\right)},\]
where $v_{w_I}$ is the word vector for input word $w_I$, $v'_{w_j}$ is the word vector for output word $w_j$, $j\in[1,V]$, $V$ is the size of the vocabulary.</p>

<p>We maximise
\[\log y_{j^*}=u_{j^*}-\log\sum_{j'=1}^{V}\exp\left(u_{j'}\right):=-E.\]
By Chain Rule,
\begin{align}
\frac{\partial E}{\partial u_j}&=y_j-t_j:=e_j,\quad\text{ where }t_j=\mathbb{1}\left(j=j^*\right)\\
\frac{\partial E}{\partial w'_{ij}}&=\frac{\partial E}{\partial u_j}\cdot\frac{u_j}{\partial w'_{ij}}=e_j\cdot v_{w_i},
\end{align}
with which we learn the output word vectors. Similarly we could learn the input word vectors.</p>