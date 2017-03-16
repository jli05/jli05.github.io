---
title: Code and Test on CUDA 
layout: post
---

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

## Coding on CUDA
1. Most memory-related functions work with memory length in bytes, e.g. `cudaMallocXxx`, `cudaMemcpyXxx`.

2. As CUDA Streams are widely used, whenever unsure and there's need, do synchronisation as `cudaDeviceSynchronize()`.

3. The CUDA Unified Memory is greatly handy. It can be allocated by `cudaMallocManaged()` and released by `cudaFree()`.

4. Below is a sample snippet for `nvcc` `gencode` instruction.

```make
# CUDA architecture setting: going with all of them.
# For CUDA < 6.0, comment the *_50 lines for compatibility.
CUDA_ARCH := -gencode arch=compute_30,code=sm_30 \
	-gencode arch=compute_35,code=sm_35 \
	-gencode arch=compute_50,code=sm_50 \
	-gencode arch=compute_50,code=compute_50
```

## Testing on CUDA
Below is a sample snippet of Makefile for making C++ test files for CUDA.

```make
ifeq ($(USE_CUDA), 1)
# All tests for GPU in *_test_gpu.cu
TEST_SRC_GPU = $(wildcard tests/cpp/*_test_gpu.cu)
TEST_GPU = $(patsubst tests/cpp/%_test_gpu.cu, tests/cpp/%_test_gpu, $(TEST_SRC_GPU))
override TEST += $(TEST_GPU)

tests/cpp/%_test_gpu : tests/cpp/%_test_gpu.cu lib/libmxnet.a
  $(NVCC) $(NVCCFLAGS) $(CUDA_ARCH) -Xcompiler "$(CFLAGS) -I$(GTEST_INC)" -M -MT tests/cpp/$*_test_gpu $< >tests/cpp/$*_test_gpu.d
  $(NVCC) -c $(NVCCFLAGS) $(CUDA_ARCH) -Xcompiler "$(CFLAGS) -I$(GTEST_INC)" -o $@.o $(filter %.cu, $^)
  $(CXX) -o $@ $@.o $(filter %.a, $^) -L$(GTEST_LIB) -lgtest $(LDFLAGS) -lcudart

endif
```


