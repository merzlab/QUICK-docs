Known Issues of Current Version
===============================

QUICK is under continous development and as of version 20.03, we have detected 
issues listed below. If you find anything other than these, please feel free to 
report through our git hub repository: `https://github.com/merzlab/QUICK/issues <https://github.com/merzlab/QUICK/issues>`_. 

Installation 
^^^^^^^^^^^^

1. BLAS object linking error in cuda version
********************************************

::

 ./obj/scf.o: In function `electdiis_':
 scf.f90:(.text+0x3eb7): undefined reference to `dgemm_'

If you encounter this linking error in cuda compilation, you are most likeley to have 
compiled QUICK cpu or mpi version and trying to compile the cuda version without running  
*make clean* in between. Note that cuda version uses CUBLAS which is provided within src
folder and does not use BLAS. The latter is only used in CPU and MPI versions.

Solution: Run *make clean* and recompile using *make quick.cuda*.


2. Atomic add function overload error
*************************************

::

 gpu_getxc.cu(636): error: no instance of overloaded function "atomicAdd" matches the argument list
            argument types are: (double *, double)

 gpu_getxc.cu(637): error: no instance of overloaded function "atomicAdd" matches the argument list
            argument types are: (double *, double)

 gpu_getxc.cu(638): error: no instance of overloaded function "atomicAdd" matches the argument list
            argument types are: (double *, double)

 gpu_getxc.cu(2563): error: no instance of overloaded function "atomicAdd" matches the argument list
            argument types are: (double *, double)

 gpu_getxc.cu(2564): error: no instance of overloaded function "atomicAdd" matches the argument list
            argument types are: (double *, double)

 gpu_getxc.cu(2565): error: no instance of overloaded function "atomicAdd" matches the argument list
            argument types are: (double *, double)

 gpu_getxc.cu(2574): error: no instance of overloaded function "atomicAdd" matches the argument list
            argument types are: (double *, double)

 gpu_getxc.cu(2575): error: no instance of overloaded function "atomicAdd" matches the argument list
            argument types are: (double *, double)

 gpu_getxc.cu(2576): error: no instance of overloaded function "atomicAdd" matches the argument list
            argument types are: (double *, double)

 9 errors detected in the compilation of "/tmp/tmpxft_0004a02d_00000000-6_gpu_getxc.cpp1.ii".

You are trying to compile QUICK for a GPU architecture that does not support double precision atomicAdd
operators. 

Solution: Run *make clean* and recompile code for Pascal, Volta or Turing architecture. We are still
working on getting QUICK to work on older architectures.  

Runtime 
^^^^^^^

1. Input file open error
**************************

::

 Error: Failed to open Unit=  15,file name=
 At line 181 of file quick_molspec_module.f90
 Fortran runtime error: Attempt to DEALLOCATE unallocated 'xyz'

You are trying to run quick without an input file. 

Solution: Run quick executable as "./quick input.in". See hands on tutorial section for more information. 

2. Basis file open error
************************

::

 Error: Failed to open Unit=  20,file name= /6-31GSS
 At line 326 of file quick_calculated_module.f90
 Fortran runtime error: Attempt to DEALLOCATE unallocated 'self'

You havent set the basis file path. 

Solution: Set QUICK_BASIS variable using "export QUICK_BASIS=your_quick_home/basis".

3. Missing basis set information 
********************************

If your calculation unexpectedly crashes during SAD initial guess and you dont see any error message in output or
terminal, you are likely to have used a basis set that does not have information for a particular atom.

::

 
 For Atom Kind =    3
 ELEMENT = S
 BASIS FUNCTIONS =    0

 @ Begin Energy Calculation 

For example, the above calculation crashed while trying to use a basis set that doesnt have sulfur.

If you are using serial or MPI version, you may also see a fortran error on the terminal. 

::

 Backtrace for this error:
 #0  0x2b4cf76ba27f in ???
 #1  0x2b4cf76ba207 in ???
 #2  0x2b4cf76bb8f7 in ???
 #3  0x2b4cf76fcd26 in ???
 #4  0x2b4cf7705488 in ???
 #5  0x41fa34 in fullx_
        at src/quick_one_electron_integral.f90:26
 #6  0x4232bc in getenergy_
        at src/getEnergy.f90:29
 #7  0x406beb in getmolsad_
        at src/getMolSad.f90:112
 #8  0x403623 in quick
        at src/main.f90:123
 #9  0x403623 in main
        at src/main.f90:28

Solution: You should obtain basis set information for the missing atom from 
`https://www.basissetexchange.org/ <https://www.basissetexchange.org/>`_ in Gaussian basis file format
and paste this info into correct basis set file inside *basis* folder. Make sure to maintain the format. Otherwise, 
you will end up with the same error again. 

*Last updated by Madu Manathunga on 03/06/2020.*
