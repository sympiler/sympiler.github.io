The Sympiler library is built from the Sympiler-generated codes and has a collection of inspector/executor routines for a wide range of sparse linear algebra kernels. We call them sparse BLAS routines. Additionally, algorithms used in Sympiler's inspectors, such as aggregation methods, can be independently used. Here we explain the C++ interfaces of these routines.




## Sparse BLAS


### Sparse lower triangular solver (SpTRSV):

* Description: 
solving linear system $Lx=b$ where $L$ is a sparse lower triangular matrix and $x$ and $b$ are dense vectors.

* Inspector: Builds the inspection set for SpTRSV for a given matrix $L$ in CSC format.
```
int get_coarse_levelSet_DAG_CSC_tree(size_t n,
	int *lC, /* column pointer for CSC  */
	int *lR, /*  row indices for CSC */
	int stype, /* 0 means general matrix, -1 means lower half, 1 means upper half */

	int &level_no, /* number of l- or s- partitions (coarsened wavefronts)
	int *&level_ptr, /* pointer to beginning of a coarsened wavefront */
	int &partNo, /* not needed*/
	int *&par_ptr, /* specifying the beginning of a w-partition in the partition array */
	int *&partition, /* array of iteration numbers */

	int innerParts, /* number of threads */
	int minLevelDist, /*tuning parameter, often an integer in range [-4,4]*/
	int divRate, /* tuning parameter, often between [2,10] or a very large value like 4000 */
	double *nodeCost /* cost of each iteration */
	)
```


* Executor: given the inspection set from the inspector, SpTRSV is performed by calling:
```
sptrsv_csr_lbc(
 	int n, /* matrix dimension */
 	int *Lp, /* row pointer for CSR or column pointer for CSC */
 	int *Li, /* column indices for CSR and row indices for CSC */
    double *Lx, /* Matrix values */
	double *x, /* As input has the values of $b$, and after execution has the solution $x$ */
    int level_no, /* Inspector information */
    int *level_ptr, 
    int *par_ptr, 
    int *partition
    )
```
If $L$ is stored in CSC format, then you should call `sptrsv_csc_lbc`. 

### Sparse Cholesky factorization
* Description: decomposes a sparse symmetric positive definite matrix $A$ into $L L^T$, 
where $L$ is a sparse lower triangular matrix.

* Inspector: Creates an object and stores symbolic information internally (see an example [here](https://github.com/sympiler/sympiler-bench/blob/main/sym_interface/cholesky_demo.cpp)). 
```
 auto *sym_chol = new sym_lib::parsy::SolverSettings(H);
 sym_chol->symbolic_analysis();

```

* Executor: The computed factor is stored internally and can be used by calling `sym_chol->solve_only()`.
```
sym_chol->numerical_factorization();
```

### Sparse incomplete LU (SpILU0)
* Description: approximately decomposes a sparse symmetric matrix $A$ into $L U$, 
where $L$ and $U$ are sparse lower and upper triangular matrices, respectively. Sparsity 
pattern of $L$ and $U$ are the same lower and upper half of matrix $A$, respectively. 

* Inspector: The [SpTRSV inspector](#sparse-lower-triangular-solver-sptrsv) can be reused, the matrix type should be general for SpILU0. 

* Executor: 
```
 void spilu0_csr_lbc(
 	int n, /* matrix dimension */
 	int nnz, /* number of nonzeros */
 	const int *Ap, /* row pointer for CSR or column pointer for CSC */
 	const int *Ai, /* column indices for CSR and row indices for CSC */
    const double *Ax, /* Matrix values */
    const int *A_diag, /* location of diagonal elements in each row/col */
    double *l, /* the out factors stored as $L + U$ (Output)*/
    int level_no, /* Inspector information */
    const int *level_ptr,
    const int *par_ptr, 
    const int *partition,
    int *tempvecs /* integer array with size n times number of threads */
    )
```

### Sparse incomplete Cholesky (SpIC0)
* Description: approximately decomposes a sparse symmetric positive definite matrix $A$ into $L L^T$, 
where $L$ is sparse lower triangular matrix, with the same sparsity pattern of the lower half of matrix $A$. 

* Inspector: The [SpTRSV inspector](#sparse-lower-triangular-solver-sptrsv) can be reused.


* Executor: performs an in-place decomposition.
```
 void spic0_csr_lbc(
 	int n, /* matrix dimension */
 	int *Lp, /* row pointer for CSR or column pointer for CSC */
 	int *Li, /* column indices for CSR and row indices for CSC */
    double *Lx, /* Matrix values (input/output) */
    int level_no, /* Inspector information */
    int *level_ptr,
    int *par_ptr, 
    int *partition,
    )
```



## Tiling and Aggregation

### Load-balanced wavefront coarsening 
* Description: creates a schedule by coarsening wavefronts of a DAG and then partition 
each coarsened DAG to independent (disjoint) partitions. 
* Usage: 
```
int get_coarse_levelSet_DAG_CSC_tree(size_t n,
	int *lC, /* column pointer for CSC  */
	int *lR, /*  row indices for CSC */
	int stype, /* 0 means general matrix, -1 means lower half, 1 means upper half */

	int &level_no, /* number of l- or s- partitions (coarsened wavefronts)
	int *&level_ptr, /* pointer to beginning of a coarsened wavefront */
	int &partNo, /* not needed*/
	int *&par_ptr, /* specifying the beginning of a w-partition in the partition array */
	int *&partition, /* array of iteration numbers */

	int innerParts, /* number of threads */
	int minLevelDist, /*tuning parameter, often an integer in range [-4,4]*/
	int divRate, /* tuning parameter, often between [2,10] or a very large value like 4000 */
	double *nodeCost /* cost of each iteration */
	)
```

### HDagg
* Description: creates a schedule by coarsening wavefronts of a DAG and then partition 
each coarsened DAG to independent (disjoint) partitions. 
* Usage: 
```

```

## Codelet mining
The details for using codelet mining, SpMV, and SpTRSV will come soon.

## Fusion
Not public yet. 






