.. include:: quick_docs_common.rst

Features and Limitations
^^^^^^^^^^^^^^^^^^^^^^^^

|QUICK_VERSION| has the following features and limitations.

Features
********

• Hartree-Fock energy calculations
• Density functional theory calculations (LDA, GGA and Hybrid-GGA functionals available).
• Gradient and geometry optimization calculations
• Mulliken charge analysis
• Exports Molden format for visualization of geometry and orbital data
• Wide range of basis sets included
• Supports QM/MM calculations with Amber22
• Fortran API to use QUICK as QM energy and force engine
• MPI parallelization for CPU platforms
• Massively parallel GPU implementation via CUDA and HIP for Nvidia and AMD GPUs 
• Multi-GPU support via MPI + CUDA/HIP, also across multiple compute nodes

Limitations
***********

• Supports energy/gradient calculations with basis functions up to d
• Supports only Cartesian basis functions (no spherical harmonics)
• Effective core potentials (ECPs) are not supported
• DFT calculations are performed exclusively using SG1 grid system

*Last updated by Andy Goetz on 11/21/2022.*
