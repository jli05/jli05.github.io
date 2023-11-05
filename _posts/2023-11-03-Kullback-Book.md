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

This logarithm of the likelihood ratio, <math><mo>log</mo><mrow><mo>[</mo><msub><mi>f</mi><mi>1</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo><mo>/</mo><msub><mi>f</mi><mi>2</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo><mo>]</mo></mrow></math> is defined as the *information* in <math><mi>X</mi><mo>=</mo><mi>x</mi></math> for discrimination in favour of <math><msub><mi>H</mi><mi>1</mi></msub></math> against <math><msub><mi>H</mi><mi>2</mi></msub></math>.

The mean information for discrimination in favour of <math><msub><mi>H</mi><mi>1</mi></msub></math> against <math><msub><mi>H</mi><mi>2</mi></msub></math> over set <math><mi>E</mi></math> is

<math display="block">
<mrow><mi>I</mi><mo>(</mo><mn>1:2</mn><mo>)</mo></mrow>
<mo>=</mo>
<mrow>
<mo>{</mo>
<mtable>
<mtr>
<mtd columnalign="left">
<mrow><mfrac><mn>1</mn><mrow><msub><mi>μ</mi><mi>1</mi></msub><mo>(</mo><mi>E</mi><mo>)</mo></mrow></mfrac>
<msub><mo stretchy="true">&int;</mo><mi>E</mi></msub>
<mrow><msub><mi>f</mi><mi>1</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow>
<mo>log</mo><mfrac><mrow><msub><mi>f</mi><mi>1</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow><mrow><msub><mi>f</mi><mi>2</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow></mfrac>
<mrow><mi>d</mi><mi>λ</mi><mo>(</mo><mi>x</mi><mo>)</mo></mrow>
</mrow>
</mtd>
<mtd columnalign="left">
<mrow><msub><mi>μ</mi><mi>1</mi></msub><mo>(</mo><mi>E</mi><mo>)</mo><mo>></mo><mn>0</mn></mrow>
</mtd>
</mtr>
<mtr>
<mtd columnalign="left">
<mn>0</mn>
</mtd>
<mtd columnalign="left">
<mrow><msub><mi>μ</mi><mi>1</mi></msub><mo>(</mo><mi>E</mi><mo>)</mo><mo>=</mo><mn>0</mn></mrow>
</mtd>
</mtr>
</mtable>
</mrow>
</math>

Over the entire space,

<math display="block">
<mtable>
<mtr>
<mtd>
<mrow><mi>I</mi><mo>(</mo><mn>1:2</mn><mo>)</mo></mrow>
</mtd>
<mtd>
<mo>=</mo>
</mtd>
<mtd columnalign="left">
<mrow><mo stretchy="true">&int;</mo>
<mo>log</mo><mfrac><mrow><msub><mi>f</mi><mi>1</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow><mrow><msub><mi>f</mi><mi>2</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow></mfrac>
<mrow><mi>d</mi><msub><mi>μ</mi><mi>1</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow>
</mrow>
</mtd>
</mtr>
<mtr>
<mtd></mtd>
<mtd><mo>=</mo></mtd>
<mtd columnalign="left">
<mrow><mo stretchy="true">&int;</mo>
<mrow><msub><mi>f</mi><mi>1</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow>
<mo>log</mo><mfrac><mrow><msub><mi>f</mi><mi>1</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow><mrow><msub><mi>f</mi><mi>2</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow></mfrac>
<mrow><mi>d</mi><mi>λ</mi><mo>(</mo><mi>x</mi><mo>)</mo></mrow>
</mrow>
</mtd>
</mtr>
<mtr>
<mtd>
</mtd>
<mtd>
<mo>=</mo>
</mtd>
<mtd columnalign="left">
<mrow>
<mo>(</mo><mo stretchy="true">&int;</mo>
<mo>log</mo><mfrac><mrow><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>1</mi></msub><mo>|</mo><mi>x</mi><mo>)</mo></mrow><mrow><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>2</mi></msub><mo>|</mo><mi>x</mi><mo>)</mo></mrow></mfrac>
<mrow><mi>d</mi><msub><mi>μ</mi><mi>1</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow><mo>)</mo>
<mo>-</mo>
<mo>log</mo><mfrac><mrow><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>1</mi></msub><mo>)</mo></mrow><mrow><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>2</mi></msub><mo>)</mo></mrow></mfrac>
<mtext>.</mtext>
</mrow>
</mtd>
</mtr>
</mtable>
</math>

