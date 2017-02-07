---
title: Start a Cluster of MXNet Parameter Server
layout: post
---

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

MXNet comes with the Parameter Server Lite `ps-lite`. Normally the Parameter Server is started by `tools/launch.py` and solely used for gradient reducing during the SGD training. In this case, all the programs run on worker nodes share the same list of arguments. 

However if one would like to use it for Map-Reduce operations, one needs to explicit start the cluster and run programs with slightly different list of arguments so that each program knows what portion of data to compute on.

There're three roles for PS nodes: scheduler, server and worker. We start the scheduler first, then $$m$$ servers, and finally $$n$$ workers.

On the scheduler node:

```bash
export DMLC_ROLE=scheduler DMLC_NUM_SERVER=<m> DMLC_NUM_WORKER=<n> DMLC_PS_ROOT_URI=<scheduler_ip> DMLC_PS_ROOT_PORT=<scheduler_port>
export PS_VERBOSE=1
python3 -c "import mxnet"
```

On each server node:

```bash
export DMLC_ROLE=server DMLC_NUM_SERVER=<m> DMLC_NUM_WORKER=<n> DMLC_PS_ROOT_URI=<scheduler_ip> DMLC_PS_ROOT_PORT=<scheduler_port>
export PS_VERBOSE=1
python3 -c "import mxnet"
```

On either scheduler or server node, after executing the last line it'll hang up waiting for network connections.

On each worker node:

```bash
export DMLC_ROLE=worker DMLC_NUM_SERVER=<m> DMLC_NUM_WORKER=<n> DMLC_PS_ROOT_URI=<scheduler_ip> DMLC_PS_ROOT_PORT=<scheduler_port>
export PS_VERBOSE=1
ipython3 / python3 <.py> 
```

On worker nodes we could run Python code to interact with the PS servers. For example,

```python
import mxnet as mx

s = mx.kvstore.create('dist_sync')
s.init(<key>, <value>)
s.push(<key>, <value>)

a = <mxnet ndarray of same dimension>
s.pull(<key>, out=a)

a.asnumpy()
```

We have to run the `init()` and `push()` on each worker node before `pull()`. `a.asnumpy()` is blocking as the KVStore's mode is `dist_sync`.

