---
title: Tensor Calculations 
layout: post
---

<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

# Tensor Matricization (Unfolding)
As an example, let the frontal slices of $\mathcal{X}\in\mathbb{R}^{3\times 4\times 2}$ be

$$X_1=\Biggl[\begin{matrix}1 & 4 & 7 & 10\\2 & 5 & 8 & 11\\3 & 6 & 9 & 12\end{matrix}\Biggr],\quad X_2=\Biggl[\begin{matrix}13 & 16 & 19 & 22\\14 & 17 & 20 & 23\\15 & 18 & 21 & 24\end{matrix}\Biggr]$$

Then the three mode-$n$ unfoldings are

$$X_{(1)}=\Biggl[\begin{matrix}1 & 4 & 7 & 10 & 13 & 16 & 19 & 22\\2 & 5 & 8 & 11 & 14 & 17 & 20 & 23\\3 & 6 & 9 & 12 & 15 & 18 & 21 & 24\end{matrix}\Biggr],$$

$$X_{(2)}=\Biggl[\begin{matrix}1 & 2 & 3 & 13 & 14 & 15\\4 & 5 & 6 & 16 & 17 & 18\\7 & 8 & 9 & 19 & 20 & 21\\10 & 11 & 12 & 22 & 23 & 24\end{matrix}\Biggr],$$

$$X_{(3)}=\Biggl[\begin{matrix}1 & 2 & 3 & 4 & 5 & \dots & 9 & 10 & 11 & 12\\13 & 14 & 15 & 16 & 17 & \ldots & 21 & 22 & 23 & 24\end{matrix}\Biggr].$$

# Matrix Kronecker, Khatri-Rao, and Hadamard Products
The *Kronecker product* of matrices $A\in\mathbb{R}^{I\times J}$ and $B\in\mathbb{R}^{K\times L}$ is denoted by $A\otimes B$. The result is a matrix of size $(IK)\times(JL)$ and defined by

$$A\otimes B\begin{split}=&\begin{bmatrix}a_{11}B & a_{12}B & \cdots & a_{1J}B\\a_{21}B & a_{22}B & \cdots & a_{2J}B \\ \vdots & \vdots & \ddots & \vdots \\ a_{I1}B & a_{I2}B & \cdots & a_{IJ}B\end{bmatrix}\\
=&\begin{bmatrix}a_1\otimes b_1 & a_1\otimes b_2 & a_1\otimes b_3 & \dots & a_J\otimes b_{L-1} & a_J\otimes b_L\end{bmatrix}.\end{split}$$

The *Khatri-Rao product* is the "matching columnwise" Kronecker product. Given matrices $A\in\mathbb{R}^{I\times J}$ and $B\in\mathbb{R}^{K\times L}$, their Khatri-Rao product is denoted by $A\odot B$. The result is a matrix of size $(IJ)\times K$ defined by

$$A\odot B=\begin{bmatrix}a_1\otimes b_1 & a_2\otimes b_2 & \cdots & a_K\otimes b_K\end{bmatrix}.$$

The *Hadamard product* is the elementwise matrix product. Given matrices $A$ and $B$, both of size $I\times J$, their Hadamard product is denoted by $A*B$. The result is also of size $I\times J$ and defined by

$$A*B=\begin{bmatrix}a_{11}b_{11} & a_{12}b_{12} & \cdots & a_{1J}b_{1J}\\a_{21}b_{21} & a_{22}b_{22} & \cdots & a_{2J}b_{2J}\\ \vdots & \vdots & \ddots & \vdots \\ a_{I1}b_{I1} & a_{I2}b_{I2} & \cdots & a_{IJ}b_{IJ}\end{bmatrix}$$

Some useful formulas:

$$\begin{align}
\left(A\otimes B\right)\left(C\otimes D\right)&=AC\otimes BD,\\
\left(A\otimes B\right)^{\dagger}&=A^{\dagger}\otimes B^{\dagger},\\
A\odot B\odot C&=\left(A\odot B\right)\odot C=A\odot\left(B\odot C\right),\\
\left(A\odot B\right)^{\top}\left(A\odot B\right)&=A^{\top}A*B^{\top}B,\\
\left(A\odot B\right)^{\dagger}&=\left(\left(A^{\top}A\right)*\left(B^{\top}B\right)\right)^{\dagger}\left(A\odot B\right)^{\top},
\end{align}$$

