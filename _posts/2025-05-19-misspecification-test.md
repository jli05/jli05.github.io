---
title: "Misspecification test, or why there is need for a high degree of sparsity for a large neural network to be statistically verified"
layout: post
---


# Introduction
Suppose some human being is given a list of logical or practical questions, and has answered each of them.

Now a model is asked to simulate the human being with a [likelihood function](https://en.wikipedia.org/wiki/Likelihood_function). The likelihood function outputs a likelihood for _any_ input-output pair. But for an input statement in the training data, the calculated likelihood should be generally higher when the correpsonding output matches the human's output, and lower otherwsie.

Below, we'll check if the model has learned the "true" human function. It is also called misspecification test in statistics: whether the learned likelihood function is correctly specified.

## Misspecification test
In econometrics, the **[Information matrix test](https://en.wikipedia.org/wiki/Information_matrix_test)** is used to determine whether a regression model is misspecified.

Following the same notations as in the Wikipedia article, after learning, the model's <math><mi>r</mi></math> parameters are stored in the vector <math><mi>&theta;</mi></math>, and the logarithm of the learned likelihood function is denoted by <math><mrow><mi>l</mi><mo>(</mo><mi>&theta;</mi><mo>)</mo></mrow></math>. If the model is a neural network, the <math><mi>r</mi></math> parameters in <math><mi>&theta;</mi></math> would represent the weights inside the neural network.

Now, for each input-output training sample, one'd evaluate the matrix below
<math display="block">
<mrow>
<mfrac>
  <mrow>
  <msup>
    <mo>&part;</mo>
    <mn>2</mn>
  </msup>
  <mi>l</mi>
  <mo>(</mo>
  <mi>&theta;</mi>
  <mo>)</mo>
  </mrow>
  <mrow>
    <mo>&part;</mo>
    <mi>&theta;</mi>
    <mo>&part;</mo>
    <msup>
      <mi>&theta;</mi>
      <mo>T</mo>
    </msup>
  </mrow>
</mfrac>
<mo>+</mo>
<mrow>
<mo>(</mo>
<mfrac>
  <mrow>
    <mo>&part;</mo>
    <mi>l</mi>
    <mo>(</mo>
    <mi>&theta;</mi>
    <mo>)</mo>
  </mrow>
  <mrow>
    <mo>&part;</mo>
    <mi>&theta;</mi>
  </mrow>
</mfrac>
<mo>)</mo>
</mrow>
<msup>
<mrow>
<mo>(</mo>
<mfrac>
  <mrow>
    <mo>&part;</mo>
    <mi>l</mi>
    <mo>(</mo>
    <mi>&theta;</mi>
    <mo>)</mo>
  </mrow>
  <mrow>
    <mo>&part;</mo>
    <mi>&theta;</mi>
  </mrow>
</mfrac>
<mo>)</mo>
</mrow>
<mo>T</mo>
</msup>
<mtext>,</mtext>
</mrow>
</math>

a square matrix of size <math><mrow><mi>r</mi><mo>&times;</mo><mi>r</mi></mrow></math>.

If the machine learned well the human being's function, the [information matrix test](https://en.wikipedia.org/wiki/Information_matrix_test) says that, each of the <math><msup><mi>r</mi><mn>2</mn></msup></math> elements of the matrix should be independent [white noise](https://en.wikipedia.org/wiki/Normal_distribution) centred around zero, statistically. The test of that hypothesis will involve computing the [covariance matrix](https://en.wikipedia.org/wiki/Covariance_matrix) between the <math><msup><mi>r</mi><mn>2</mn></msup></math> elements of the above matrix, over the training samples. 

According to [White, Halbert et al 1992](#ref-white92), if the model was correctly specified, this [covariance matrix](https://en.wikipedia.org/wiki/Covariance_matrix) is computable by a formula. The result is a matrix of size <math><mrow><msup><mi>r</mi><mn>2</mn></msup><mo>&times;</mo><msup><mi>r</mi><mn>2</mn></msup></mrow></math>, and would take <math><msup><mi>r</mi><mn>4</mn></msup></math> scale of computation.

## The <math><msup><mi>r</mi><mn>4</mn></msup></math> scale of computation
For Large Language Models (LLM), the number of parameters <math><mi>r</mi></math> is usually in the order of billions, or <math><msup><mn>10</mn><mn>9</mn></msup></math>, if not more, e.g. Facebook (aka Meta)'s open sourced [Llama](#ref-llama). Then <math><msup><mi>r</mi><mn>4</mn></msup></math> would take it to the order of <math><msup><mn>10</mn><mn>36</mn></msup></math> at minimum.

According to a [list of ranking of fastest computers](https://en.wikipedia.org/wiki/List_of_fastest_computers), the fastest one may execute at 1.102 exaFLOPS, or <math><mn>1.102</mn><mo>&times;</mo><msup><mn>10</mn><mn>18</mn></msup></math> floating-point operations per second. If we have 1,000 of them, the time for our computation would be in the order of <math><msup><mn>10</mn><mn>15</mn></msup></math> seconds, that is, 31 million years.

Hence it may be close to impossible to statistically test if a LLM was misspecified, if the LLM was densely connected inside, i.e. one neuron connected to most other neurons.

## If there is a high degree of sparsity in the neural network
If relative to the total number of neurons, one neuron is only connected to few fellow neurons, the matrix
<math display="block">
<mrow>
<mfrac>
  <mrow>
  <msup>
    <mo>&part;</mo>
    <mn>2</mn>
  </msup>
  <mi>l</mi>
  <mo>(</mo>
  <mi>&theta;</mi>
  <mo>)</mo>
  </mrow>
  <mrow>
    <mo>&part;</mo>
    <mi>&theta;</mi>
    <mo>&part;</mo>
    <msup>
      <mi>&theta;</mi>
      <mo>T</mo>
    </msup>
  </mrow>
</mfrac>
<mo>+</mo>
<mrow>
<mo>(</mo>
<mfrac>
  <mrow>
    <mo>&part;</mo>
    <mi>l</mi>
    <mo>(</mo>
    <mi>&theta;</mi>
    <mo>)</mo>
  </mrow>
  <mrow>
    <mo>&part;</mo>
    <mi>&theta;</mi>
  </mrow>
</mfrac>
<mo>)</mo>
</mrow>
<msup>
<mrow>
<mo>(</mo>
<mfrac>
  <mrow>
    <mo>&part;</mo>
    <mi>l</mi>
    <mo>(</mo>
    <mi>&theta;</mi>
    <mo>)</mo>
  </mrow>
  <mrow>
    <mo>&part;</mo>
    <mi>&theta;</mi>
  </mrow>
</mfrac>
<mo>)</mo>
</mrow>
<mo>T</mo>
</msup>
</mrow>
</math>

would consequentially be highly sparse, or with many elements being zero. The number of non-zero elements would be in the order of <math><mi>r</mi></math>, mostly on the diagonal of the matrix.

The covariance matrix of about <math><mi>r</mi></math> non-zero elements, would be of size of order <math><msup><mi>r</mi><mn>2</mn></msup></math>. In the example for Llama, the scale of computation drops to <math><msup><mn>10</mn><mn>18</mn></msup></math>. The 1,000 supercomputers would need the order of 0.001 second to run the statistical test.

## Human's number of neurons and their interconnections (synapses)
If we check this Wikipedia article on [Neuron](https://en.wikipedia.org/wiki/Neuron),

> The human brain has some <math><mrow><mn>8.6</mn><mo>&times;</mo><msup><mn>10</mn><mn>10</mn></msup></mrow></math> (eighty six billion) neurons.[28] Each neuron has on average 7,000 synaptic connections to other neurons. It has been estimated that the brain of a three-year-old child has about <math><msup><mn>10</mn><mn>15</mn></msup></math> synapses (1 quadrillion). This number declines with age, stabilizing by adulthood. Estimates vary for an adult, ranging from <math><msup><mn>10</mn><mn>14</mn></msup></math> to <math><mrow><mn>5</mn><mo>&times;</mo><msup><mn>10</mn><mn>14</mn></msup></mrow></math> synapses (100 to 500 trillion).

we can read two things:

first, for an adult human brain, relatively to the total number of neurons, averagely one neuron is connected to less than one-millionth of them;

second, the total number of synapses, our <math><mi>r</mi></math>, decreases as the human grows from child to adult. Take the 3-year old child, <math><mi>r</mi></math> is circa <math><msup><mn>10</mn><mn>15</mn></msup></math>.

For a 3-year old child, we can only hope any single task has involved far fewer than <math><msup><mn>10</mn><mn>15</mn></msup></math> synapses to be able to statistically verify a computational model of it with the current machinery.

## REFERENCES
<a name="ref-white92">White, Halbert et al, 1992, Artificial Neural Networks: Approximation and Learning Theory.</a>

<a name="ref-llama">LLaMA: Open and Efficient Foundation Language Models. https://arxiv.org/abs/2302.13971</a>
