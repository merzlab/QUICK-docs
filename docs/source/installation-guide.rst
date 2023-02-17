.. include:: quick_docs_common.rst

Installation Guide
==================

QUICK has been compiled and tested on x86, ARM CPU and Nvidia, AMD GPU hardware.
The compilation of the GPU enabled ERI code can take a significant amount of time (several minutes) - be patient, the compiler is working hard to generate lightning fast code for you.

Compatible Compilers and Hardware
---------------------------------

In general QUICK works well with a range of GNU or Intel compilers, Intel MKL, Open MPI, Intel MPI, different CUDA and ROCM versions. 
We have specifically tested |QUICK_VERSION| with following compilers under the Linux operating system.

• Serial version

 1. GNU/4.8.5, 7.2.0, 8.3.1, 9.3.0  : No issue detected so far.
 2. Intel/2018.1.163, 2019.1.127    : No issue detected so far.

• MPI version

 1. GNU/8.3.1; OpenMPI/4.0.4              : No issue detected so far.
 2. GNU/9.3.0; OpenMPI/4.0.3              : No issue detected so far.
 3. Intel/2018.1.163; IntelMPI/2018.1.163 : No issue detected so far.
 4. Intel/2019.1.127; IntelMPI/2019.8.254 : No issue detected so far.

• CUDA version

 1. GNU/4.8.5, 7.2.0, 8.3.1, 9.3.0; CUDA/10.1, 10.2, 11.0, 11.2, 11.4 : No issue detected so far.
 2. Intel/2018.1.163, 2019.1.127; CUDA/10.2         : No issue detected so far.

• CUDA-MPI version

 1. GNU/8.3.1; CUDA/10.2; OpenMPI/4.0.4              : No issue detected so far.
 2. GNU/9.3.0; CUDA/11.0.207, 11.2, 11.4; OpenMPI/4.0.3          : No issue detected so far.
 3. Intel/2018.1.163; IntelMPI/2018.1.163; CUDA/10.2 : No issue detected so far.

• HIP version

 1. GNU/9.3.0; ROCM/5.1, 5.2, 5.3 : No issue detected so far.

• HIP-MPI version

 1. GNU/9.3.0; ROCM/5.1, 5.2, 5.3; OpenMPI/4.0.3, 4.1.4 : No issue detected so far.

|QUICK_VERSION| CUDA version has been tested on the following GPU cards: A100, RTX3080Ti, RTX2080Ti, RTX8000, RTX6000, RTX2080, T4, V100, Titan V, P100, M40, GTX1080, K80 and K40.

|QUICK_VERSION| HIP version has been tested on the following GPU cards: MI100, MI210, MI250. As of QUICK-23.03, the performance on MI210 and MI250 cards is not optimized but the code runs properly. 

**Note:** We recommend that the CUDA, CUDA-MPI, HIP, HIP-MPI versions be executed only on dedicated GPU cards where no other tasks are being run. Performance is better on datacenter GPUs than on consumer GPUs. 
For CUDA-MPI and HIP-MPI versions, we also recommend that only one CPU core (MPI task) is used per GPU; this can be done by setting the number of processes (*e.g.*, in the *mpirun* command) equal to the number of GPUs.

We have also tested |QUICK_VERSION| on Intel based Macbooks with following software stack (compiler installed via Macports):

 1. macOS 11.7.3; GNU/10.4.0, 11.3.0, 12.2.0; OpenMPI 4.1.4 : No issue detected so far.
 2. macOS 13.2; GNU 12.2.0, OpenMPI 4.1.4 : No issue detected so far.


Installation
------------

QUICK installation requires at least CMake/3.9.0 installed in the target machine. To install QUICK using CMake, one must first create a build  directory. After installation you can safely delete this build directory if you want to save disk space. Assuming the root folder of the repository is ``QUICK_HOME``, you can make the build directory like this:

.. code-block:: none

	cd ${QUICK_HOME}
	mkdir builddir

CUDA version
^^^^^^^^^^^^