It is non-negative, <math><mn>0</mn><mo>&le;</mo><mi>I</mi><mo>(</mo><mn>1:2</mn><mo>)</mo><mo>&le;</mo><mo>+&infin;</mo></math>. On the web one may find examples in which it takes zero value or infinity.

The prior information, <math><mo>log</mo><mfrac><mrow><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>1</mi></msub><mo>)</mo></mrow><mrow><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>2</mi></msub><mo>)</mo></mrow></mfrac></math> still remains in <math><mi>I</mi><mo>(</mo><mn>1:2</mn><mo>)</mo></math>.

Likewise,
<math display="block">
<mtable>
<mtr>
<mtd>
<mrow><mi>I</mi><mo>(</mo><mn>2:1</mn><mo>)</mo></mrow>
</mtd>
<mtd>
<mo>=</mo>
</mtd>
<mtd columnalign="left">
<mrow><mo stretchy="true">&int;</mo>
<mo>log</mo><mfrac><mrow><msub><mi>f</mi><mi>2</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow><mrow><msub><mi>f</mi><mi>1</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow></mfrac>
<mrow><mi>d</mi><msub><mi>μ</mi><mi>2</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow>
</mrow>
</mtd>
</mtr>
<mtr>
<mtd></mtd>
<mtd><mo>=</mo></mtd>
<mtd columnalign="left">
<mrow><mo stretchy="true">&int;</mo>
<mrow><msub><mi>f</mi><mi>2</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow>
<mo>log</mo><mfrac><mrow><msub><mi>f</mi><mi>2</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow><mrow><msub><mi>f</mi><mi>1</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo></mrow></mfrac>
<mrow><mi>d</mi><mi>λ</mi><mo>(</mo><mi>x</mi><mo>)</mo></mrow>
<mtext>.</mtext>
</mrow>
</mtd>
</mtr>
</mtable>
</math>

One may define the *divergence* <math><mi>J</mi><mo>(</mo><mn>1,2</mn><mo>)</mo></math>,

<math display="block">
<mtable>
<mtr>
<mtd>
<mi>J</mi><mo>(</mo><mn>1,2</mn><mo>)</mo>
</mtd>
<mtd><mo>=</mo></mtd>
<mtd columnalign="left"><mrow><mi>I</mi><mo>(</mo><mn>1:2</mn><mo>)</mo><mo>+</mo><mi>I</mi><mo>(</mo><mn>2:1</mn><mo>)</mo></mrow></mtd>
</mtr>
<mtr>
<mtd></mtd>
<mtd><mo>=</mo></mtd>
<mtd columnalign="left">
<mrow>
<mo stretchy="true">&int;</mo>
<mo>log</mo><mfrac><mrow><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>1</mi></msub><mo>|</mo><mi>x</mi><mo>)</mo></mrow><mrow><mi>P</mi><mo>(</mo><msub><mi>H</mi><mi>2</mi></msub><mo>|</mo><mi>x</mi><mo>)</mo></mrow></mfrac>
<mrow><mo>(</mo><mi>d</mi><msub><mi>μ</mi><mi>1</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo><mo>-</mo><mi>d</mi><msub><mi>μ</mi><mi>2</mi></msub><mo>(</mo><mi>x</mi><mo>)</mo><mo>)</mo></mrow>
<mtext>.</mtext>
</mrow>
</mtd>
</mtr>
</mtable>
</math>

If one swaps the position of hypothesis 1 and 2 one obtains the same quantity, <math><mi>J</mi><mo>(</mo><mn>1,2</mn><mo>)</mo><mo>=</mo><mi>J</mi><mo>(</mo><mn>2,1</mn><mo>)</mo></math>. So it is symmetric, and the prior information disappeared. Kullback proposes to call <math><mi>I</mi><mo>(</mo><mn>1:2</mn><mo>)</mo></math> or <math><mi>I</mi><mo>(</mo><mn>2:1</mn><mo>)</mo></math> *directed divergence*.
