---
title: Continuous Relaxation of Logic Gates
layout: post
---

Differentiable Logic Cellular Automata
[https://google-research.github.io/self-organising-systems/difflogic-ca/](https://google-research.github.io/self-organising-systems/difflogic-ca/)

| Index        | Operation           | Continuous relaxation  |
| ------------ | ------------------- | ---------------------- |
|  0           | FALSE               | <math><mn>0</mn></math> |
|  1           | AND                 | <math><mi>a</mi><mi>b</mi></math> |
|  2           | A AND (NOT B)       | a - a * b              |
|  3           | A                   |     a                  |
|  4           | (NOT A) AND B       | b - a * b              |
|  5           | B                   |    b                   |
|  6           | XOR                 | a + b - 2 a * b        |
|  7           |  OR                 | a + b - a * b          |
|  8           | NOR                 | 1 - (a + b - a * b)    |
|  9           | XNOR                | 1 - (a + b - 2a * b)   |
|  10          | NOT B               | 1 - b                  |
|  11          | A OR (NOT B)        | 1 - b + a * b          |
|  12          | NOT A               | 1 - a                  |
|  13          | (NOT A) OR B        | 1 - a + a * b          |
|  14          | NAND                | 1 - a * b              |
|  15          | TRUE                | 1                      |
