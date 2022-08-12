
We explain how you can build Sympiler from source. We also provide details for how to build DAG partitioners and codelet mining algorithm standalone. 

## Pre-requisites
* CMake
* C++ compiler (gcc, icc, or clang)

Dependencies handled by CMake:

* OpenMP  : OpenMP is an optional dependency but without OpenMP, methods run sequentially.
* METIS : METIS dependency is already handled by CMake. 
* a BLAS Library : (MKL or OpenBLAS) is required 


## Installation
If the installation paths of these libraries are in the system path, CMake should be able to handle dependencies. If not, you should set CMake variables as shown below:
```bash
git clone --recursive https://github.com/sympiler/sympiler.git
cd sympiler
mkdir build
cd build
cmake  -DCMAKE_BUILD_TYPE=Release ..
make
```



## Getting started with DAG partitioners:





## Sympiler APIs and interfaces


C++ API and Eigen interface is [here](sympiler-lib.md).


