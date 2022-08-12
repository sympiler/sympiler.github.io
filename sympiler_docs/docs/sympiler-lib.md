The sympiler library is built from the sympiler generated code and has a collection of inspector/executor routines for wide range of sparse linear algebra kernels.


Here we explain C++ interfaces of the sympiler library.

## DAG partitioning and scheduling


## Sparse BLAS


### Sparse lower triangular solver (SpTRSV):

* Description: 
solving linear system $Lx=b$ where $L$ is a sparse lower triangular matrix and $x$ and $b$ are dense vectors.

* Inspector:
inputs, outputs, description

* Executor:
inputs, outputs, description

### Sparse Cholesky factorization
* Description: 

* Inspector:

* Executor:


### Sparse incomplete LU (SpILU0)

### Sparse incomplete Cholesky (SpIC0)

### Sparse matrix scaling

### Sparse matrix-vetor multiplication (SpMV)