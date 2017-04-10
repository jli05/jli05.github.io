---
title: Neural Financial Market Factors
layout: post
---

<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

We could give (multi-) series of data to LSTM, at every step feeding the observations obtained for that step. An illustration of the architecture of LSTM is given below. (Courtesy: [https://colah.github.io](https://colah.github.io))

![LSTM](https://colah.github.io/posts/2015-08-Understanding-LSTMs/img/RNN-unrolled.png)

$h_t$ is the Hidden or System state at every moment $t$. The key idea is to minimise a certain distance metric 

$$\phi(Wh_t,W_{\text{port}}x_{t+1})$$

with certain regularisation.


