---
title: Learn Time Series with LSTM 
layout: post
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

We tested if a generic model as LSTM can learn time series and its latent state. We found that we need to model the full distribution of the time series; and if the latent state is low-variance, we need to feed surrogate to the latent state to the fitting procedure.

[https://github.com/jli05/CS229-TimeSeries-LSTM](https://github.com/jli05/CS229-TimeSeries-LSTM)

