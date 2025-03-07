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
|  2           | A AND (NOT B)       | <math><mi>a</mi><mo>-</mo><mi>a</mi><mi>b</mi></math>  |
|  3           | A                   | <math><mi>a</mi></math>  |
|  4           | (NOT A) AND B       | <math><mi>b</mi><mo>-</mo><mi>a</mi><mi>b</mi></math>  |
|  5           | B                   | <math><mi>b</mi></math> |
|  6           | XOR                 | <math><mi>a</mi><mo>+</mo><mi>b</mi><mo>-</mo><mn>2</mn><mi>a</mi><mi>b</mi></math> |
|  7           |  OR                 | <math><mi>a</mi><mo>+</mo><mi>b</mi><mo>-</mo><mi>a</mi><mi>b</mi></math> |
|  8           | NOR                 | <math><mn>1</mn><mo>-</mo><mo>(</mo><mi>a</mi><mo>+</mo><mi>b</mi><mo>-</mo><mi>a</mi><mi>b</mi><mo>)</mo></math>  |
|  9           | XNOR                | <math><mn>1</mn><mo>-</mo><mo>(</mo><mi>a</mi><mo>+</mo><mi>b</mi><mo>-</mo><mn>2</mn><mi>a</mi><mi>b</mi><mo>)</mo></math>   |
|  10          | NOT B               | <math><mn>1</mn><mo>-</mo><mi>b</mi></math>   |
|  11          | A OR (NOT B)        | <math><mn>1</mn><mo>-</mo><mi>b</mi><mo>+</mo><mi>a</mi><mi>b</mi></math>          |
|  12          | NOT A               | <math><mn>1</mn><mo>-</mo><mi>a</mi></math>                  |
|  13          | (NOT A) OR B        | <math><mn>1</mn><mo>-</mo><mi>a</mi><mo>+</mo><mi>a</mi><mi>b</mi></math>          |
|  14          | NAND                | <math><mn>1</mn><mo>-</mo><mi>a</mi><mi>b</mi></math>     |
|  15          | TRUE                | <math><mn>1</mn></math>   |
