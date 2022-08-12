

## Hiearchical Level coarsening code lowering (H-level)
To utilize the H-Level set to efficiently execute the schedule,
the original code must be transformed for parallelism. Figure 2
shows how the H-Level transformation modifies Cholesky
factorization1. As shown, the outermost loop in line 2 of
Figure 2a is transformed to lines 1–5 in Figure 2b. Since
in the left-looking Cholesky algorithm nodes do not write to
other nodes, the loop body does not change because no critical
region is required. The OpenMP pragma enables parallelism
over sub-DAGs, executing dependent nodes within the same
thread, which increases locality. For the example DAG in
Figure 1a, the outer loop in the code of Figure 2b executes only
one iteration, resulting in a single synchronization, compared
to the six synchronizations required by wavefront parallelism.
The available parallelism in a sparse algorithm is not
uniform and typically different approaches for parallelism
must be used to efficiently exploit the underlying parallel
architecture. For example, l-partition 1 in the partitioned DAG
in Figure 1b benefits from tree parallelism; however, the nodes
in l-partition 2, which contains the sync node (the node with
no outgoing edges), have no tree parallelism but such nodes
can be repartitioned to increase data parallelism within their
corresponding dense computations [23 ]. The last iteration,
which corresponds to the last partition of the H-Level set,
is peeled and optimized differently. For such nodes, ParSy
enables using parallel BLAS operations for the node; however,
ParSy can be extended to support more advanced specialization
techniques such as repartitioning





## Iteration tiling/aggregation inspector

There are two major aggregation algorithms implemented in Sympiler.  

### Load balance level coarsening (LBC)

The goal of Load-Balanced Level Coarsening is to find a
set of l-partitions, and within each l-partition, to find a set
of disjoint w-partitions with as balanced cost as possible. For
improved performance, these partitions adhere to additional
constraints to reduce synchronization between threads and
maintain load balance. Additionally, there are objective func-
tions for minimizing communication between threads and the
number of synchronizations between levels.





### Hierarchical Iteration DAG Aggregation (HDagg)



### Aggregation benchmark 

## Tile-block detection

2D Variable-Sized Blocking
(VS-Block) converts a sparse code to a set of non-uniform dense sub-
kernels. In contrast to the conventional approach of blocking/tiling
dense codes, where the input and computations are blocked into
smaller uniform sub-kernels, the unstructured computations and inputs in sparse kernels make blocking optimizations challenging.
The symbolic inspector identies sub-kernels with similar structure
in the sparse matrix methods and the sparse inputs to provide the
VS-Block stage with “blockable” sets that are not necessarily of the
same size or consecutively located. These blocks are similar to the
concept of supernodes [ 44] in sparse libraries.

The code transformation is domain specific and the general H-Level transformation can not be used. 