---
title: Smoothing Text Counts
layout: post
---

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

# Stupid Backoff

Brants, et al, 2007, Large Language Models in Machine Translation

<p>For Web-scale N-grams, <em>Stupid Backoff</em> is defined as
\begin{split}
S\left(w_i|w_{i-k+1}^{i-1}\right)&=\begin{cases}
\frac{\text{count}\left(w_{i-k+1}^i\right)}{\text{count}\left(w_{i-k+1}^{i-1}\right)},&\text{if }\text{count}\left(w_{i-k+1}^{i-1}\right)>0\\
0.45 S\left(w_i|w_{i-k+2}^{i-1}\right),&\text{otherwise}
\end{cases}\\
S(w_i)&=\frac{\text{count}\left(w_i\right)}{N}
\end{split}
where $N$ is the total number of tokens, $w_j^i$ is the context from the $j$-th to the $i$-th words.
</p>

# Kneser-Ney Smoothing

For smaller corpus than web-scale, Kneser-Ney smoothing is regularly used.

<p>We first define the continuation count and continuation value of a word $w$, which corresponds to how varied its preceding context could be. The continuation count is 
\[
N(w)=\lvert\lbrace w_{i-1}:c\left(w_{i-1},w\right)>0\rbrace\rvert;
\]
We normalise the continuation count to get the continuation value
\[
P_{\text{continuation}}(w)=\frac{\lvert\lbrace w_{i-1}:c\left(w_{i-1},w\right)>0\rbrace\rvert}{\sum_{w'\in V}\lvert\lbrace w'_{i-1}:c\left(w'_{i-1},w'\right)>0\rbrace\rvert}
\]
with $V$ being the entire vocabulary.</p>

<p>Then the Kneser-Ney Smoothing is 
\[
P_{\text{KN}}\left(w_i|w_{i-1}\right)=\frac{\max\left(c\left(w_{i-1},w_i\right)-d,0\right)}{c\left(w_{i-1}\right)}+\lambda\left(w_{i-1}\right)P_{\text{continuation}}\left(w_i\right),
\]
where $d$ is a constant, $\lambda(w)$ is a normalising constant for the probability mass we discounted
\[
\lambda\left(w_{i-1}\right)=\frac{d}{c\left(w_{i-1}\right)}\lvert\lbrace w:c\left(w_{i-1},w\right)>0\rbrace\rvert.
\]</p>

<p>Now for higher order context, we apply recursively the formula
\[
P_{\text{KN}}\left(w_i|w_{i-n+1}^{i-1}\right)=\frac{\max\left(c_{\text{KN}}\left(w_{i-n+1}^i\right)-d,0\right)}{c_{\text{KN}}\left(w_{i-n+1}^{i-1}\right)}+\lambda\left(c_{i-n+1}^{i-1}\right)P_{\text{KN}}\left(w_i\vert w_{i-n+2}^{i-1}\right),
\]
where $c_{\text{KN}}\left(w_j^i\right)$ is the continuation count (the count of  unique word preceding the given words $w_j^i$). For high order of context $w_j^i$, we could use the exact count for $c_{\text{KN}}\left(w_j^i\right)$.</p>

For more details, refer to <http://mkoerner.de/media/oberseminar-2013-07-25.pdf>.


 