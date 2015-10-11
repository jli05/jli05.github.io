---
title: Context Free Grammars
layout: post
---

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

# Phrase Structure Grammars in NLP

<p>\[
G=(T,C,N,S,L,R)
\]</p>

where

- $T$ is a set of terminal symbols

- $C$ is a set of preterminal symbols

- $N$ is a set of nonterminal symbols

- $S$ is the start symbol ($S\in N$)

- $L$ is the lexicon, a set of items of the form $X\rightarrow x$, $X\in C$ and $x\in T$

- $R$ is the grammar, a set of items of the form $X\rightarrow \gamma$, $X\in N$ and $\gamma\in\left(N\cup C\right)$

Note

- $S$ is the start symbol, but in statistical NLP, we usually have an extra node at the top (ROOT, TOP)

# Probabilistic Context-Free Grammars
<p>\[
G=(T,N,S,R,P)
\]</p>
where

- $T$ is a set of terminal symbols

- $N$ is a set of nonterminal symbols

- $S$ is the start symbol ($S\in N$)

- $R$ is a set of rules/productions of the form $X\rightarrow\gamma$

- $P$ is a probability function such that

   - $P:R\rightarrow\[0,1\]$

   - $\forall X\in N$, $\sum_{X\rightarrow\gamma\in R}P\left(X\rightarrow\gamma\right)=1$

<p>A Grammar $G$ generates a language model $L$,
\[
\sum_{\gamma\in T*}P(\gamma)=1.
\]</p>
