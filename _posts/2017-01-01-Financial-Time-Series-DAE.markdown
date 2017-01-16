---
title: Machine Learning on Financial Time Series 
layout: post
---

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

## Introduction 
The code is at [https://github.com/jli05/MacroTimeSeries](https://github.com/jli05/MacroTimeSeries).

We extract the following financial time series from Quandl:

```text
['YAHOO/INDEX_GSPC - Close', 'YAHOO/INDEX_DJI - Close',
 'YAHOO/INDEX_IXIC - Close', 'FRED/DCOILBRENTEU - VALUE',
 'FRED/DCOILWTICO - VALUE', 'FRED/GOLDPMGBD228NLBM - VALUE',
 'FRED/DEXUSEU - VALUE', 'FRED/DEXUSUK - VALUE', 'FRED/DEXJPUS - VALUE',
 'FRED/DGS10 - VALUE', 'FRED/DFF - VALUE', 'CBOE/VIX - VIX Close',
 'CHRIS/ICE_TF1 - Settle', 'YAHOO/INDEX_N225 - Close']
```

We only work on levels of the prices/values, not the increments. We convert each series into the percentile, thus each series will take value between [0,1]. We're going to model the multi-variate series by Denoising Auto-Encoder and LSTM.

## Auto-Encoder
We cut the series by year. For example, there're 230 observations for year 2010. We divide it into two parts: the first 170 obs. for training, the last 60 obs. for test. During training, we always randomly select a series of 60 consecutive observations, flatten it, then feed it through the Denoising Auto-Encoder.

The Denoising Auto-Encoder first expands the input vector into 2000 features, then compress into 30 features in the middle layer, expands back into 2000 features, finally reconstruct the input in the final layer. We use tanh for the non-linear transform and RMSE for the error metric.

The test loss usually settles at 0.1 except for year 2014, which settles at 0.15. If we look at the chart, the year end of 2014 underwent some visibly unusual change.

