---
title: Distributed TensorFlow
layout: post
---

## References

[Distributed TensorFlow: A Gentle Introduction](http://amid.fish/assets/Distributed%20TensorFlow%20-%20A%20Gentle%20Introduction.html)

[Distriubted Computing with TensorFlow](https://learningtensorflow.com/lesson11/)

## How to Set Up
Start multiple processes of `tf.train.Server`s in the background:

```python
import tensorflow as tf
from multiprocessing import Process

cluster = tf.train.ClusterSpec({'local': ['localhost:2222', 'localhost:2223']})

def worker(i):
    ''' Start Worker i '''
    server = tf.train.Server(cluster, job_name='local', task_index=i)
    server.start()
    server.join()

if __name__ == '__main__':
    n_tasks = cluster.num_tasks('local')
    proc = [None for i in range(n_tasks)]
    for i in range(n_tasks):
        proc = Process(target=worker, args=(i,), daemon=True)
        proc.start()
```

Then run a separate piece of code to connect to the servers:

```python
import tensorflow as tf

cluster = tf.train.ClusterSpec({'local': ['localhost:2222', 'localhost:2223']})

x = tf.constant(2)

with tf.device('/job:local/task:1'):
    y2 = x - 66

with tf.device('/job:local/task:0'):
    y1 = x + 300
    y = y1 + y2

with tf.Session('grpc://localhost:2222') as sess:
    print(sess.run(y))
```




