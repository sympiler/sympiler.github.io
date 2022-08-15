
To evaluate the performance of the Sympiler's generated code and compare it 
with other libraries and code generators, we created a set of benchmarks. 
The Sympiler benchmark compares the performance sparse BLAS kernels generated 
by Sympiler with other libraries. And the HDagg benchmark compares different 
aggregation techniques for a group of methods with loop-carried dependence. 


## Sympiler benchmark
Sympiler benchmark is available from [here](https://github.com/sympiler/sympiler-bench) 
and contains scripts and drivers to compare Sympiler with libraries 
such as MKL and Suitesparse for Cholesky factorization and sparse triangular solver. 


## HDagg benchmark 
The HDagg benchmark is available from [here](https://github.com/BehroozZare/HDagg-benchmark) and compares prior DAG 
partitioning or DAG scheduling methods with aggregation methods used 
in Sympiler. All methods are tested for the sparse triangular solver, Incomplete LU0, 
and Incomplete Cholesky. A detailed comparison between aggregation algorithms and the 
two aggregation algorithms in Sympiler is provided in the 
[HDagg paper](citation.md#hdagg).