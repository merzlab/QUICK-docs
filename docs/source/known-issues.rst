Known Issues of Current Version
===============================

QUICK is under continous development and as of version 21.03, we have detected
the issues listed below. If you find anything other than these, please feel free to
report any bugs or issues through our Git Hub page: `https://github.com/merzlab/QUICK/issues <https://github.com/merzlab/QUICK/issues>`_.

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

Solution: Use a device with more global memory.

2. Accuracy of gradients on old gaming cards
********************************************

When computing gradients using CUDA/CUDAMPI versions on old gaming cards (eg. from maxwell and kepler microarchitectures), the resulting gradients may be less accurate if one uses the default density matrix cutoff (1.0E-6). 

Solution: Tighten the cutoff value to 1.0E-7 or 1.0E-8.

3. Wrong gradients in CUDAMPI version
*************************************

If one launches CUDAMPI version with a number of processes greater than the available GPUs on the system, it is possible to result in wrong gradients on some hardware. 

Solution: Always launch CUDAMPI version with a number of processes less than or equal to the available number of GPUs (i.e. mpirun -np N ;where N <= # of GPUs)  
  

*Last updated by Madu Manathunga on 03/18/2021.*
