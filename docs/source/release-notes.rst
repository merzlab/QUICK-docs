Release notes
^^^^^^^^^^^^^

The new features released with each QUICK version are as follows. 

QUICK-25.03
***********
• AMD GPU support restored for HIP/MPI+HIP codes (requires ROCm <= v5.4.2, >= v6.2.1 due to known ROCm bugs)
• Added Clang and NVHPC SDK (PGI) compiler support and fixes for MacOS builds
• GPU code improvements (refactoring to unify codes, reduce memory utilization, provide better error checking, and provide fixes)
• Updated SAD guesses to match those with QUICK-21.03 to fix SCF performance regression (faster convergence)
• Various other bug fixes, optimizations, and test updates (expanded automated CI testing on Github)
• QUICK-25.03 available with AmberTools 2025

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
