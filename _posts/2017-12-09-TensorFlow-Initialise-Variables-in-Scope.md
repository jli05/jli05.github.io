---
title: TensorFlow Initialise Variables in Variable Scope
layout: post
---

<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

In most tutorials the variables initialisation is done with

```python
sess.run(tf.global_variables_initializer())
```

However if we're running training of multiple models in a Session, `global_variables_initializer()` would re-initialise all variables even if they have been trained and set. To initialise variables within a specific scope,

``` python
var_list = tf.get_collection(tf.GraphKeys.GLOBAL_VARIABLES,
                             scope=variable_scope)
sess.run(tf.variables_initializer(var_list))
```

