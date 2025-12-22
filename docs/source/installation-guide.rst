.. include:: quick_docs_common.rst

Installation Guide
==================

QUICK has been compiled and tested on x86 and ARM CPU architectures, and on
NVIDIA and AMD GPU architectures.

**NOTE:** For GPU builds, the compilation of the GPU-enabled two electron
repulsion integral (ERI) code can take a significant amount of time (several
minutes for default builds and several hours for f-function basis set support)
-- be patient as the compiler is working hard to generate highly-performant code.

Compatible Compilers and Hardware
---------------------------------

In general QUICK works well with a range of compilers (GNU, Clang, Intel, NVHPC
SDK/PGI), math libraries (Intel MKL, reference BLAS/LAPACK, MAGMA), MPI
implementations (OpenMPI, MPICH, Intel MPI), and GPU SDK versions (CUDA,
ROCm/HIP). |QUICK_VERSION| is automatically tested on Github with following
combinations of OS versions, compilers, libraries, and tools:

 - Ubuntu v22.04.05 (x86_64), GNU GCC v10.5.0; OpenMPI v4.1.2; CMake v3.31.6
 - Ubuntu v22.04.05 (x86_64), GNU GCC v11.4.0; OpenMPI v4.1.6; CMake v3.31.6
 - Ubuntu v24.04.2 (x86_64), GNU GCC v12.3.0; OpenMPI v4.1.6; CMake v3.31.6
 - Ubuntu v24.04.2 (x86_64), GNU GCC v13.3.0; OpenMPI v4.1.6; CMake v3.31.6
 - Ubuntu v24.04.2 (x86_64), GNU GCC v14.2.0; OpenMPI v4.1.6; CMake v3.31.6
 - Ubuntu v24.04.2 (x86_64), GNU GCC v14.2.0; MPICH v4.2.0; CMake v3.31.6
 - Ubuntu v24.04.2 (x86_64), Clang v17.0.6; OpenMPI v4.1.6; CMake v3.31.6
 - Ubuntu v24.04.2 (x86_64), Clang v18.1.3; OpenMPI v4.1.6; CMake v3.31.6
 - Ubuntu v24.04.2 (x86_64), Intel oneAPI v2024.2.1; Intel MPI (CCL) v2021.14; CMake v3.31.6
 - Ubuntu v24.04.2 (x86_64), Intel oneAPI v2025.0.1; Intel MPI (CCL) v2021.14; CMake v3.31.6
 - Ubuntu v24.04.2 (x86_64), NVIDIA HPC SDK v25.1 (PGI); OpenMPI v4.1.7rc1; CMake v3.31.6
 - Ubuntu v24.04.2 (ARM), GNU GCC v14.2.0; OpenMPI v4.1.6; CMake v3.31.6
 - Ubuntu v24.04.2 (ARM), GNU GCC v14.2.0; MPICH v4.2.0; CMake v3.31.6
 - MacOS 13 (x86_64), GNU GCC v14.2.0_1; OpenMPI v; CMake v3.31.6 (Homebrew)
 - MacOS 13 (x86_64), GNU GCC v15.0.7; OpenMPI v; CMake v3.31.6 (Homebrew)
 - MacOS 14 (ARM), GNU GCC v14.2.0_1; OpenMPI v; CMake v3.31.6 (Homebrew)
 - MacOS 14 (ARM), GNU GCC v15.0.7; OpenMPI v; CMake v3.31.6 (Homebrew)

**NOTE:** QUICK GPU builds require CUDA >= v7.x or ROCm <= v5.4.2, >= v6.2.1
for CUDA and HIP versions, respectively. Please consult the Release Notes for
the respective GPU SDKs on supported GPU devices and compatible software
dependencies (compilers, etc.).

|QUICK_VERSION| CUDA version has been tested on the following GPUs: H200, H100,
A100, RTX3080TI, RTX2080TI, RTX8000, RTX6000, RTX2080, T4, V100, Titan V, P100,
M40, GTX1080, K80, and K40.

|QUICK_VERSION| HIP version has been tested on the following GPUs: MI100,
MI210, MI250, and MI300A.

**NOTE:** We recommend that the CUDA/MPI+CUDA and HIP/MPI+HIP versions be
executed only on dedicated GPU cards where no other tasks are being run.
Performance is better on datacenter GPUs than on consumer GPUs.  For the
MPI+CUDA and MPI+HIP versions, we also recommend that only one CPU core (MPI
process) is used per GPU; this can be done by setting the number of processes
(*e.g.*, in the *mpirun* command) equal to the number of GPUs.
    

Installation
------------

Installation of QUICK requires that at least CMake v3.12.0 be installed in the
target machine. To install QUICK using CMake, one must first create a build
directory (separate from the source directory). After installation you can
safely delete this build directory if you want to save disk space. Assuming the
root folder of the repository is ``QUICK_HOME``, you can make the build
directory like this:

.. code-block:: none

	cd ${QUICK_HOME}
	mkdir builddir

check CMake version
^^^^^^^^^^^^^^^^^^^^^^
Ensure CMake (version 3.12.0 or higher) is installed::

	cmake --version


CPU version
^^^^^^^^^^^

Assuming you have created a directory named *builddir* in the ``QUICK_HOME``
directory and you want to install QUICK into directory ``QUICK_INSTALL``, use CPU compiler toolchain in Macbook or Linux. All
QUICK CPU versions can be configured and built as follows:

