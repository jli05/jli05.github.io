---
title: Misspecification Test for Large Models
layout: post
excerpt: misspecification test on the log-likelihood function for large models, or why there may be need for a high degree of sparsity in large neural network models
---


## Introduction
Suppose a specific (or a group of) human being is asked to score any input text with a [likelihood function](https://en.wikipedia.org/wiki/Likelihood_function). A piece of input text could describe a real-life or hypothetical situation or a sequence of problem solving, and the human being is asked to indicate how likely or reasonable the whole piece looks according to the person's own instincts, reasonings, and perceived social conventions.

If the human being encountered the same situation in the real world as described by an input text, he would act in consistence with his likelihood function. As an example, one input text says "In situation S, take action A", another says "In the same situation S, take action B", if the person gave the former likelihood 0.3, the latter likelihood 0.2, then in situation S in the real world, his chances of taking A vs B would be 3-to-2. This requirement transports a "mental" likelihood function to a real-world, physical likelihood function.

Now a Large Model is asked to simulate the human being and output the likelihood as well. It may be trained on human-produced data first and settle on a learned computational mechanism to output a likelihood score for any input text. 

Below, we'll check if the model has likely learned a "true" likelihood function just as the human being's. It is also called misspecification test in statistics: whether the learned likelihood function is good or not, aka correctly specified.

## Misspecification Test
Following the notations as in [Information matrix test](https://en.wikipedia.org/wiki/Information_matrix_test), after learning, the model's <math><mi>r</mi></math> parameters are stored in the vector <math><mi>&theta;</mi></math>, and the logarithm of the learned likelihood function is denoted by <math><mrow><mi>l</mi><mo>(</mo><mi>&theta;</mi><mo>)</mo></mrow></math>.

As a note, if the model was a neural network, the <math><mi>r</mi></math> parameters in <math><mi>&theta;</mi></math> would represent the weights inside the neural network.

Now, given any input text, whether already seen in the training phase or brand new, one'd evaluate the matrix below
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


a square one of size <math><mrow><mi>r</mi><mo>&times;</mo><mi>r</mi></mrow></math>, i.e. with <math><msup><mi>r</mi><mn>2</mn></msup></math> elements in total.

Again, if the model was a neural network and densely connected, i.e. with each neuron connected to many other neurons, the above square matrix could also be dense, i.e. with many non-zero elements and rare zero elements in it.

Over <math><mi>N</mi></math> input texts, we would have evaluated <math><mi>N</mi></math> copies, or realisations of the above matrix, probably obtaining a different one for different input text.

If the machine learned well the human being's likelihood function, the [information matrix test](https://en.wikipedia.org/wiki/Information_matrix_test) says that, every of the <math><msup><mi>r</mi><mn>2</mn></msup></math> elements of the matrix should be independent [white noise](https://en.wikipedia.org/wiki/Normal_distribution) centred around zero.

Furthermore, if the model was correctly specified, according to [White, Halbert et al 1992](#ref-white92), the [covariance matrix](https://en.wikipedia.org/wiki/Covariance_matrix) between the <math><msup><mi>r</mi><mn>2</mn></msup></math> elements, a matrix of size <math><mrow><msup><mi>r</mi><mn>2</mn></msup><mo>&times;</mo><msup><mi>r</mi><mn>2</mn></msup></mrow></math>, shall be computable by a formula. The formula however, would take <math><msup><mi>r</mi><mn>4</mn></msup></math> scale of computation.

Once the <math><mrow><msup><mi>r</mi><mn>2</mn></msup><mo>&times;</mo><msup><mi>r</mi><mn>2</mn></msup></mrow></math> covariance matrix is computed under the hypothesis of good model specification, a statistical test can be readily carried out to indicate how conflict-free the evidence is against the hypothesis of good model specification, assuming the model *is* already correctly specified.

## The <math><msup><mi>r</mi><mn>4</mn></msup></math> Scale of Computation
For Large Language Models (LLM), the number of parameters <math><mi>r</mi></math> is usually in the order of billions, or <math><msup><mn>10</mn><mn>9</mn></msup></math>, if not more, e.g. Facebook (aka Meta)'s open sourced [Llama](#ref-llama). Then <math><msup><mi>r</mi><mn>4</mn></msup></math> would take it to the order of <math><msup><mn>10</mn><mn>36</mn></msup></math> at minimum.

According to a [list of ranking of fastest computers](https://en.wikipedia.org/wiki/List_of_fastest_computers), the fastest one may execute at 1.102 exaFLOPS, or <math><mn>1.102</mn><mo>&times;</mo><msup><mn>10</mn><mn>18</mn></msup></math> floating-point operations per second. If we have 1,000 of them, the time for our computation would be in the order of <math><msup><mn>10</mn><mn>15</mn></msup></math> seconds, that is, 31 million years.

Hence it may be close to impossible to find out if a LLM was correctly specified as its modelling objective, if the LLM was densely connected inside, i.e. one neuron connected to most other neurons.

## If there were a High Sparsity in the Neural Network
If relative to the total number of neurons, one neuron is only connected to few fellow neurons, so sparse that for a given input text, only a tiny fraction of the neurons would be involved in crunching out the final likelihood value, the matrix
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

would consequentially be highly sparse, or with many elements being zero. The number of non-zero elements would be in the order of <math><mi>r</mi></math>, arranged sporadically on and symmetrically off the diagonal line of the matrix.

As regards the covariance matrix of these non-zero elements, the number of non-zero elements could still be in the order of <math><mi>r</mi></math>, assuming <math><mi>r</mi></math> is a very big number, and the neurons comparatively are very sparsely connected.

If we check this Wikipedia article on [Neuron](https://en.wikipedia.org/wiki/Neuron),

> The human brain has some <math><mrow><mn>8.6</mn><mo>&times;</mo><msup><mn>10</mn><mn>10</mn></msup></mrow></math> (eighty six billion) neurons.[28] Each neuron has on average 7,000 synaptic connections to other neurons. It has been estimated that the brain of a three-year-old child has about <math><msup><mn>10</mn><mn>15</mn></msup></math> synapses (1 quadrillion). This number declines with age, stabilizing by adulthood. Estimates vary for an adult, ranging from <math><msup><mn>10</mn><mn>14</mn></msup></math> to <math><mrow><mn>5</mn><mo>&times;</mo><msup><mn>10</mn><mn>14</mn></msup></mrow></math> synapses (100 to 500 trillion).

we can read two things:

first, for an adult human brain, relatively to the total number of neurons, averagely one neuron is connected to less than one-millionth of them;

second, the total number of synapses, our <math><mi>r</mi></math>, decreases as the human grows from child to adult. Take the 3-year old child, <math><mi>r</mi></math> is circa <math><msup><mn>10</mn><mn>15</mn></msup></math>.

From what we said above, in this case of high sparsity, every step of computation would be in the order of <math><mi>r</mi></math>, in order to carry out the misspecification test. When <math><mi>r</mi></math> is in the order of <math><msup><mn>10</mn><mn>15</mn></msup></math>, on a single typical laptop microprocessor, it may take at most weeks to finish the computation, provided that it can sustain working at full power and heat for that long.

## Parallel Processing for Multiple Objectives in a Large Model
In the section above, we supposed that for a given input text, only a tiny fraction of the neurons would be involved in the computation. Broadly, if for any given specific task, only a tiny fraction of neurons would be involved in computing the result, it is possible to imagine a practical Large Model always computing for multiple tasks or objectives at the same time, dedicating largely different fractions of neurons to different tasks or objectives. At the outcome, it may weigh the results obtained for the various objectives and take an action. Is that why we as human are always puzzled about life's conundrums?

## REFERENCES
<a name="ref-white92">White, Halbert et al, 1992, Artificial Neural Networks: Approximation and Learning Theory.</a>

<a name="ref-llama">LLaMA: Open and Efficient Foundation Language Models. https://arxiv.org/abs/2302.13971</a>
