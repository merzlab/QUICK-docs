.. include:: quick_docs_common.rst

Features and Limitations
^^^^^^^^^^^^^^^^^^^^^^^^

|QUICK_VERSION| has the following features and limitations.

Features
********

• Hartree-Fock theory
• Density functional theory (LDA, GGA and Hybrid-GGA functionals)
• Grimme type dispersion corrections
• Restricted closed-shell and unrestricted open-shell wavefunctions
• Gradient and geometry optimization calculations
• Mulliken charge analysis
• Exports Molden format for visualization of geometry and orbital data
• Wide range of popular Gaussian basis sets included
• Supports QM/MM calculations with Amber22 and later
• Fortran API to use QUICK as QM energy and force engine
• Message Passing Interface (MPI) distributed parallelization for CPU platforms
• Massively parallel, single GPU implementation via CUDA and HIP for NVIDIA and AMD GPUs
• Distributed, multi-node, multi-GPU support via MPI+CUDA/MPI+HIP codes

Limitations
***********

• Supports energy/gradient calculations with basis functions up to f
• GPU f function code is not highly optimized, requires large amount of RAM (may fail on consumer GPUs)
• No open shell gradients with f functions on GPUs
• Supports only Cartesian basis functions (no spherical harmonics)
• Effective core potentials (ECPs) are not supported
• DFT calculations are performed exclusively using the SG1 grid system
• No meta-GGA nor range-separated hybrid functionals are supported at present

*Last updated by Andreas Goetz on 04/25/2024.*
