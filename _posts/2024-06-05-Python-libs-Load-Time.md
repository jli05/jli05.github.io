---
title: Python libs load time
layout: post
---

```sh
# numpy about 200 milliseconds
% echo 'import numpy' >tmp.py; time python3 tmp.py
python3 tmp.py  0.33s user 0.07s system 171% cpu 0.234 total
% echo 'import numpy' >tmp.py; time python3 tmp.py
python3 tmp.py  0.33s user 0.07s system 172% cpu 0.235 total
% echo 'import numpy' >tmp.py; time python3 tmp.py
python3 tmp.py  0.33s user 0.07s system 171% cpu 0.235 total

# scipy about 200 milliseconds
% echo 'import scipy' >tmp.py; time python3 tmp.py
python3 tmp.py  0.36s user 0.07s system 173% cpu 0.249 total
% echo 'import scipy' >tmp.py; time python3 tmp.py
python3 tmp.py  0.36s user 0.07s system 172% cpu 0.248 total
% echo 'import scipy' >tmp.py; time python3 tmp.py
python3 tmp.py  0.35s user 0.07s system 173% cpu 0.246 total

# pandas about 800 milliseconds
% echo 'import pandas' >tmp.py; time python3 tmp.py
python3 tmp.py  1.09s user 0.18s system 142% cpu 0.893 total
% echo 'import pandas' >tmp.py; time python3 tmp.py
python3 tmp.py  1.08s user 0.17s system 143% cpu 0.876 total
% echo 'import pandas' >tmp.py; time python3 tmp.py
python3 tmp.py  1.08s user 0.18s system 143% cpu 0.875 total

# matplotlib about 400 milliseconds
% echo 'import matplotlib' >tmp.py; time python3 tmp.py
python3 tmp.py  0.62s user 0.14s system 147% cpu 0.519 total
% echo 'import matplotlib' >tmp.py; time python3 tmp.py
python3 tmp.py  0.55s user 0.10s system 167% cpu 0.386 total
% echo 'import matplotlib' >tmp.py; time python3 tmp.py
python3 tmp.py  0.55s user 0.10s system 167% cpu 0.387 total

# tensorflow about 6.6 seconds
% echo 'import tensorflow' >tmp.py; time python3 tmp.py
2024-06-05 16:04:45.663927: I tensorflow/core/platform/cpu_feature_guard.cc:210] This TensorFlow binary is optimized to use available CPU instructions in performance-critical operations.
To enable the following instructions: AVX2 FMA, in other operations, rebuild TensorFlow with the appropriate compiler flags.
python3 tmp.py  6.57s user 0.65s system 107% cpu 6.692 total
% echo 'import tensorflow' >tmp.py; time python3 tmp.py
2024-06-05 16:04:53.896548: I tensorflow/core/platform/cpu_feature_guard.cc:210] This TensorFlow binary is optimized to use available CPU instructions in performance-critical operations.
To enable the following instructions: AVX2 FMA, in other operations, rebuild TensorFlow with the appropriate compiler flags.
python3 tmp.py  6.52s user 0.63s system 108% cpu 6.605 total
% echo 'import tensorflow' >tmp.py; time python3 tmp.py
2024-06-05 16:05:03.904113: I tensorflow/core/platform/cpu_feature_guard.cc:210] This TensorFlow binary is optimized to use available CPU instructions in performance-critical operations.
To enable the following instructions: AVX2 FMA, in other operations, rebuild TensorFlow with the appropriate compiler flags.
python3 tmp.py  6.55s user 0.64s system 108% cpu 6.647 total
```
