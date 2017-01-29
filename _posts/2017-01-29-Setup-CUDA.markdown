---
title: Setup CUDA 8.0 on Ubuntu 16.04 LTS
layout: post
---

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

We no longer need to install the Ubuntu package `linux-generic` for the `drm.ko`. We could simply install CUDA 8.0 via either the Network or Local method, then export the `PATH` and `LD_LIBRARY_PATH` in `~/.profile`. 
