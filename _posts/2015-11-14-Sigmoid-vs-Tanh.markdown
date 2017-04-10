---
title: Sigmoid vs Tanh
layout: post
---

<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

# Logistic (Sigmoid)
<p>\[
\sigma(x)=\frac{1}{1+\exp(-x)}\in(0,1),
\] 
and $\sigma'(x)=\sigma(x)\left(1-\sigma(x)\right)$.</p>

# Tanh
<p>\[
\tanh(x)=\frac{e^x-e^{-x}}{e^x+e^{-x}}\in(-1,1),
\]
and $\tanh'(x)=1-\tanh^2(x)$.</p>

<p>Tanh is just rescaled and shifted sigmoid,
\[
\tanh(x)=2\sigma(2x)-1.
\]</p>
