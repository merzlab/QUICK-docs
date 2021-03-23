Installation Guide
========================

1. Compatible Compilers and Hardware
------------------------------------

We have tested QUICK-21.03 with following compilers on Linux operating system.

• Serial version

 1. GNU/4.8.5, 7.2.0, 8.3.1, 9.3.0  : No issue detected so far.
 2. Intel/2018.1.163, 2019.1.127    : No issue detected so far.

• MPI version

 1. GNU/8.3.1; OPENMPI/4.0.4              : No issue detected so far.
 2. GNU/9.3.0; OPENMPI/4.0.3              : No issue detected so far.
 3. Intel/2018.1.163; Intelmpi/2018.1.163 : No issue detected so far.

• CUDA version

 1. GNU/4.8.5, 7.2.0, 8.3.1, 9.3.0; CUDA/10.1, 10.2 : No issue detected so far.
 2. Intel/2018.1.163, 2019.1.127; CUDA/10.2         : No issue detected so far.

• CUDA-MPI version

 1. GNU/8.3.1; CUDA/10.2; OPENMPI/4.0.4              : No issue detected so far.
 2. GNU/9.3.0; CUDA/11.0.207; OPENMPI/4.0.3          : No issue detected so far.
 3. Intel/2018.1.163; Intelmpi/2018.1.163; CUDA/10.2 : No issue detected so far.

QUICK-21.03 CUDA version has been tested on the following GPU cards: A100, RTX2080Ti, RTX8000, RTX6000, RTX2080, T4, V100, TitanV, P100, M40, GTX1080, K80 and K40.

**Note:** we recommend that the CUDA and CUDA-MPI versions be executed only on dedicated GPU cards where no other tasks are being run.
For the CUDA-MPI version we also recommend that only one CPU per GPU is used; this can be done by setting the number of processes (*e.g.*,
in the *mpirun* command) equal to the number of CPUs.

2. Installation
---------------

2.1 Using legacy build system
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The initial step is to setup the installation for the desired QUICK version. For this, go to QUICK home folder and run configure script
as follows.

For serial version installation:

.. code-block:: none

	./configure --serial --prefix <installdir> compiler

possible options for compiler are *gnu* or *intel*.

For MPI parallel version installation:

.. code-block:: none

        ./configure --mpi --prefix <installdir> compiler

For CUDA version installation:

.. code-block:: none

        ./configure --cuda --arch <micro-arch> --prefix <installdir> compiler

For CUDA-MPI version installation:

.. code-block:: none

        ./configure --cudampi --arch <micro-arch> --prefix <installdir> compiler

possible options for <micro-arch> are *kepler*, *maxwell*, *pascal*, *volta*, *turing*, *ampere*.

You can configure the installtion for multiple CUDA architectures as follows.

.. code-block:: none

	./configure --cuda --arch <micro-arch-1> --arch <micro-arch-2> --prefix <installdir> compiler

Note that if you dont set the *--arch* option, QUICK will be compiled for multiple architectures based on your CUDA toolkit version.
This will lead to a very time consuming compilation.

More information on configure script options can be found `here <configure-options.html>`_.

Once the configuration script has been successfully executed, you will have a make.in file in QUICK home directory.
At this point simply run:

.. code-block:: none

	make

This will build the QUICK version you requested and place an executable inside *QUICK_HOME/bin*. All object files
and libraries will be located inside *QUICK_HOME/build*. 

Next, install QUICK using:

.. code-block:: none

	make install

This will copy executables, libraries and .mod files into *installdir*. In case the *--prefix* variable is not specified,
*installdir* will be set to the QUICK_HOME folder.

2.2 Using CMake build system
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

CMake installation requires you to have at least CMake/3.9.0 installed in the target machine. To install QUICK using CMake, one must first create build and install directories. Assuming you have created directories named *builddir* and *installdir* in *QUICK_HOME* directory, GNU compiler tool chain, and volta microarchitecture, all QUICK versions can be configured and build as follows.

.. code-block:: none

	cd ${QUICK_HOME}/builddir
	cmake .. -DMPI=TRUE -DCUDA=TRUE -DCMAKE_INSTALL_PREFIX=${QUICK_HOME}/installdir \
	-DCOMPILER=GNU -DQUICK_USER_ARCH=volta  
	make
	make install

Where *-DMPI* and *-DCUDA* flags enable compiling MPI parallel and CUDA serial versions. Specifying both of them will compile CUDA parallel version. Serial version is compiled by default. A full list of available flags and their defintions written by Jamie Smith can be found `here <cmake-options.html>`_. 


3. Environment Variables and Testing
------------------------------------

Both build systems make use of a shell script (*runtest*, located in $QUICK_HOME/tools) for testing QUICK. Below we describe the standard procedure to carry out tests; but if you are interested, see `here <runtest-options.html>`_ for more information on *runtest* script.
 
3.1 Legacy build system
^^^^^^^^^^^^^^^^^^^^^^^

Once you have installed any version of QUICK following section 2.1, it is necessary to set environment variables.
This can be done by sourcing quick.rc in the installation directory.

.. code-block:: none

	source $(installdir)/quick.rc

If QUICK is built using legacy build system, tests can be executed as follows from the QUICK home directory.

.. code-block:: none

	make test

This will run a series of short test cases and inform you which tests passed or failed. It is also possible to run a robust
test as follows. 

.. code-block:: none

	make fulltest

3.2 CMake build system
^^^^^^^^^^^^^^^^^^^^^^

If QUICK is built using CMake build system, short tests can be run using the *runtest* shell script that you would find
inside install directory. 

.. code-block:: none

	source $(installdir)/quick.rc
	cd $(installdir)
	./runtest

Similarly, robust testing can be performed as follows. 

.. code-block:: none

	cd $(installdir)
	./runtest --full

4. Uninstallation and Cleaning
------------------------------

4.1 Legacy build system
^^^^^^^^^^^^^^^^^^^^^^^

If QUICK was built using legacy build system, uninstallation can be performed by executing the following from the QUICK home directory:

.. code-block:: none

	make uninstall

In order to clean a QUICK build, the following must be run from the QUICK home directory:

.. code-block:: none

	make clean

This will remove all the object files located inside *QUICK_HOME/build*.

For a complete removal of object files, executables and .mod files, including  *QUICK_HOME/bin*
and *QUICK_HOME/build* directories:

.. code-block:: none

	make distclean

4.2 CMake build system
^^^^^^^^^^^^^^^^^^^^^^

Simply delete contents inside build and install directories.

*Last updated by Madu Manathunga on 03/20/2021.*
