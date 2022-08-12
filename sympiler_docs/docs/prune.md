Variable Iteration Space
Pruning (VI-Prune) prunes the iteration space of a loop using infor-
mation about the sparse computation. The iteration space for sparse
codes can be considerably smaller than that for dense codes, since
the computation needs to only consider iterations with nonzeros.
The inspection stage of Sympiler generates an inspection set that
enables transforming the unoptimized sparse code to a code with a
reduced iteration space.


## Variable Iteration Space pruning transformation

Given this inspection set, the VI-Prune transformation can be
applied at a particular loop-level to the sparse code to transform
it from Figure 3a to Figure 3b. In the gure, the transformation is
applied to the kt h loop nest in line 4. In the transformed code the
iteration space is pruned to pruneSetSize, which is the inspec-
tion set size. In addition to the new loop, all references to Ik (the
loop index before transformation) are replaced by its correspond-
ing value from the inspection set, pruneSet[Ip ]. Furthermore, the
transformation phase utilizes inspection set information to anno-
tate specic loops with further low-level optimizations to be applied
by later stages of code generation. These annotations are guided by
thresholds that decide when specific low-level optimizations result
in faster code.






## Prune Inspector 

In the symbolic inspector, the dependency graph DGL is
traversed using depth first search (DFS) to determine the inspection