1. Configure with CMake. For a basic MPI-enabled build using Clang compiler::
	
	cmake .. \
		-DCOMPILER=CLANG \
		-DMPI=TRUE \
		-DCMAKE_INSTALL_PREFIX=$HOME/quick_install

   where ``-DMPI`` flag enables compiling the MPI parallel version. 
   The serial version is compiled by default. 
   Multiple compiler toolchains are supported through the ``-DCOMPILER`` flag:

   * GNU compiler (default): ``-DCOMPILER=GNU``
   * Intel compiler: ``-DCOMPILER=INTEL``
   * Clang compiler: ``-DCOMPILER=CLANG``
   Note that requesting the Clang compiler requires the GNU Fortran compiler (`gfortran`) to be installed. C/C++ code will be compiled by Clang, 
   while Fortran code will be compiled by gfortran.

2. Build and install::

	make -j
	make install

   Note that the flag -j enables parallel builds that use all available CPU cores for compilation.

CUDA version
^^^^^^^^^^^^

Assuming you have created a directory named *builddir* in the ``QUICK_HOME``
directory and you want to install QUICK into directory ``QUICK_INSTALL``, use
GNU compiler tool chain, and want to compile for the NVIDIA Volta
microarchitecture, all QUICK versions can be configured and built as follows:

.. code-block:: none

	cd ${QUICK_HOME}/builddir
	cmake .. -DCOMPILER=GNU -DMPI=TRUE -DCUDA=TRUE -DQUICK_USER_ARCH=volta \
          -DCMAKE_INSTALL_PREFIX=${QUICK_INSTALL}
	make
	make install

where ``-DMPI`` and ``-DCUDA`` flags enable compiling MPI parallel and CUDA
serial versions. Specifying both flags simultaneously will trigger compilation
of the MPI+CUDA multi-GPU version. The serial version is compiled by default.

If you want to compile CUDA code for different microarchitectures, you can
specify these as a string with space separation, e.g.
``-DQUICK_USER_ARCH='volta turing'`` to compile for Volta and Turing
architectures.

If the microarchitecture is not specified, then QUICK will be compiled for
multiple architectures based on your CUDA toolkit version. This will lead to a
very time consuming compilation but provides maximum compatibility of the
``quick.cuda`` and ``quick.cuda.MPI`` executables with different types of GPUs.

HIP version
^^^^^^^^^^^^

Assuming you have created a directory named *builddir* in the ``QUICK_HOME``
directory and you want to install QUICK into directory ``QUICK_INSTALL``, use
GNU compiler tool chain, and want to compile for AMD gfx908 microarchitecture,
all QUICK versions can be configured and built as follows:

.. code-block:: none

        cd ${QUICK_HOME}/builddir
        cmake .. -DCOMPILER=GNU -DMPI=TRUE -DHIP=TRUE -DQUICK_USER_ARCH=gfx90a \
          -DCMAKE_INSTALL_PREFIX=${QUICK_INSTALL} -DHIP_TOOLKIT_ROOT_DIR=${ROCM_PATH} \
          -DMAGMA=TRUE -DMAGMA_ROOT=${MAGMA_PATH} 
        make
        make install

where ``-DMPI`` and ``-DHIP`` flags enable compiling MPI parallel and HIP
serial versions. Specifying both flags simultaneously will trigger compilation
of the MPI+HIP multi-GPU version. The serial version is compiled by default.
Path to ROCm installation can be specified using ``-DHIP_TOOLKIT_ROOT_DIR`` but
this is optional. Flags ``-DMAGMA`` and ``-DMAGMA_ROOT`` are used to enable
MAGMA library support for matrix diagonalization and specify the MAGMA
installation directory, respectively. The use of MAGMA is optional but highly
recommended for older ROCm versions (< v5.3.0) since matrix diagonalization is
performed on host (CPU) in QUICK by default due to poor performance in the ROCm
math libraries (rocSOLVER). 

If the microarchitecture is not specified (i.e. absence of the
``-DQUICK_USER_ARCH`` flag), QUICK will be compiled for gfx908 architecture. As of
QUICK-23.03, specifying multiple microarchitectures for HIP version (e.g.
``-DQUICK_USER_ARCH=gfx908 gfx90a``) is not supported. This means that the
resulting ``quick.hip`` and ``quick.hip.MPI`` executables will only run on a
single AMD GPU architecture.    

Build System: CMake Options
^^^^^^^^^^^^^^^^^^^^^^^^^^^

For additional configurations, a full list of available options and their
definitions for the CMake build system can be found here: `cmake options <cmake-options.html>`_. 


Testing
-------

Once you have installed QUICK, you can add the location of the executables to
your path and set relevant environment variables by sourcing (source executes
the commands in a file) the ``quick.rc`` script. This must be done again every
time a new terminal session is opened:

.. code-block:: none

		source ${QUICK_INSTALL}/quick.rc

The build system makes use of a shell script (``runtest``, located in
``${QUICK_HOME}/tools`` but will be copied to the installation directory
``QUICK_INSTALL``) for testing QUICK. Below we describe the standard procedure
to carry out tests; but if you are interested, see `runtest options <runtest-options.html>`_
for more information on the ``runtest`` script.

Short tests can be run using the ``runtest`` shell script found inside the install directory. 

.. code-block:: none

	source $(QUICK_INSTALL)/quick.rc
	cd ${QUICK_INSTALL}
	./runtest

Similarly, robust testing can be performed as follows. 

.. code-block:: none

	cd ${QUICK_INSTALL}
	./runtest --full

You may now try some hands-on tutorials to learn how to use QUICK
here: `hands-on tutorials <hands-on-tutorials.html>`_.

Uninstallation and Cleaning
---------------------------

Delete the build and install directories and their contents.

*Last updated by Andreas Goetz on 04/25/2024.*
