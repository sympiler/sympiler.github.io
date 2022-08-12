

## Sparse fusion transformation
o generate the fused code, a fused transformation is applied
to the initial AST at compile-time and two variants of the
fused code are generated, shown in Figure 4. The transforma-
tion variants are separated and interleaved. The fused code
uses the reuse ratio at runtime to select the correct variant
for the specific input. The variable fusion in line 1 of Fig-
ure 4b and 4c is set to False if MSP determines fusion is not
profitable. Figure 4a shows the sequential loops in the AST,
which are annotated with Fuse, and are transformed to the
separated and interleaved code variants as shown in order
in Figures 4b and 4c. The separated variant is selected when the reuse ratio is smaller than one. In this variant, iterations
of one of the loops run consecutively without checking the
loop type. The interleaved variant is chosen when the reuse
ratio is larger than one. In this variant, iterations of both
loops should run interleaved, and the variant checks the loop
type per iteration as shown in lines 6 and 10 in Figure 4c.



## Multi-DAG Iteration Partitioning
The MSP algorithm requires kernel-specific inputs. Its inputs
are the dependency matrix between kernels, the DAG of each
kernel, a reuse ratio. Sparse fusion analyzes the kernel code,
available from its AST, to generate inspector components
that create these inputs.
Dependency DAGs: Lines 6–7 in Figure 3b use an internal
domain-specific library to generate the dependency DAG 



Sparse fusion uses the multi-sparse DAG partitioning (MSP)
algorithm to create an efficient fused partitioning that will
be used to schedule iterations of the fused code. MSP parti-
tions vertices of the DAGs of the two input kernels to create
parallel load-balanced workloads for all cores while improv-
ing locality within each thread. 
.


Algorithm 1 shows the MSP algorithm. It takes the inputs
and goes through three steps of (1) vertex partitioning and
partition pairing with the objective to aggregate iterations
without joining the DAGs of the inputs kernels; (2) merging
and slack vertex assignment to reduce synchronization and
to balance workloads; and (3) packing to improve locality.