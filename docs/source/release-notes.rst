Release notes
^^^^^^^^^^^^^

The new features released with each QUICK version are as follows. 

QUICK-24.03
***********
• Added ERI engine support for f basis functions to CUDA/MPI+CUDA codes (disabled be default)
• HIP/MPI+HIP support disabled, please use QUICK version 23.08b for HIP support
• Added initial Intel OneAPI/LLVM compiler support
• Added support for the following basis sets: aug-cc-pVTZ, def2-TZVPD, def2-TZVPP, aug-pc-1, pc-2, and aug-pc-2
• Various bug fixes, optimizations, and test updates

QUICK-23.03
***********
• Performance enhancements leading to 2x speedup of the GPU code compared to the previous release
• Support for AMD GPUs, Multi-GPU support via MPI + HIP, also across multiple compute nodes 
• Supports accounting for long range electrostatic interactions in QM/MM simulations under periodic boundary conditions with AmberTools v23 
• Supports data exporting to MOLDEN format 
• Supports Grimme's dispersion correction in DFT
• Configure based legacy build system is no longer supported

QUICK-22.03
***********
• Performance enhancements
• Support for spin-unrestricted calculations
• Efficient geometry optimizations using the DL-FIND geometry optimizer

QUICK-21.03
***********
• Performance enhancements
• Multi-GPU support via MPI + CUDA, also across multiple compute nodes
• API based QM/MM capability with AmberTools v21
• User friendly configure based and CMake based build systems 

QUICK-20.03
***********
• Support for GPU capable DFT calculations
• QM/MM capability with AmberTools v20 through file based interface

*Last updated by Madu Manathunga on 11/21/2022.*
