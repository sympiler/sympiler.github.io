The sympiler library is built from the sympiler generated code and has a collection of inspector/executor routines for wide range of sparse linear algebra kernels.


Here we explain C++ interfaces of the sympiler library.




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

### Sparse matrix-vector multiplication (SpMV)



## Tiling and Aggregation

### Load-balanced wavefront coarsening 
* Description
* Inputs
* outputs

### HDagg
* Description
* Inputs
* outputs


## Codelet mining
Coming soon.

## Fusion
Not public yet. 






