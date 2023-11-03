---
title: Kullback, Information Theory and Statistics
layout: post
---

Solomon Kullback, Information Theory and Statistics

If <math><msub><mi>H</mi><mi>i</mi></msub></math>, <math><mi>i</mi><mo>=</mo><mn>1,2</mn></math> is the hypothesis that <math><mi>X</mi></math> is from statistical measure <math><msub><mi>μ</mi><mi>i</mi></msub></math>, where

<math display="block">
<mi>d</mi><msub><mi>μ</mi><mi>i</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo>
<mo>=</mo>
<msub><mi>f</mi><mi>i</mi></msub><mi>d</mi><mi>λ</mi><mo>(</mo><mi>x</mi><mo>)</mo>
<mtext>,</mtext>
</math>

and <math><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>i</mi></msub><mo>)</mo></math> is the prior probability of the respective hypothesis, <math><mi>i</mi><mo>=</mo><mn>1,2</mn></math>, it follows that conditional on <math><mi>x</mi></math>,

<math display="block">
<mrow><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>i</mi></msub><mo>|</mo><mi>x</mi><mo>)</mo></mrow>
<mo>=</mo>
<mfrac>
<mrow><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>i</mi></msub><mo>)</mo><msub><mi>f</mi><mi>i</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow>
<mrow><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>1</mi></msub><mo>)</mo><msub><mi>f</mi><mi>1</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo><mo>+</mo><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>2</mi></msub><mo>)</mo><msub><mi>f</mi><mi>2</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow>
</mfrac>
<mtext>,</mtext>
</math>

when <math><mi>x</mi></math> is over a non-zero measured set.

One obtains

<math display="block">
<mo>log</mo><mfrac><mrow><msub><mi>f</mi><mi>1</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow><mrow><msub><mi>f</mi><mi>2</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow></mfrac>
<mo>=</mo>
<mo>log</mo><mfrac><mrow><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>1</mi></msub><mo>|</mo><mi>x</mi><mo>)</mo></mrow><mrow><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>2</mi></msub><mo>|</mo><mi>x</mi><mo>)</mo></mrow></mfrac><mo>-</mo>
<mo>log</mo><mfrac><mrow><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>1</mi></msub><mo>)</mo></mrow><mrow><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>2</mi></msub><mo>)</mo></mrow></mfrac>
<mtext>,</mtext>
</math>

when <math><mi>x</mi></math> is over a non-zero measured set.

This logarithm of the likelihood ratio, <math><mo>log</mo><mo>[</mo><msub><mi>f</mi><mi>1</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo><mo>/</mo><msub><mi>f</mi><mi>2</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo><mo>]</mo></math> is defined as the information in <math><mi>X</mi><mo>=</mo><mi>x</mi></math> for discrimination in favour of <math><msub><mi>H</mi><mi>1</mi></msub></math> against <math><msub><mi>H</mi><mi>2</mi></msub></math>.


