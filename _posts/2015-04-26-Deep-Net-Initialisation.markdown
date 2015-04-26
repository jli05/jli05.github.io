---
title: Deep Net Initialisation
layout: post
---

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

<ol><li>Initialise hidden layer biases to 0 and output (or reconstruction) biases to optimal value if weights were 0 (e.g. mean target or inverse sigmoid of mean target).</li>
<li>Initialise weights to $\text{Uniform}(-r,r)$, where
\[r=\sqrt{\frac{6}{\text{fan-in}+\text{fan-out}}}\]
for $\tanh$ units, and 4x bigger for sigmoid units (Glorot AISTATS 2010).</li></ol>
