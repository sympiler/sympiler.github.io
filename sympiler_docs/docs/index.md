Sympiler is a domain-specific code generator and library that optimizes sparse matrix computations by decoupling the symbolic analysis phase from the numerical manipulation stage in sparse codes. Symbolic analysis refers to extracting information from location of nonzeros in a sparse matrix. Sympiler includes a set of compile-time and runtime techniques that we provide an overview in this part. 


## Why Sympiler?
Sympiler addresses the challenge of irregular memory and indirect memory accesses in sparse computation through efficient use of compile-time and runtime information. Since compile-time analysis in these codes miss on several optimizations, Sympiler also organizes runtime information, particularly for when computation pattern will be reuse due to static sparsity pattern in an application.  


## Sympiler Internals 
Sympiler takes an input specification and generates an efficient parallel code using a set of compile-time transformation and also runtime inspection. We provide an overview of input and the Sympiler internals. 


### Input and Output
The input code to Sympiler specifies a set of sparse methods and their input matrix and vectors. 
Code implementing a sparse method is represented in a domain-specific abstract syntax tree (AST). Sympiler produces the output code by applying a series of lowering phases to the AST. Each lowering phase applies a inspector-guided tranformation at compile-time and builds a symbolic inspector associated with the transformation. 


### Compile-time inspector-guided transformations
The compile-time flow in Sympiler is shown in Figure, 
where a series of inspector-guided transformations are 
applied to the initial AST insided Sympiler. Each of these 
transformation, when applied, would require runtime 
information (obtained through runtime inspectors). 
Enabling these transformations are either directed by the user or 
chosen based on the input sparsity pattern of the matrix at runtime. 



### Runtime symbolic inspectors 
Runtime inspectors in Sympiler process location of nonzeros in a 
sparse matrix to extract information for optimize the code. 
The extracted information is called symbolic information and 
the inspectors are called symbolic inspectors. 
For each class of numerical algorithms with the same symbolic 
analysis approach, Sympiler uses a specific symbolic inspector 
to obtain information about the sparsity structure of the input matrix and stores it in an algorithm-specific way for use during executing the transfomred code (obtained at compile-time from here).
We classify the used symbolic inspectors based on the numerical
method as well as the transformations enabled by the obtained
information. For each combination of algorithm and transformation,
the symbolic inspector creates an inspection graph from the given
sparsity pattern and traverses it during inspection using a specific
inspection strategy. The result of the inspection is the inspection set,
which contains the result of running the inspector on the inspection
graph. Inspection sets are used to guide the transformations in
Sympiler. Additional numerical algorithms and transformations
can be added to Sympiler, as long as the required inspectors can be
described in this manner as well.




The current transformations proposed and prototyped in Sympiler are [Iteration-space Prunning](prune.md), [Loop Tiling](tiling.md), [Loop Fusion](fusion.md), and [Vectorization](vect.md).










