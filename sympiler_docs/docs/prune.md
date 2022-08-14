Variable Iteration Space Pruning (VI-Prune) prunes the iteration space of a loop 
using information about the sparse computation. The iteration space for sparse codes 
can be considerably smaller than that for dense codes, since the computation needs 
to only consider iterations with nonzeros. The inspection stage of Sympiler generates 
an inspection set that enables transforming the unoptimized sparse code to a code with a
reduced iteration space.


## Transformation

The VI-Prune transformation can be applied at a particular loop-level to 
the sparse code to transform it as shown below. The loop that should be pruned
is annotated with `PRUNE` internally. In the transformed code the
iteration space is pruned to `pruneSetSize`, which is the inspection set size. 
In addition to the new loop, all references to Ik (the loop index before 
transformation) are replaced by its corresponding value from the inspection set, 
pruneSet[Ik]. 



```
/// Input code (as an internal AST)
for(I1){
 .
  .
Prune: for(Ik < m) {
   .
   .
   for(In (Ik , ..., In−1)) {
    a[idx(I1,...,Ik ,...,In )];
   }
  }
}
```

```
/// After applying the VI-Prune transformation  
for(I1){
 .
 .
 for(Ik < pruneSetSize) {
  J = pruneSet[Ik];
  .
  .
  for(In (J,..., In−1)) {
   a[idx(I1,...,J,...,In )];
  }
 }
}
```


## Inspector 

The VI-Prune inspector, the dependency graph of loop is
traversed using depth first search (DFS) to determine the new pruned 
iteration space, i.e. `pruneSet` and `pruneSetSize`. The dependence 
graph computation can be either done with code analysis or using domain 
information. Sympiler uses domain information, such as knowing the 
sparse method, to create the dependence graph. 

More information and examples are available from [here](citation.md#sympiler)