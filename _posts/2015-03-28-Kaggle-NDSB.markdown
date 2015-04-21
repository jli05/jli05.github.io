---
title: Kaggle 2015 National Data Science Bowl competition
layout: post
---

The competition is to classify 121-class sea plankton images. The winner Sander Dieleman has [a thorough post][1] about their winning strategy. This is the first time I join a deep learning competition so it was a good learning experience for me. The code is at [https://github.com/jli05/ndsb](https://github.com/jli05/ndsb).

# Image Preprocessing
The images come with different sizes, I resized them conserving the width-height ratio, then padded to size (120,120). As for certain species we have fewer than 10 images, for others we could have thousands, I used Stratified Sampling to produce train, validate data with [`sklearn.cross_validation.StratifiedShuffleSplit`](http://scikit-learn.org/stable/modules/generated/sklearn.cross_validation.StratifiedShuffleSplit.html).

# Training with Convolutional Net
The code is derived from `convolutional_mlp.py` in the [Deep Learning Tutorial](http://deeplearning.net/tutorial/). I made the following changes:

<ol>
<li><p>At input, we spawn four copies of images from one input image. They are the originial one, the original one turned 90 degrees clock-wise, the original one turned 180 degrees clock-wise, the original one turned 270 degrees clock-wise.</p>
<pre><code>x_ = x.reshape((batch_size, 1, 120, 120))
# Rotate 90 degree each time
x1_ = x_[:, :, ::-1, :].dimshuffle((0, 1, 3, 2))
x2_ = x1_[:, :, ::-1, :].dimshuffle((0, 1, 3, 2))
x3_ = x2_[:, :, ::-1, :].dimshuffle((0, 1, 3, 2))
# layer0_input is of shape (4 * batch_size, 1, 120, 120)
layer0_input = T.concatenate([x_, x1_, x2_, x3_], axis=0)
# y2 is of shape (4 * batch_size)
y2 = T.concatenate([y, y, y, y], axis=0)
</code></pre>
<p>Then in the code defining all the conv-maxpooling layers, the first dimension is <code>4 * batch_size</code> rather than <code>batch_size</code>.</p>
<pre><code>layer0 = LeNetConvPoolLayer(
        rng,
        input=layer0_input,
        image_shape=(batch_size * 4, 1, 120, 120),
        filter_shape=(nkerns[0], 1, 5, 5),
        poolsize=(2, 2),
        W=None if param_values is None else param_values['layer0_W'],
        b=None if param_values is None else param_values['layer0_b'],
        name='layer0'
    )
</code></pre>
</li>

<li><p>The architecture is 4 layers of conv-maxpooling, followed by 1 layer of fully connected hidden layer, finally a softmax layer. Specifically, the first conv-maxpooling layer uses conv filter (5,5), maxpooling (2,2); the 2nd conv-maxpooling layer uses conv filter (5,5), maxpooling (2,2); the 3rd conv-maxpooling layer uses conv filter (4,4), maxpooling (2,2); the last one uses conv filter (3,3), maxpooling (2,2). The hidden layer outputs 500 images to feed into the softmax layer. The hidden layer also has 0.5 dropout rate.</p>
<pre><code>layer_h0_input = layer3.output.flatten(2)
dropout = 0.5
# construct a fully-connected sigmoidal layer
layer_h0 = HiddenLayer(
        rng,
        input=layer_h0_input,
        n_in=nkerns[-1] * 4 * 4,
        n_out=500,
        activation=sigmoid,
        srng=srng,
        dropout=dropout,
        W=None if param_values is None else param_values['layer_h0_W'],
        b=None if param_values is None else param_values['layer_h0_b'],
        name='layer_h0'
    )

# classify the values of the fully-connected sigmoidal layer
layer_logistic = LogisticLayer(
        input=layer_h0.output,
        n_in=500,
        n_out=n_classes,
        W=None if param_values is None else param_values['layer_logistic_W'],
        b=None if param_values is None else param_values['layer_logistic_b'],
        name='layer_logistic'
    )

layer_logistic_validate = LogisticLayer(
        input=layer_h0.output_shrunk,
        n_in=500,
        n_out=n_classes,
        W=layer_logistic.W,
        b=layer_logistic.b,
        name='layer_logistic_validate'
    )
</code></pre>
<p>whereas for the Logistic Layer, <code>output</code> uses the Theano RandomStreams for binomial sampling, <code>output_shrunk</code> turns off binomial activation but multiplies the sigmoidal output by the dropout rate.</p>
<pre><code>class HiddenLayer(object):
    def __init__(self, rng, input, n_in, n_out, W=None, b=None,
                 activation=None, srng=None, dropout=0.0, name=None):
        ....
        prob_retain = 1 - dropout
        if dropout > 1e-8:
            self.output = self.output_ * srng.binomial(size=(n_out,),
                                                      p=prob_retain)
            self.output_shrunk = self.output_ * prob_retain
        else:
            self.output = self.output_
            self.output_shrunk = self.output_
        ....
</code></pre>
<p>When we train the network we use <code>layer_logistic</code>. For calculation of validation score, we use <code>layer_logistic_validate</code>. Note both layers share the same weight and bias matrices.</p></li>

<li><p>The transform matrices are initialised with random orthogonal matrices. The optimisation is done with SGD-momentum.</p>
<pre><code>param_deltas = [theano.shared(param.get_value() * 0.0,
                                  broadcastable=param.broadcastable)
                    for param in params]

param_delta_updates = [
        (delta_i, clipped(rho * delta_i - eta * grad_i))
        for delta_i, grad_i in zip(param_deltas, grads)
        ]

updates = param_delta_updates + [
        (param_i, param_i + clipped(rho * delta_i - eta * grad_i))
        for param_i, delta_i, grad_i in zip(params, param_deltas, grads)
        ]
</code></pre>
<p>We use L2 regularisation.
<pre><code># create a list of all model parameters to be fit by gradient descent
params = layer_logistic.params + layer_h0.params \
        + layer3.params + layer2.params + layer1.params + layer0.params

L2_norm = sum([T.sum(p_ ** 2) for p_ in params])
cost = layer_logistic.negative_log_likelihood(y2) + C * L2_norm
</code></pre></p>
<p>We split 75% of data for train set; 25% for validate set. Whenever the validate set scores a drop in the best error metric, the current parameter values are saved once. We first train the network with learning rate 0.04 for 2-4 epochs, then with learning rate 0.02 for 200 epochs, we half the learning rate every 20 epochs, till 0.0001. The regularisation constant <code>C</code> needs to be reduced as well as the error metric drops and the total parameter L2-norm increases.</p></li></ol>

Currently I could train the net to error metric 1.16. It's still ongoing work. It remains to experiment with deeper convolutional layers, maxout for model regularisation and the Knowledge Transfer approaches as described in [Sander Dieleman's post][1].

[1]: http://benanne.github.io/2015/03/17/plankton.html
