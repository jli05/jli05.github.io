---
title: Kaggle 2015 National Data Science Bowl competition
layout: post
---

The competition is to classify 121-class sea plankton images. The winner Sander Dieleman has [a thorough post][1] about their winning strategy. This is the first time I join a deep learning competition so it was a good learning experience for me. The code is at [https://github.com/jli05/ndsb](https://github.com/jli05/ndsb).

# Image Preprocessing
The images come with different sizes, I resized them conserving the width-height ratio, then padded to size (120,120). As for certain species we have fewer than 10 images, for others we could have thousands, I produced a sample limiting the images to preprocess in each directory to 400 (if a directory contains more than 400 images, I choose randomly 400 images out of them); a second sample with all the images.

# Training with Convolutional Net
The code is derived from `convolutional_mlp.py` in the [Deep Learning Tutorial](http://deeplearning.net/tutorial/). I made the following changes:

1. At input, we spawn four copies of images from one input image. They are the originial one, the original one turned 90 degrees clock-wise, the original one turned 180 degrees clock-wise, the original one turned 270 degrees clock-wise.

2. The architecture is 4 layers of conv-maxpooling, followed by 1 layer of fully connected hidden layer, finally a softmax layer. Specifically, the first conv-maxpooling layer uses conv filter (5,5), maxpooling (2,2); the 2nd conv-maxpooling layer uses conv filter (5,5), maxpooling (2,2); the 3rd conv-maxpooling layer uses conv filter (4,4), maxpooling (2,2); the last one uses conv filter (3,3), maxpooling (2,2). The hidden layer outputs 500 images to feed into the softmax layer.

3. The transform matrices are initialised with random orthogonal matrices. The optimisation is done with SGD-momentum. As each image has many white pixels for the background, we also use L1 regularisation. We use 75% of data for train set; 25% for validate set. Whenever the validate set scores a drop in the best error metric, the current parameter values are saved in a `numpy` `.npy` file. We first train with the first sample with the number of images in each directory limited to 400. When it's finished, we read the optimal parameter values from the saved file, use them to initialise the net and continue the optimisation for the entire sample.

Currently I could train the net to error metric 1.37. It's still ongoing work. It remains to experiment with deeper convolutional layers, dropout for model regularisation and the Knowledge Transfer approaches as described in [Sander Dieleman's post][1].

[1]: http://benanne.github.io/2015/03/17/plankton.html
