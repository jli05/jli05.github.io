---
title: Factor-Variable Graph Message Passing 
layout: post
---

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

![Factor-Variable Graph Sum-Product Algorithm]({{ site.url }}/assets/fac-var-graph-sum-product.png)

We normally start by initialising all variable-to-factor to a constant value, then first do factor-to-variable message update, followed by variable-to-factor message update.
