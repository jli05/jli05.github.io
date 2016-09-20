---
title: Latent Dirichlet Allocation (LDA) for Topic Modelling 
layout: post
---

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

# The Generative Model for Topic Modelling
Given a prior $\alpha\in\mathbb{R}^k$ for topic distribution, $\beta=\left(\beta_1,\ldots,\beta_k\right)\in\mathbb{R}^{V\times k}$ for the word distributions per topic, where $V$ is the vocabulary size, $k$ is the number of topics, we define the following process for the generation of term count vector $c_d\in\mathbb{R}^V$ for any document $d$:

we first draw a sample $\theta_d\sim\text{Dir}(\alpha)$ for the topic distribution for document $d$, then

$$c_d\sim\text{Multi}\left(\beta\theta_d\right),$$

where $\text{Dir}(\cdot)$ is the Dirichlet distribution, $\text{Multi}(\cdot)$ is the multinomial distribution. In the Blei paper, there's an additional generative process for the word distributions $\beta_i\sim\text{Dir}(\eta_i)$, where $\eta_i\in\mathbb{R}^{V}$, $1\le i\le k$.

# Tensor LDA Model
Given the hierarchical generative model, we could form empirical word counts $\mathbf{E}\left[x_1\right]$, word pair counts $\mathbf{E}\left[x_1\otimes x_2\right]$, word triplet counts $\mathbf{E}\left[x_1\otimes x_2\otimes x_3\right]$. Furthermore, Anandkumar et al. discovered that if we denote 

$$\begin{align}
&M_1=\mathbf{E}\left[x_1\right],\\
&M_2=\mathbf{E}\left[x_1\otimes x_2\right]-\frac{\alpha_0}{\alpha_0+1}M_1\otimes M_1,\\
&\begin{split}M_3&=\mathbf{E}\left[x_1\otimes x_2\otimes x_3\right]\\
&-\frac{\alpha_0}{\alpha_0+2}\bigl(\mathbf{E}\left[x_1\otimes x_2\otimes M_1\right]+\mathbf{E}\left[x_1\otimes M_1\otimes x_2\right]+\mathbf{E}\left[M_1\otimes x_1\otimes x_2\right]\bigr)\\
&+\frac{2\alpha_0^2}{\left(\alpha_0+1\right)\left(\alpha_0+2\right)}M_1\otimes M_1\otimes M_1,\end{split}
\end{align}$$

with

$$\alpha_0=\sum_{i=1}^k\alpha_i,$$

from statistics we could derive that

$$\begin{align}
M_2&=\frac{1}{\alpha_0\left(\alpha_0+1\right)}\sum_{i=1}^k\alpha_i\beta_i\otimes\beta_i,\\
M_3&=\frac{2}{\alpha_0\left(\alpha_0+1\right)\left(\alpha_0+2\right)}\sum_{i=1}^k\alpha_i\beta_i\otimes\beta_i\otimes\beta_i.
\end{align}$$

The problem is now: given the above two equations, could we solve for $\alpha$, $\beta$?

## Orthogonal Tensor Decomposition
Let's first scale $M_2$, $M_3$ to get

$$\begin{align}
H_2&=\sum_{i=1}^k\alpha_i\beta_i\otimes\beta_i=\sum_{i=1}^k\left(\alpha_i^{1/2}\beta_i\right)\otimes\left(\alpha_i^{1/2}\beta_i\right),\\
H_3&=\sum_{i=1}^k\alpha_i\beta_i\otimes\beta_i\otimes\beta_i=\sum_{i=1}^k\alpha_i^{-1/2}\left(\alpha_i^{1/2}\beta_i\right)\otimes\left(\alpha_i^{1/2}\beta_i\right)\otimes\left(\alpha_i^{1/2}\beta_i\right).
\end{align}$$

Let's set out to find the *whitening* matrix $W\in\mathbb{R}^{V\times k}$, such that 

$$\begin{split}
H_2(W,W)&=\sum_{i=1}^k\left(W^{\top}\alpha_i^{1/2}\beta_i\right)\otimes\left(W^{\top}\alpha_i^{1/2}\beta_i\right)\\
&=\Bigl(W^{\top}\beta\text{diag}\left(\alpha^{1/2}\right)\Bigr)\Bigl(W^{\top}\beta\text{diag}\left(\alpha^{1/2}\right)\Bigr)^{\top}=I,
\end{split}$$

then we could show the matrix 

$$W^{\top}\beta\text{diag}\left(\alpha^{1/2}\right)=\begin{bmatrix}W^{\top}\alpha_1^{1/2}\beta_1 & \cdots & W^{\top}\alpha_k^{1/2}\beta_k\end{bmatrix}\in\mathbb{R}^{k\times k}$$ 

is of rank $k$, it's thus orthogonal.

It follows that 

$$H_3(W,W,W)=\sum_{i=1}^k\alpha_i^{-1/2}\left(W^{\top}\alpha_i^{1/2}\beta_i\right)\otimes\left(W^{\top}\alpha_i^{1/2}\beta_i\right)\otimes\left(W^{\top}\alpha_i^{1/2}\beta_i\right)$$

is a symmetric orthogonal tensor.

Following the notation of last post, if we did the CP decomposition

$$H_3(W,W,W)=\left[\Lambda;N,N,N\right]$$

where $\Lambda=\left(\lambda_1,\ldots,\lambda_k\right)\in\mathbb{R}^k$, $N=\begin{bmatrix}\nu_1 & \cdots & \nu_k\end{bmatrix}\in\mathbb{R}^{k\times k}$.

We could equate

$$\begin{align}
\Lambda&=\alpha^{-1/2},\\
N&=W^{\top}\beta\alpha^{1/2},
\end{align}$$

from which we obtain

$$\begin{align}
\alpha&=\Lambda^{-2},\\
\beta&=\left(W^{\top}\right)^{\dagger}N\text{diag}(\Lambda),
\end{align}$$

where $\dagger$ denotes the Moore-Penrose pseudoinverse.

## The Whitening Matrix
It's actually straightforward to get the whitening matrix $W$. Note that

$$\Bigl(W^{\top}\beta\text{diag}\left(\alpha^{1/2}\right)\Bigr)\Bigl(W^{\top}\beta\text{diag}\left(\alpha^{1/2}\right)\Bigr)^{\top}=W^{\top}\beta\text{diag}(\alpha)\beta W=W^{\top}H_2 W=I.$$

If the top-$k$ eigendecomposition of $H_2$ is $U\text{diag}(S)U^{\top}$, where $U\in\mathbb{R}^{V\times k}$, $S\in\mathbb{R}^k$, obviously

$$W=U\text{diag}\left(S^{-1/2}\right)$$

and

$$\left(W^{\top}\right)^{\dagger}=U\text{diag}\left(S^{1/2}\right).$$

Note that by whitening the tensors $H_2$, $H_3$, we shrank the tensor size from $\mathbb{R}^{V\times\cdots\times V}$ to $\mathbb{R}^{k\times\cdots\times k}$.

## Forming Tensors from Empirical Counts
Given document $d$ and its word count vector $c_d\in\mathbb{R}^V$, denote the total counts $l_d=\sum_{j=1}^{V}\left(c_d\right)_j$. The contribution of $d$ to $\mathbf{E}\left[x_1\otimes x_2\right]$ is

$$\frac{1}{l_d\left(l_d-1\right)}P_d=\frac{1}{l_d\left(l_d-1\right)}\Bigl[c_d\otimes c_d-\sum_{j=1}^V\left(c_d\right)_j e_j\otimes e_j\Bigr]$$