where $A^{\dagger}$ denotes the Moore-Penrose pseudoinverse of $A$.

# Number of Eigenvectors of Orthogonally Decomposable (*odeco*) Tensors
A tensor $T\in\mathbb{R}^n\otimes\cdots\otimes\mathbb{R}^n$ is *orthogonally decomposable* or *odeco* if we can decompose it as

$$T=\sum_{i=1}^n\lambda_i a_i\otimes b_i\otimes c_i\otimes\cdots,$$

so that $a_1,\ldots,a_n\in\mathbb{R}^n$ are orthonormal, $b_1,\ldots,b_n\in\mathbb{R}^n$ are orthonormal, $c_1,\ldots,c_n\in\mathbb{R}^n$ are orthonormal, etc.

A symmetric odeco tensor is defined as

$$T=\sum_{i=1}^n\lambda_i v_i^{\otimes d},$$

where $v_1,\ldots,v_n\in\mathbb{R}^n$ are orthonormal.

If there exists a scalar $\lambda\in\mathbb{R}$ and a vector $u\in\mathbb{R}^n$ such that

$$T(I,u,\ldots,u)=\lambda u,$$

then $\lambda$ is one eigenvalue, $u$ is one eigenvector.

**Theorem (Sturmfels and Cartwright)**
If a tensor $T\in S^d\left(\mathbb{C}^n\right)$ has finitely many eigenvectors, then their number is 

$$\frac{(d-1)^n-1}{d-2}.$$

To understand why a rank-$n$ symmetric odeco tensor would have $O(\exp(n))$ eigenvectors, let's see an example 

$$U=\sum_{i=1}^3 e_i^{\otimes 3}\in S^3\left(\mathbb{R}^3\right),$$

where $\lbrace e_i,i=1,2,3\rbrace$ are the natural basis in $\mathbb{R}^3$. It's straightforward to check that $e_1+e_2$, $e_2+e_3$, $e_3+e_1$, $e_1+e_2+e_3$ are all $U$'s eigenvectors as well -- they're all combinations of $\lbrace e_i,i=1,2,3\rbrace$. 

# CANDECOMP/PARAFAC (CP) Decomposition
The CP decomposition factorises a tensor into a sum of component rank-one tensors. For example, given a third-order tensor $\mathcal{X}\in\mathbb{R}^{I\times J\times K}$, it could be written as

$$\mathcal{X}=\left[\Lambda;A,B,C\right]=\sum_{r=1}^R\lambda_r a_r\circ b_r\circ c_r,$$

where $R$ is the number of rank, $\lambda_r\in\mathbb{R}$, $a_r\in\mathbb{R}^I$, $b_r\in\mathbb{R}\in\mathbb{R}^J$, $c_r\in\mathbb{R}^K$ for $r=1,\ldots,R$. $\Lambda=\begin{pmatrix}\lambda_1 & \cdots & \lambda_R\end{pmatrix}$, $A=\begin{bmatrix}a_1 & \cdots & a_R\end{bmatrix}$, likewise for $B$ and $C$. The $L_2$-norms of each column of $A$, $B$, $C$ are all 1.

If we unfold tensor $\mathcal{X}$,

$$X_{(1)}=A\text{diag}(\Lambda)\left(C\odot B\right)^{\top}.$$

Therefore we solve

$$\hat{A}=\mathcal{X}_{(1)}\left[\left(C\odot B\right)^{\top}\right]^{\dagger}=\mathcal{X}_{(1)}\left(C\odot B\right)\left(C^{\top}C*B^{\top}B\right)^{\dagger},$$

normalise the norms of each column in $\hat{A}$ and cycle between $A$, $B$, $C$.

# References 

- Kolda, Tamara G., and Brett W. Bader, Tensor Decompositions and Applications
- Cartwright, Dustin, and Bernd Sturmfels, The Number of Eigenvalues of a Tensor, [https://arxiv.org/abs/1004.4953](https://arxiv.org/abs/1004.4953)
- Robeva, Elina, Orthogonally Decomposable Tensors, [http://www.maths.qmul.ac.uk/~fink/tensors16/OdecoPresentationLondon.pdf](http://www.maths.qmul.ac.uk/~fink/tensors16/OdecoPresentationLondon.pdf)


