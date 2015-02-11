---
title: t-SNE's perplexity parameter
layout: post
---
When invoking t-SNE (in scikit-learn), note the `perplexity` parameter could impact the final result. [Laurens van der Maaten's page][1] explains that 

> Perplexity is a measure for information that is defined as 2 to the power of the Shannon entropy. The perplexity of a fair die with k sides is equal to k. In t-SNE, the perplexity may be viewed as a knob that sets the number of effective nearest neighbors. It is comparable with the number of nearest neighbors k that is employed in many manifold learners.'

[1]: http://lvdmaaten.github.io/tsne/

