
Irregular computations, such as in sparse matrix codes,
frequently appear in scientific and machine learning problems.
The performance of these applications is noticeably improved
if their code is vectorized to exploit single instruction mul-
tiple data (SIMD) capabilities of the underlying architecture.
Vectorization potentially increases opportunities to optimize
for locality, further increasing performance. SIMD instructions
can efficiently vectorize groups of operations that access
consecutive data, i.e. have a strided access pattern.


Sympiler's vectorization transformation creates a set of codelets 
per an operation in the code and then uses a mining algorithm as 
its inspector to find codelets at runtime. A codelet is a polyhedral 
model for a group of operations where each operation has three access functions.
Codelets are classified into strided codelets (or BLAS) and partially strided codelets. 
These codelets differ on how many strided access functions they have. 
BLAS codelets have three strided access functions and partially strided codelets 
have one or two strided access functions. This classification enables 
performing any sparse method with a handful of codelets. 



## Transformation
Sympiler transforms the code so the mined codelets at runtime 
are mapped to the proper codelet type. 
Each codelet type, PSC or BLAS, is implemented with a parameterized 
vectorized routine that efficiently uses the SIMD instructions of the target
architecture, i.e. x86. The parameterization also allows us to
generate concise code invariant to the number of codelets that
are mined for a set of operations. The parameterized vectorized
routine takes the iteration space and access functions of a
codelet and data spaces of the kernel as input and vectorizes
all operations in the codelet. 
The cpde below shows how a `switch-case` statement is used to 
map a mined codelet from `clist` to one of the three parametrized
codes. The number of parameterized code per codelet types 
can increase for better efficiency. 

```
/// Codelet mapping transformation for sparse method with one operation such as SpMV
#include "Codeletsx86.h"

void vectorized_code(vector<Codlet*> clsit, CSR* A, double *x, double *y){
 for(int i = 0; i < clist.partitions(); i++){
#pragma omp parallel 
  for(int j = 0; j < clist[i].size(); j++){
   switch(clist[i][j].type){
    case BLAS:
     BLAS_codelet();
     break;
    case PSCI:
     PSCI_codelet();
     break;
    case PSCII:
     PSCII_codelet();
     break;
   }
  }
 }
}
```


## Inspector
Sympiler's vectorization inspector has a codelet mining algorithm 
that creates a list of codelets from access functions and the 
iteration space of a sparse kernel with the objective to
minimize the overall cost of the final codelet list. The list is 
computed at runtime and provided to the transformed code.  
The codelet mining uses a locality-based mining (LCM) algorithm that mines
operations and finds an efficient codelet combination that satisfies 
the correctness of the vectorized code and minimizes
the total cost of codelets. The LCM algorithm has two steps;
in the first step, LCM permutes operations to increase data
reuse and the possibility of creating efficient codelets from 
that list of operations. This step also groups operations that
have the most reuse into computation regions. The second
step of LCM searches for efficient codelets in consecutive
operations of each computation region. 