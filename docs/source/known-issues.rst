Known Issues of Current Version
===============================

QUICK is under continous development and as of the latest version, we have
detected the issues listed below. If you find anything other than these, please
feel free to report any bugs or issues through our GitHub page:
`https://github.com/merzlab/QUICK/issues <https://github.com/merzlab/QUICK/issues>`_.

Compile time
^^^^^^^^^^^^

1. Compiling CUDA/MPI+CUDA versions for Kepler GPUs using CUDA v11.0 or higher
******************************************************************************

NVIDIA has dropped support for sm_30 microarchitecture starting from CUDA
v11.1, and support for sm_35 amd sm_37 starting from CUDA v12.0.  However,
QUICK build systems still use sm_30/sm_35/sm_37 flags in NVCC compiler flag
list for CUDA v11 or higher toolkits if the user specifies Kepler as target
microarchitecture (i.e. --arch kepler, -DQUICK_USER_ARCH=kepler in legacy and
CMake builds respectively). This will lead to a compile time error.

Solution: Manually ensure that the installed CUDA version supports the correct
Kepler targeted microarchitectures (<= v11.0 for sm_30, <= v11.8 for
sm_35/sm_37).  Please consult the Release Notes for your installed CUDA SDK
version for further details on supported GPU microarchitectures.

2. Compiling HIP/MPI+HIP versions fails for this release (unsupported)
**********************************************************************
HIP/MPI+HIP support disabled for this release due to required GPU code rewrites
(related to added f basis function support).

Solution: Use QUICK v23.08b for HIP/MPI+HIP support until support is restored.

Runtime
^^^^^^^

1. Out of memory error in CUDA code
***********************************

.. code-block:: none

 cudaMalloc cuda_buffer_type :: Allocate failed!: out of memory in gpu_type.h at line 590

or

.. code-block:: none

 gpu_get2e.cu.getGrad.358: 0x2 (out of memory)

The above memory allocation errors are observed in CUDA version when the
available global memory of the device is insufficient.  

Solution: Use a device with more global memory. Also make sure not to execute
other codes on the GPU while running QUICK.

2. Accuracy of gradients on old gaming cards
********************************************

When computing gradients using CUDA/MPI+CUDA versions on old gaming cards
(e.g., from Kepler and Maxwell microarchitectures), the resulting gradients may
be less accurate if the default density matrix cutoff (1.0E-6) is used. 

Solution: Tighten the cutoff value to 1.0E-7 or 1.0E-8.

3. Wrong gradients in MPI+CUDA version
**************************************

If one launches MPI+CUDA version with a number of processes greater than the
available GPUs on the system, it is possible to result in wrong gradients on
some hardware. 

Solution: Always launch the MPI+CUDA version with a number of processes less
than or equal to the available number of GPUs (i.e. mpirun -np N ; where N <= #
of GPUs)  
  

*Last updated by Madu Manathunga on 03/03/2022.*
