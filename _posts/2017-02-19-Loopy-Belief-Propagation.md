---
title: Loopy Belief Propagation 
layout: post
---

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

![Loopy Belief Propagation]({{ site.url }}/assets/loopy-belief-propagation.png)

At convergence,

$$p(x_f)\propto\phi_f(x_f)\prod_{l\in\mathcal{N}(f)}\delta_{l\rightarrow f}(x_f).$$

