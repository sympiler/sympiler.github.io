
Numerical algorithms and optimization methods are typically 
composed of numerous consecutive sparse matrix computations. 
For example, in iterative solvers such as Krylov methods, 
sparse kernels that apply a preconditioner or update 
the residual are repeatedly executed inside and between 
iterations of the solver. Separately optimizing such kernels 
creates synchronization and load imbalance between threads. 
Also, opportunities for data reuse between two sparse computations
might not be realized when sparse kernels are optimized
separately. Sympiler uses sparse fusion to fuse sparse methods 
for improving locality and thread-level parallelism. 
Sparse fusion has a transformation and an inspector. 

## Transformation
To generate the fused code, a fused transformation is applied
to the initial AST at compile time and two variants of the
fused code are generated, as shown below. The transformation 
variants are _separated_ and _interleaved_. The fused code
uses a reuse ratio at runtime to select the correct variant
for the specific input. The reuse ratio (`reuse_ratio`) specifies 
how much common data between the two loops exists. 
The variable fusion in line 2 of the two fused variants 
is set to `False` if the inspector determines fusion is not
profitable. The separated variant is selected when 
the reuse ratio is smaller than one. In this variant, iterations
of one of the loops run consecutively without checking the
loop type. The interleaved variant is chosen when the reuse
ratio is larger than one. In this variant, iterations of both
loops should run interleaved, and the variant checks the loop
type per iteration.


```
/// The input AST to the fusion transformation
Fuse:for(I1){//loop 1
 ...
 for(In)
  x[h(I1,...,In)] = a*y[g(I1,...,In)];
 }
Fuse:for(J1){//loop 2
 ...
 for(Jm)
  z[h’(J1,...,Jm)] = a*x[g’(J1,...,Jm)];
 }
```

```
/// Separated variant of the fusion transformation 
if(FusedSchedule.fusion && reuse_ratio < 1){
 for (every s-partition s){
 #pragma omp parallel for
  for (every w-partition w){
   for(v ∈ FusedSchedule[s][w].L1){//loop 1
    ...
    for(In)
     x[h(v,...,In)] = a*y[g(v,...,In)];
    }
   for(v ∈ FusedSchedule[s][w].L2){//loop 2
    ...
    for(Jm)
     z[h’(v,...,Jm)] = a*x[g’(v,...,Jm)];
    }
 
   }
  }
}
```

```
/// Interleaved variant of the fusion transformation
if(FusedSchedule.fusion && reuse_ratio >= 1){
 for (every s-partition s){
  #pragma omp parallel for
  for (every w-partition w){
   for(v ∈ FusedSchedule[s][w]){
    if(v.type == L1){//loop 1
     for(In)
      x[h(v.id,...,In)] = a*y[g(v,...,In)];
    } else {//loop 2
     for(Jm)
      z[h’(v.id,...,Jm)] = a*x[g’(v,...,Jm)];
    }
   }
  }
 }
}
```

## Inspector 
Sparse fusion uses the multi-sparse DAG partitioning (MSP)
algorithm to create an efficient fused partitioning that will
be used to schedule iterations of the fused code. MSP partitions 
vertices of the DAGs of the two input kernels to create
parallel load-balanced workloads for all cores while 
improving locality within each thread. 
The inputs to MSP are the dependency matrix between sparse methods, the DAG of each
method, and a reuse ratio. Sparse fusion analyzes the code of both methods,
available from its AST, to generate inspector components
that create these inputs.
MSP takes the inputs and goes through three steps of 
(1) vertex partitioning and partition pairing with the 
objective to aggregate iterations without joining the 
DAGs of the inputs kernels; (2) merging and slack vertex 
assignment to reduce synchronization and balance workloads; 
and (3) packing to improve locality.

A detailed explanation of sparse fuision is provided in [these references](citation.md#sparse-fusion).