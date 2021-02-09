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

QUICK-21.03 CUDA version has been tested on the following GPU cards: A100, V100, TitanV, P100, RTX2080Ti, GTX1080, K80 and K40.

**Note:** we recommend that the CUDA and CUDA-MPI versions be executed only on dedicated GPU cards where no other tasks are being run.
For the CUDA-MPI version we also recommend that only one CPU per GPU is used; this can be done by setting the number of processes (*e.g.*,
in the *mpirun* command) equal to the number of CPUs.

2. Installation
---------------

The initial step is to setup the installation for the desired QUICK version. For this, go to QUICK home folder and run configure script
as follows.

For serial version installation:

::

	./configure --serial --prefix <installdir> compiler

possible options for compiler are *gnu* or *intel*.

For MPI parallel version installation:

::

        ./configure --mpi --prefix <installdir> compiler

For CUDA version installation:

::

        ./configure --cuda --arch <micro-arch> --prefix <installdir> compiler

For CUDA-MPI version installation:

::

        ./configure --cudampi --arch <micro-arch> --prefix <installdir> compiler

possible options for <micro-arch> are *kepler*, *pascal*, *volta*, *turing*, *ampere*.

You can configure the installtion for multiple CUDA architectures as follows.

::

	./configure --cuda --arch <micro-arch-1> --arch <micro-arch-2> --prefix <installdir> compiler

Note that if you dont set the *--arch* option, QUICK will be compiled for multiple architectures based on your CUDA toolkit version.
This will lead to a very time consuming compilation.

More information on configure script options can be obtained by running:

::

	./configure --help

Once the configuration script has been successfully executed, you will have a make.in file in QUICK home directory.
At this point simply run:

::

	make

This will build the QUICK version you requested and place an executable inside *QUICK_HOME/bin*. All object files
and libraries will be located inside *QUICK_HOME/build*.

Next, install QUICK using:

::

	make install

This will copy executables, libraries and .mod files into *installdir*. In case the *--prefix* variable is not specified,
*installdir* will be set to the QUICK_HOME folder.

3. Environment Variables and Testing
------------------------------------

Once you have installed any version of QUICK following above instructions, it is necessary to set environment variables.
This can be done by sourcing quick.rc in the installation directory.

::

 source $(installdir)/quick.rc

The QUICK tests can be executable as follows, from the QUICK home directory.

::

 make test

This will run a series of test cases and inform you which tests passed or failed.

4. Uninstallation and Cleaning
------------------------------

To uninstall QUICK, from the QUICK home directory execute:

::

 make uninstall

In order to clean a QUICK build, the following must be run from the QUICK home directory:

::

make clean

This will remove all the object files located inside *QUICK_HOME/build*.

For a complete removal of object files, executables and .mod files, including  *QUICK_HOME/bin*
and *QUICK_HOME/build* directories:

::

make distclean

*Last updated by Madu Manathunga on 02/05/2021.*
