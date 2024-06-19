Configure Script Options
^^^^^^^^^^^^^^^^^^^^^^^^

This page gives a summary of options available in QUICK configure script. 

General
*******

• **--serial**: Builds a serial version. (Default)
• **--debug**: Compiles debug version.
• **--debug-time**: Compiles a debug version that reports more information on timing.
• **--ncores <ncore>**: Specify the number of cores to be used in compilation. If unspecified, QUICK will attempt to automatically detect the number of cores in your system.
• **--verbose**: Enable full compilation output.
• **--help**: Prints help.

External library control
************************

• **--prefix <dir>**: User specified installation directory.
• **--shared**: Build shared object libraries.
• **--amber**: Install QUICK executables and libaries into AMBER. Requires specifying AMBER home directory as prefix.
• **--lapack**: Use matrix diagonalizer from LAPACK. BLASROOT environment variable must be set with the correct blas installation path. This path must contain lib and include directories.
• **--mkl**: Use matrix diagonalizer from MKL. MKLROOT environment variable must be set with the correct mkl installation path. This path must contain lib and include directories.
• **--mirp**: Use Boys function for ERI calculations from mirp library. MIRP_HOME and MIRP_DEP_HOME environment variables must be set to mirp and dependency installation directories. These paths must contain lib and include directories.

Parallel versions
*****************

• **--mpi**: Compiles MPI parallel version.
• **--cuda**: Builds GPU version that utilizes a single NVIDIA GPU.
• **--cudampi**: Builds multi-GPU version that utilizes multiple NVIDIA GPUs.
• **--arch <kepler|maxwell|pascal|volta|turing|ampere>**: Specify gpu architecture. Applicable for CUDA and MPI+CUDA versions only. If unspecified, QUICK will be compiled for several architectures based on the CUDA toolkit version.
• **--enablef**: Enables the compilation of time consuming F functions in the ERI code of cuda version. **NOTE**: The current version of the F function code takes very long to compile (hours) and requires a large amount of RAM. Work is planned to optimize this in future releases.
• *-DCMAKE_BUILD_TYPE=<Debug|Release>*: Controls whether to build debug or release versions.

*Last updated by Madu Manathunga on 03/09/2022.*
