Sympiler is a domain-specific code generator and library that optimizes sparse matrix computations by decoupling the symbolic analysis phase from the numerical manipulation stage in sparse codes. Sympiler includes a set of compile-time and runtime techniques that we provide an overview in this part. 


## Why Sympiler?
Sympiler addresses the challenge of irregular memory and indirect memory accesses in sparse computation through efficient use of compile-time and runtime information. Since compile-time analysis in these codes miss on several optimizations, Sympiler also organizes runtime information, particularly for when computation pattern will be reuse due to static sparsity pattern in an application.  


## Overview
Sympiler takes an input specification and generates an efficient parallel code using a set of compile-time transformation and also runtime inspection. We provide an overview of input and the Sympiler internals. 


### Input and Output
The input code to Sympiler specifies a set of sparse methods and their input matrix and vectors. 
Code implementing a sparse method is represented in a domain-specific abstract syntax tree (AST). Sympiler produces the internal code by applying a series of phases to this AST, transforming
the code in each phase. 



An abs The output code is an optimized parallel code of the input methods running on a multicore processor. 
The initial kernel is generated internally, what is the internal code?
then


### Compile-time inspector-guided transformations

The initial lowered code along with the inspection sets obtained by
the symbolic inspector are passed to a series of passes that further
transform the code. Sympiler currently supports two transforma-
tions guided by the inspection sets: Variable Iteration Space Pruning
and 2D Variable-Sized Blocking, which can be applied independently
or jointly depending on the input sparsity. As shown in Figure 2a,
the code is annotated with information showing where inspector-
guided transformations may be applied. The symbolic inspector
provides the required information to the transformation phases,
which decide whether to transform the code based on the inspection
sets. 



### Runtime symbolic inspectors 

Different numerical algorithms can make use of symbolic informa-
tion in different ways, and prior work has described run-time graph
traversal strategies for various numerical methods [ 12, 14 , 45, 52 ].
The compile-time inspectors in Sympiler are based on these strate-
gies. For each class of numerical algorithms with the same symbolic
analysis approach, Sympiler uses a specic symbolic inspector to
obtain information about the sparsity structure of the input ma-
trix and stores it in an algorithm-specic way for use during later
transformation stages.
We classify the used symbolic inspectors based on the numerical
method as well as the transformations enabled by the obtained
information. For each combination of algorithm and transformation,
the symbolic inspector creates an inspection graph from the given
sparsity pattern and traverses it during inspection using a specic
inspection strategy. The result of the inspection is the inspection set,
which contains the result of running the inspector on the inspection
graph. Inspection sets are used to guide the transformations in
Sympiler. Additional numerical algorithms and transformations
can be added to Sympiler, as long as the required inspectors can be
described in this manner as well.



The Prunning, Tiling, Fusion, and Vectorization techniques are explained in separate sections of this document.


## How to use sympiler?
Sympiler can be used as a code generator or as a library.
The DAG partitioning algorithm used inside sympiler can also be used independently. 
Sympiler also has a python interface, Sympyler that contains all routines in the sympiler library. 







