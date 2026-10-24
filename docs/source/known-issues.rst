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

1. Incorrect results, runtime failure, performance issues
*********************************************************

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

3. SAD guess producing wrong results
************************************

The SAD guess refactored after QUICK-21.03 release produces wrong results.
The old SAD guess routines were replaced after release of QUICK-21.03.
This was done because the old SAD guess is extremely inefficient.
However, the new SAD guess produces wrong results. Thus, the users should only
use the basis sets provided in the User Manual. For those basis sets we have
precomputed the guess.

*Last updated by Vikrant Tripathy on 23/10/2026.*
