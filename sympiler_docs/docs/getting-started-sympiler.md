
 Here we explain how you can install NASOQ and use its C++ API to solve your QP problems.

## Pre-requisites
* CMake
* C++ compiler (gcc, icc, or clang)
* OpenMP (optional) : OpenMP is an optional dependency but without OpenMP, NASOQ runs sequentially.
* METIS (Optional) : METIS dependency is already handled by CMake. 
* a BLAS Library (MKL or OpenBLAS)


##Installation
If the installation paths of these libraries are in the system path, CMake should be able to handle dependencies. If not, you should set CMake variables as shown below:
```bash
git clone https://github.com/sympiler/sympiler.git
cd sympiler
mkdir build
cd build
cmake  -DCMAKE_BUILD_TYPE=Release ..
make
```


##Sympiler APIs and interfaces
Sympiler code generator is explained [here](sympiler-codegen.md).

C++ API and Eigen interface is [here](sympiler-lib.md).

Sympyler (the sympiler python interface) is presented [here](sympyler.md).

