---
title: Backprop on CP Decomposition 
layout: post
---

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

# CANDECOMP/PARAFAC (CP) Decomposition
The CP decomposition factorises a tensor into a sum of component rank-one tensors. For example, given a third-order tensor $\mathcal{X}\in\mathbb{R}^{I\times J\times K}$, it could be written as

$$\mathcal{X}=\left[\Lambda;A,B,C\right]=\sum_{r=1}^R\lambda_r a_r\circ b_r\circ c_r,$$

where $R$ is the number of rank, $\lambda_r\in\mathbb{R}$, $a_r\in\mathbb{R}^I$, $b_r\in\mathbb{R}\in\mathbb{R}^J$, $c_r\in\mathbb{R}^K$ for $r=1,\ldots,R$. $\Lambda=\begin{pmatrix}\lambda_1 & \cdots & \lambda_R\end{pmatrix}$, $A=\begin{bmatrix}a_1 & \cdots & a_R\end{bmatrix}$, likewise for $B$ and $C$. The $L_2$-norms of each column of $A$, $B$, $C$ are all 1.

If we unfold tensor $\mathcal{X}$,

$$X_{(1)}=A\text{diag}(\Lambda)\left(C\odot B\right)^{\top}.$$

Therefore we solve

$$\hat{A}=\mathcal{X}_{(1)}\left[\left(C\odot B\right)^{\top}\right]^{\dagger}=\mathcal{X}_{(1)}\left(C\odot B\right)\left(C^{\top}C*B^{\top}B\right)^{\dagger},$$

normalise each column in $\hat{A}$ and cycle between $A$, $B$, $C$.

# Backprop on Vector Normalisation
In the last equation each column of the result $\hat{A}$ is normalised into $A$, the norms of those columns enter $\Lambda$. The essential question is therefore given a vector $a$, denote its norm $\sigma=\sqrt{a^{\top}a}$, normalised vector $\tilde{a}=\frac{a}{\sigma}$, how to compute $d\sigma$ and $d\tilde{a}$?

From $\sigma+d\sigma=\sqrt{(a+da)^{\top}(a+da)}$, we could deduce that

$$\frac{d\sigma}{\sigma}=\frac{a^{\top}da}{a^{\top}a},$$

$$d\sigma=\tilde{a}^{\top}da.$$

From $\tilde{a}+d\tilde{a}=\frac{a+da}{\sigma+d\sigma}$, we could obtain (by keeping the first-order terms related to $da$)

$$\tilde{a}+d\tilde{a}=\frac{a+da}{\sigma}\left(1-\frac{d\sigma}{\sigma}\right)=\tilde{a}+\frac{da}{\sigma}-\frac{\tilde{a}\tilde{a}^{\top}}{\sigma}da,$$

$$d\tilde{a}=\left(I-\tilde{a}\tilde{a}^{\top}\right)\frac{da}{\sigma}.$$


