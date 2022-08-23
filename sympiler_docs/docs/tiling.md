
Sympiler aggregates iterations of a loop to create balanced 
thread-level parallelism and improve locality within a thread. 
A separate repository for aggregation is provided 
[here](https://github.com/sympiler/aggregation.git) to enable using it 
standalone. 
In addition to iteration aggregation, Sympiler also uses block-tiling 
for some sparse methods to enable locality and vectorization.


## Aggregation 
Tiling is performed in Sympiler using a Hierarchical wavefront transformation with 
an aggregation inspector. Sympiler has two aggregation inspectors, LBC and HDagg.


### Transformation (or Hierarchical wavefront transformation)
The parallel tile transformation, also named as hierarchical wavefront(level), 
tiles a specified loop to use the tiling information provided at runtime.
The two code snippets below show before and after the parallel tiling 
transformation. 
The loop in line 2 of the code before transformation is changed
to lines 2–5 in the parallel tiled code. After transformation,
all operations and indices that use `I1`, which is the index of
the transformed loop, will be replaced with a proper value
from `HLevelSet`. The parallel `pragma` in line 2 ensures that
all w-partitions within an l-partition run in parallel. Note that
some algorithms may require `atomic` pragmas, as shown in line 9 
of the parallel tiled code.



```
/// Input code (as an internal AST)
Tile: for(I1){
 .
  .
   .
    for(In(I1)){
Atomic: c /= a[idx(I1,...,In)]; 
    }

}
```

```
/// Tiled code after application of the parallel tiling transformation
for( every l−partition i ) {
 #pragma omp parallel for private (pVars)
 for( every w−partition j ) {
  for ( every v ∈ HLevelSet[i][j] ) {
   I1 = v ;
   . . .
   for( In(I1) ) {
   #pragma omp atomic
    c /= a [ idx( I1, ... , In ) ] ; 

   } 
  } 
 } 
} 
```



### Load balance level coarsening (LBC) inspector
The goal of Load-Balanced Level Coarsening is to find a
set of coarsened wavefronts ( or l-partitions), and within each coarsened wavefronts, to find a set
of disjoint w-partitions with as balanced cost as possible. For
improved performance, these partitions adhere to additional
constraints to reduce synchronization between threads and
maintain load balance. Additionally, there are objective functions 
for minimizing communication between threads and the
number of synchronizations between levels. The LBC is designed for 
chordal DAGs (A DAG where each vertex is part of a cycle of at most length three) and trees.
A detailed discussion of the algorithm is provided in the [ParSy paper](citation.md#parsy).





## Block-tiling 
Sympiler's block-tiling strategy (also known as 2D Variable-Sized Blocking) converts
 a sparse code to a set of non-uniform dense sub-
kernels. In contrast to the conventional approach of blocking/tiling
dense codes, where the input and computations are blocked into
smaller uniform sub-kernels, the unstructured computations and 
inputs in sparse kernels make blocking optimizations challenging. 
This enables vectorization through calling 
BLAS kernels. Block-tiling has a transformation and an inspector.

### transformation 
The block-tiling transformation is shown in the code below. 
The inner loop in line 3 transforms into two nested
loops (lines 3–7) that iterate over the block specified by the outermost
loop. The tile-blocking code transformation is domain-specific and the 
general H-Level transformation can not be used.  
```
/// The input AST to the block-tiling transformation
block-tile: for(I) {
 for(J) {
  B[idx1(I,J)] += a[idx2(I,J)];
 }
}
```


```
/// The block-tiled code after transformation
for(b < blockSetSize) {
 for(J1 < blockSet[b].x) {
  for(J2 < blockSet [b].y) {
   B[idx1(b,J1,J2)] += A[idx2(b, J1, J2)];
  }
 }
}
```

### inspector
The symbolic inspector identifies sub-kernels with similar structures
in the sparse matrix methods and the sparse inputs to provide the
VS-Block stage with _blockable_ sets that are not necessarily of the
same size or consecutively located. These blocks are similar to the
concept of supernodes (consecutive columns with the same pattern) in sparse libraries.
Examples for the tile-blocking transformation are provided in the [Sympiler paper](citation.md#sympiler).

