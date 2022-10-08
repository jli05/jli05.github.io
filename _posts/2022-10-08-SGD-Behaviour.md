---
title: SGD Behaviour
layout: post
---

Understanding Gradient Descent on Edge of Stability in Deep Learning [https://arxiv.org/abs/2205.09745](https://arxiv.org/abs/2205.09745)

Per Zhiyuan Li,

 If the learning rate (LR) is constant, there won’t be EoS and the x(t) will converge to 0 if LR < 2/(largest eigenvalue). In such case, x(t) will align to the direction along which the power iteration converges the slowest, that is, either the smallest eigenvector or the largest eigenvector depending on the relationship between LR and the eigenvalues. If LR > 2/(largest eigenvalue) then GD diverges.

Self-Stabilization: The Implicit Bias of Gradient Descent at the Edge of Stability [https://arxiv.org/abs/2209.15594](https://arxiv.org/abs/2209.15594)

Rethinking SGD's Noise [https://francisbach.com/rethinking-sgd-noise/](https://francisbach.com/rethinking-sgd-noise/)

Rethinking SGD’s noise – II: Implicit Bias [https://francisbach.com/implicit-bias-sgd/](https://francisbach.com/implicit-bias-sgd/)
