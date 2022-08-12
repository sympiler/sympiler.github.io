
## Codelet mapping 
We vectorize a PSC with a parameterized vectorized rou-
tine that efficiently uses the SIMD instructions of the target
architecture, i.e. x86. The parameterization also allows us to
generate concise code invariant to the number of codelets that
are mined for a set of operations. The parameterized vectorized
routine takes the iteration space and access functions of a
codelet and data spaces of the kernel as input and vectorizes
all operations in the codelet. Listing 1 shows the parameterized
vectorized routine for a PSC-I and the multiply-add operation
with an unstrided access function h. For efficient use of
SIMD instructions, we implement a separate routine based on
strided properties of access functions. For a PSC I codelet, we
implement three routines, and in each routine one of f , g, h is
unstrided. For PSC II, we use three routines with one of f , g,
or h being strided per routine. To vectorize a list of codelets of
different types, a switch-case structure is used that selects the
parameterized vectorized routine associated with each codelet
type.



## Codelet mining

PSC mining creates a list of codelets from access functions
and the iteration space of a sparse kernel with the objective to
minimize the overall cost of the final codelet list. In this section
we first define PSC mining as a problem with its objective and
constraints and then propose a locality-based codelet mining
(LCM) heuristic to solve it. The extension of LCM to multi-
thread parallelism is also discussed