Assuming you have created a directory named *builddir* in the ``QUICK_HOME`` directory and you want to install QUICK into directory ``QUICK_INSTALL``, use GNU compiler tool chain, and want to compile for Nvidia Volta microarchitecture, all QUICK versions can be configured and built as follows.

.. code-block:: none

	cd ${QUICK_HOME}/builddir
	cmake .. -DCOMPILER=GNU -DMPI=TRUE -DCUDA=TRUE -DQUICK_USER_ARCH=volta \
          -DCMAKE_INSTALL_PREFIX=${QUICK_INSTALL}
	make
	make install

Where ``-DMPI`` and ``-DCUDA`` flags enable compiling MPI parallel and CUDA serial versions. Specifying both flags simultaneously will trigger compilation of the MPI-CUDA multi-GPU version. The serial version is compiled by default.

If you want to compile CUDA code for different microarchitectures, you can specify these as a string with space separation, e.g. ``-DQUICK_USER_ARCH='volta turing'`` to compile for Volta and Turing architectures.

If the microarchitecture is not specified, then QUICK will be compiled for multiple architectures based on your CUDA toolkit version. This will lead to a very time consuming compilation but to maximum compatibility of the ``quick.cuda`` and ``quick.cuda.MPI`` executables with different types of GPUs.

HIP version
^^^^^^^^^^^^

Assuming you have created a directory named *builddir* in the ``QUICK_HOME`` directory and you want to install QUICK into directory ``QUICK_INSTALL``, use GNU compiler tool chain, and want to compile for AMD gfx908 microarchitecture, all QUICK versions can be configured and built as follows.

.. code-block:: none

        cd ${QUICK_HOME}/builddir
        cmake .. -DCOMPILER=GNU -DMPI=TRUE -DHIP=TRUE -DQUICK_USER_ARCH=gfx90a \
          -DCMAKE_INSTALL_PREFIX=${QUICK_INSTALL} -DHIP_TOOLKIT_ROOT_DIR=${ROCM_PATH} \
          -DMAGMA=TRUE -DMAGMA_ROOT=${MAGMA_PATH} 
        make
        make install

Where ``-DMPI`` and ``-DHIP`` flags enable compiling MPI parallel and HIP serial versions. Specifying both flags simultaneously will trigger compilation of the MPI-HIP multi-GPU version. The serial version is compiled by default. Path to ROCM installation can be specified using ``-DHIP_TOOLKIT_ROOT_DIR`` but this is optional. Flags ``-DMAGMA`` and ``-DMAGMA_ROOT`` are used to enable Magma library support for matrix diagonalization and specify Magma installation directory. The use of Magma is optional but highly recommended since the diagonalization is performed on host by default. 

If the microarchitecture is not specified (i.e. in the absence of ``-DQUICK_USER_ARCH``), QUICK will be compiled for gfx908 architecture. As of QUICK-23.03, specifying multiple microarchitectures for HIP version (e.g. ``-DQUICK_USER_ARCH=gfx908 gfx90a``) is not supported. This means that the resulting ``quick.hip`` and ``quick.hip.MPI`` executables will only run on a single AMD GPU architecture.    


A full list of available flags and their definitions can be found `here <cmake-options.html>`_. 

Testing
-------

Once you have installed QUICK, you can add the location of the executables to your path and set relevant environment variables by sourcing (source executes the commands in a file) the ``quick.rc`` script. This must be done again every time a new terminal session is opened:

.. code-block:: none

		source ${QUICK_INSTALL}/quick.rc

The build system makes use of a shell script (``runtest``, located in ``${QUICK_HOME}/tools`` but will be copied to the installation directory ``QUICK_INSTALL``) for testing QUICK. Below we describe the standard procedure to carry out tests; but if you are interested, see `here <runtest-options.html>`_ for more information on the ``runtest`` script.

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

Simply delete contents inside build and install directories and / or delete the build and install directories.

*Last updated by Madu Manathunga on 11/21/2022.*
