---
title: C LAPACK Libraries
layout: post
---

<script type="text/javascript" src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config">MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});</script>

LAPACK, as BLAS, is written in Fortran. There're two types of C interface for the LAPACK library: CLAPACK, which is direct translation of the Fortran interfaces into C; LAPACKE, the newer standard sponsored by Intel.

macOS Accelerate and ATLAS use the CLAPACK interface. Netlib, MKL, OpenBLAS support the LAPACKE interface. It's worth noting that ATLAS only support a tiny fraction of LAPACK functions, excluding QR Decompsition, SVD, etc, which makes it almost unuseable. As of the 16.04 Release, the Ubuntu `libopenblas-dev` package didn't package in `lapacke.h` -- one has to build from the OpenBLAS's GitHub repo to have it.

As an example, to do SVD on row-major matrices, with LAPACKE,

```c++
#if MKL     // For MKL
extern "C" {
#include <mkl.h>
}
#else       // For Netlib, OpenBLAS
extern "C" {
#include <lapacke.h>
}
#endif

// ...

template <>
inline int gesdd<cpu, double>(char jobz, int m, int n, double *a, int lda,
    double *s, double *u, int ldu, double *vt, int ldvt) {
  return LAPACKE_dgesdd(LAPACK_ROW_MAJOR, jobz, m, n, a, lda,
      s, u, ldu, vt, ldvt);
}
```

with CLAPACK,

```c++
extern "C" {
#include <clapack.h>
}

// ...

template <>
inline int gesdd<cpu, double>(char jobz, int m, int n, double *a, int lda,
    double *s, double *u, int ldu, double *vt, int ldvt) {
  int status, info;

  double *work = new double[10];
  int lwork = -1;
  int *iwork = new int[8 * std::min(m, n)];

  status = dgesdd_(&jobz, &n, &m, a, &lda,
      s, vt, &ldvt, u, &ldu,
      work, &lwork, iwork, &info);
  if (status != 0) {
    delete [] work;
    delete [] iwork;
    return status;
  }

  lwork = static_cast<int>(work[0]);
  delete [] work;
  work = new double[lwork];

  status = dgesdd_(&jobz, &n, &m, a, &lda,
      s, vt, &ldvt, u, &ldu,
      work, &lwork, iwork, &info);

  delete [] work;
  delete [] iwork;
  return status;
}
```
