Known Issues of Current Version
===============================

QUICK is under continous development and as of the latest version, we have
detected the issues listed below. If you find anything other than these, please
feel free to report any bugs or issues through our GitHub page:
`https://github.com/merzlab/QUICK/issues <https://github.com/merzlab/QUICK/issues>`_.

Feel free to ask questions or start a discussion on the Discussions section of
our GitHub page: `https://github.com/merzlab/QUICK/discussions <https://github.com/merzlab/QUICK/discussions>`_.

Runtime
^^^^^^^

1. Incorrect results or runtime failure
***************************************

QUICK built with CUDA 12.8.1 shows incorrect results or runtime failures on RTX 5090.

Recommended CUDA versions
        • Hopper or older GPUs: CUDA <= v12.4.1 due to known performance regressions
        • Blackwell GPU: CUDA >= v13.0.0

2. Out of memory error in CUDA code
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

*Last updated by Vikrant Tripathy on 23/10/2026.*
