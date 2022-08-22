
We explain how you can build Sympiler and its iteration DAG partitioner from source. 



## Building Sympiler

### Pre-requisites
* CMake
* C/C++ compiler (gcc, icc, or clang)
* Fortran compiler (for compiling lapack)

Dependencies handled by CMake:

* OpenMP  : OpenMP is an optional dependency but without OpenMP, TBB is used internally.
* METIS : METIS dependency is already handled by CMake. 
* a BLAS Library : (MKL or OpenBLAS) is required 


### Installation
Please use the following:
```bash
git  clone --recursive https://github.com/sympiler/sympiler.git
cd sympiler
cmake -DCMAKE_BUILD_TYPE=Release  -S . -B build
cmake --build build --config Release -j 6 
```



## Building DAG partitioners:

### Pre-requisites
* CMake
* C/C++ compiler (gcc, icc, or clang)

Dependencies handled by CMake:

* METIS : METIS dependency is already handled by CMake. 

### Installation
If the installation paths of these libraries are in the system path, CMake should be able to handle dependencies. If not, you should set CMake variables as shown below:
```bash
git clone https://github.com/sympiler/aggregation.git
cd aggregation
cmake -DCMAKE_BUILD_TYPE=Release  -S . -B build
cmake --build build --config Release -j 6 
```




## How to use sympiler?
Sympiler can be used as a code generator or as a library.
The DAG partitioning algorithm used inside sympiler can also be used independently. 


C++ API interface is [here](sympiler-lib.md).


