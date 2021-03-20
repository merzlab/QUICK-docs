Known Issues of Current Version
===============================

QUICK is under continous development and as of version 21.03, we have detected
the issues listed below. If you find anything other than these, please feel free to
report any bugs or issues through our Git Hub page: `https://github.com/merzlab/QUICK/issues <https://github.com/merzlab/QUICK/issues>`_.

Runtime
^^^^^^^

1. Out of memory error in CUDA code
***********************************

::

 cudaMalloc cuda_buffer_type :: Allocate failed!: out of memory in gpu_type.h at line 590

or

::

 gpu_get2e.cu.getGrad.358: 0x2 (out of memory)

The above memory allocation errors are observed in CUDA version when the available global memory of the device is insufficient.  
Solution: Use a device with more global memory.

*Last updated by Madu Manathunga on 03/18/2021.*
