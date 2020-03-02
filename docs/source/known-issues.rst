Known Issues of Current Version
===============================

QUICK is under continous development and as of version 20.02, we have detected 
issues listed below. If you find anything other than these, please feel free to 
report through our git hub repository <https://github.com/merzlab/QUICK/issues>. 

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

