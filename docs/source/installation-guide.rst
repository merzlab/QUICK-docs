.. include:: quick_docs_common.rst

Installation Guide
==================

QUICK has been compiled and tested on x86 and ARM CPU architectures, and on
Nvidia and AMD GPU architectures.

**NOTE:** For GPU builds, the compilation of the GPU enabled ERI code can take
a significant amount of time (several minutes for default builds and several
hours for f-function basis set support) - be patient, the compiler is working
hard to generate lightning fast code for you.

**NOTE:** HIP/MPI+HIP support is disabled for this release.  Please use QUICK
version 23.08b for HIP support

Compatible Compilers and Hardware
---------------------------------

In general QUICK works well with a range of compilers (GNU, Intel), math
libraries (Intel MKL, reference BLAS/LAPACK, MAGMA), MPI implementations
(OpenMPI, Intel MPI), and GPU SDK versions (CUDA and ROCm/HIP).  We have
specifically tested |QUICK_VERSION| with following compilers, libraries, and
tools.

**Linux:**

 1. GNU GCC v7.3.0; OpenMPI v3.1.1; CUDA v9.2.88; CMake v3.11.4
 2. GNU GCC v8.3.0; OpenMPI v3.1.4; CUDA v10.2.89; CMake v3.15.1
 3. GNU GCC v11.3.0; OpenMPI v4.1.4; CUDA v11.8; CMake v3.23.1
 4. GNU GCC v12.3.0; OpenMPI v5.0.0; CUDA v12.3; CMake v3.26.3
 5. Clang v14.0 / GNU GCC v11.3.0 (Fortran); OpenMPI v3.1.4; CUDA v10.2.89; CMake v3.15.3
 6. Intel v2021b; Intel MPI v2021b; CUDA v11.8; CMake v3.23.1
 7. Intel OneAPI/LLVM v2022.2.1; Intel MPI v2022.2.1; CMake v3.18.4

**NOTE:** QUICK GPU builds require at least CUDA v7.x and ROCm v5.1.x for CUDA
and HIP versions, respectively. Please consult the Release Notes for the
respective GPU SDKs on supported GPU devices.

|QUICK_VERSION| CUDA version has been tested on the following GPU cards: A100,
RTX3080Ti, RTX2080Ti, RTX8000, RTX6000, RTX2080, T4, V100, Titan V, P100, M40,
GTX1080, K80, and K40.

|QUICK_VERSION| HIP version is currently disabled. Please use QUICK-23.08b if you want to use AMD GPUs.

.. |QUICK_VERSION| HIP version has been tested on the following GPU cards: MI100,
   MI210, and MI250. As of QUICK-23.03, the performance on MI210 and MI250 cards is
   not optimized but the code runs properly. 

**NOTE:** We recommend that the CUDA/MPI+CUDA and HIP/MPI+HIP versions be
executed only on dedicated GPU cards where no other tasks are being run.
Performance is better on datacenter GPUs than on consumer GPUs.  For MPI+CUDA
and MPI+HIP versions, we also recommend that only one CPU core (MPI task) is
used per GPU; this can be done by setting the number of processes (*e.g.*, in
the *mpirun* command) equal to the number of GPUs.

**Intel-based Macbooks:**

Software stack (compiler installed via Macports):

 1. macOS 11.7.3; GNU/10.4.0, 11.3.0, 12.2.0; OpenMPI 4.1.4
 2. macOS 13.2; GNU 12.2.0, OpenMPI 4.1.4

**ARM-based Macbooks (M3 Pro CPU):**

Software stack (compiler installed via Macports):

 1. macOS Sonoma 14.4.1; GNU GCC 12.3.0; OpenMPI 4.1.6
 2. macOS Sonoma 14.4.1; GNU GCC (Fortran); Clang 17.0.6; OpenMPI 4.1.6
    

Installation
------------

Installation of QUICK requires that at least CMake/3.9.0 be installed in the
target machine. To install QUICK using CMake, one must first create a build
directory (separate from the source directory). After installation you can
safely delete this build directory if you want to save disk space. Assuming the
root folder of the repository is ``QUICK_HOME``, you can make the build
directory like this:

.. code-block:: none

	cd ${QUICK_HOME}
	mkdir builddir

CUDA version
^^^^^^^^^^^^

Assuming you have created a directory named *builddir* in the ``QUICK_HOME``
directory and you want to install QUICK into directory ``QUICK_INSTALL``, use
GNU compiler tool chain, and want to compile for the Nvidia Volta
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
recommended since the diagonalization is performed on host (CPU) by default
(which can be very slow). 

If the microarchitecture is not specified (i.e. absence of the
``-DQUICK_USER_ARCH`` flag), QUICK will be compiled for gfx908 architecture. As of
QUICK-23.03, specifying multiple microarchitectures for HIP version (e.g.
``-DQUICK_USER_ARCH=gfx908 gfx90a``) is not supported. This means that the
resulting ``quick.hip`` and ``quick.hip.MPI`` executables will only run on a
single AMD GPU architecture.    


A full list of available flags and their definitions can be found `here <cmake-options.html>`_. 

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
to carry out tests; but if you are interested, see `here <runtest-options.html>`_
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

You may now try some hands-on tutorials to learn how to use QUICK `here <hands-on-tutorials.html>`_.

Uninstallation and Cleaning
---------------------------

Simply delete contents inside build and install directories and/or delete the
build and install directories.

*Last updated by Andreas Goetz on 04/25/2024.*
