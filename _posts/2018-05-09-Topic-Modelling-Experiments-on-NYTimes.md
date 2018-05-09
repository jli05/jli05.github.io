---
title: Topic Modelling Experiments on NYTimes Corpus 
layout: post
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

# Introduction
On a full scale corpus, simple formulation for Topic Modelling as Non-negative Matrix Decomposition no longer renders satisfactory results; we have to use more flexible formulation as the Latent Dirichlet Allocation. We compare the results from two main algorithms for LDA:
* Spectral LDA <https://github.com/Mega-DatA-Lab/SpectralLDA-Spark>
* Online Variational "Online Learning for Latent Dirichlet Allocation", NIPS 2010

# Dataset
The data is from the [UCI Bag of Words NYTimes dataset](https://archive.ics.uci.edu/ml/datasets/bag+of+words).

Vocabulary Size    |  200000
----------------------------
Number of Articles |  299650


Quantiles for levels (.05, .25, .5, .75, .95):

Number of Distinct Tokens per Article  |  (1.0, 181.0, 237.0, 1117.0, 1778.0)
--------------------------------------------
Document Length (Total Number of Tokens per Article) | (1.0, 217.0, 308.0, 1554.0, 3586.0)

# Model Parameters
We use non-informative priors.

Number of Topics (k)  | 10
-----------------------------
All $$\alpha_i$$ for prior Topic Distribution      | 1.0
-----------------------------
All $$\beta_{ij}$$ for prior Topic Word Distribution | 1.0

For the Online Variational algorithm, the minibatch size is as large as possible to fully utilise 8-cores on an AWS Instance. It runs 10 full iterations over the corpus.



