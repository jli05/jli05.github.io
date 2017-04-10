---
title: Machine Learning on Financial Time Series, Part II 
layout: post
---

<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

## Introduction 
The code is at [https://github.com/jli05/MacroTimeSeries](https://github.com/jli05/MacroTimeSeries).

## LSTM
The train/validation data is obtained as for the Denoising Auto-Encoder. We train a simple LSTM with 5 units for the c and h cells respectively. At each time step, we do linear transform of the h cell content to predict next day's observation. The loss is measured by the MSRE overall.

We can observe that the MSRE is roughly the same as for the Auto-Encoder.
