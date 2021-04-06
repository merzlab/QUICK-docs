Known Issues of Current Version
===============================

QUICK is under continous development and as of version 21.03, we have detected
the issues listed below. If you find anything other than these, please feel free to
report any bugs or issues through our Git Hub page: `https://github.com/merzlab/QUICK/issues <https://github.com/merzlab/QUICK/issues>`_.

Compile time
^^^^^^^^^^^^

1. Compiling CUDA/CUDAMPI version for K40 and K80 using CUDA/11.0 or higher
***************************************************************************

NVIDIA has dropped support for sm_30 microarchitecture starting from CUDA/11.0. Both QUICK build systems however, still use sm_30 flags
in NVCC compiler flag list for CUDA/11.0 or higher toolkits if the user specifies kepler as target microarchitecture (i.e. --arch kepler, 
-DQUICK_USER_ARCH=kepler in legacy and CMake builds respectively). This will lead to a compile time error.

Solution: Manually change -arch and -gencode flag values from sm_30 to sm_35 (for K40 gpu) or sm_37 (K80 gpu) in make input files.
In legacy build system, this change should be made in $QUICK_HOME/build/Make.cuda.in (for CUDA serial build) and $QUICK_HOME/build/Make.cudampi.in 
(for CUDAMPI build) after executing configure script but before running *make* or *make install* commands.  
In CMake build system, one has to replace sm_30 instances in files inside cmake build directory.

Runtime
^^^^^^^

1. Out of memory error in CUDA code
***********************************

.. code-block:: none

 cudaMalloc cuda_buffer_type :: Allocate failed!: out of memory in gpu_type.h at line 590

or

.. code-block:: none

 gpu_get2e.cu.getGrad.358: 0x2 (out of memory)

The above memory allocation errors are observed in CUDA version when the available global memory of the device is insufficient.  

Solution: Use a device with more global memory. Also make sure not to execute other codes on the GPU while running QUICK.

2. Accuracy of gradients on old gaming cards
********************************************

When computing gradients using CUDA/CUDAMPI versions on old gaming cards (eg. from maxwell and kepler microarchitectures), the resulting gradients may be less accurate if one uses the default density matrix cutoff (1.0E-6). 

Solution: Tighten the cutoff value to 1.0E-7 or 1.0E-8.

3. Wrong gradients in CUDAMPI version
*************************************

If one launches CUDAMPI version with a number of processes greater than the available GPUs on the system, it is possible to result in wrong gradients on some hardware. 

Solution: Always launch CUDAMPI version with a number of processes less than or equal to the available number of GPUs (i.e. mpirun -np N ;where N <= # of GPUs)  
  

*Last updated by Madu Manathunga on 03/18/2021.*
