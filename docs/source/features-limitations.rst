Features and Limitations
^^^^^^^^^^^^^^^^^^^^^^^^

QUICK-21.03 has the following features and limitations.

Features
********

• Hartree-Fock energy calculations
• Density functional theory calculations (LDA, GGA and Hybrid-GGA functionals available).
• Gradient and geometry optimization calculations
• Mulliken charge analysis
• Wide range of basis sets included
• Supports QM/MM calculations with Amber21
• Fortran API to use QUICK as QM energy and force engine
• MPI parallelization for CPU platforms
• Massively parallel GPU implementation via CUDA for Nvidia GPUs
• Multi-GPU support via MPI + CUDA, also across multiple compute nodes

Limitations
***********

• Supports only closed shell systems
• Supports energy/gradient calculations with basis functions up to d
• Supports only Cartesian basis functions (no spherical harmonics)
• Effective core potentials (ECPs) are not supported
• DFT calculations are performed exclusively using SG1 grid system

*Last updated by Madu Manathunga on 03/18/2021.*
