Installation Guide
========================

QUICK has been compiled and tested on x86 CPU and Intel Nvidia GPU hardware.
The compilation of the GPU enabled ERI code can take a significant amount of time (several minutes) - be patient, the compiler is working hard to generate lightning fast code for you.

Compatible Compilers and Hardware
---------------------------------

In general QUICK works well with a range of GNU or Intel compilers, Intel MKL, Open MPI, Intel MPI, and different CUDA version. 
We have specifically tested QUICK-21.03 with following compilers under the Linux operating system.

• Serial version

 1. GNU/4.8.5, 7.2.0, 8.3.1, 9.3.0  : No issue detected so far.
 2. Intel/2018.1.163, 2019.1.127    : No issue detected so far.

• MPI version

 1. GNU/8.3.1; Open MPI/4.0.4              : No issue detected so far.
 2. GNU/9.3.0; Open MPI/4.0.3              : No issue detected so far.
 3. Intel/2018.1.163; Intel MPI/2018.1.163 : No issue detected so far.
 4. Intel/2019.1.127; Intel MPI/2019.8.254 : No issue detected so far.

• CUDA version

 1. GNU/4.8.5, 7.2.0, 8.3.1, 9.3.0; CUDA/10.1, 10.2 : No issue detected so far.
 2. Intel/2018.1.163, 2019.1.127; CUDA/10.2         : No issue detected so far.

• CUDA-MPI version

 1. GNU/8.3.1; CUDA/10.2; OPENMPI/4.0.4              : No issue detected so far.
 2. GNU/9.3.0; CUDA/11.0.207; OPENMPI/4.0.3          : No issue detected so far.
 3. Intel/2018.1.163; Intelmpi/2018.1.163; CUDA/10.2 : No issue detected so far.

QUICK-21.03 CUDA version has been tested on the following GPU cards: A100, RTX2080Ti, RTX8000, RTX6000, RTX2080, T4, V100, Titan V, P100, M40, GTX1080, K80 and K40.

**Note:** we recommend that the CUDA and CUDA-MPI versions be executed only on dedicated GPU cards where no other tasks are being run.
For the CUDA-MPI version we also recommend that only one CPU per GPU is used; this can be done by setting the number of processes (*e.g.*,
in the *mpirun* command) equal to the number of CPUs.

Installation
------------

Using legacy build system
^^^^^^^^^^^^^^^^^^^^^^^^^

The initial step is to configure the installation for the desired QUICK version. For this, go to the QUICK home folder and run the ``configure`` script
as explained below. Running the configure script without options or with the ``--help`` flag will print available options.

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

Note that if you do not set the ``--arch`` option, QUICK will be compiled for multiple architectures based on your CUDA toolkit version.
This will lead to a very time consuming compilation but to maximum compatibility of the ``quick.cuda`` and ``quick.cuda.MPI`` executables with different types of GPUs.

If you specify multiple build type flags together (e.g. ``--serial`` and ``--cuda``) then all different versions will be compiled and installed.

More information on configure script options can be found `here <configure-options.html>`_.

Once the configuration script has been successfully executed, you will have a file ``make.in`` in the QUICK home directory.
At this point simply run:

.. code-block:: none

	make

This will build the QUICK version you requested and place an executable inside ``QUICK_HOME/bin``. All object files
and libraries will be located inside ``QUICK_HOME/build``. 

Next, install QUICK using:

.. code-block:: none

	make install

This will copy executables, libraries and .mod files into *installdir*. In case the ``--prefix`` variable is not specified,
*installdir* will be set to the ``QUICK_HOME`` folder.

Using the CMake build system
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

CMake installation requires at least CMake/3.9.0 installed in the target machine. To install QUICK using CMake, one must first create a build  directory. Assuming you have created a directory named *builddir* in the ``QUICK_HOME`` directory and you want to install QUICK into directory ``QUICK_INSTALL``, use GNU compiler tool chain, and want to compile for Nvidia Volta microarchitecture, all QUICK versions can be configured and built as follows.

.. code-block:: none

	cd ${QUICK_HOME}/builddir
	cmake .. -DCOMPILER=GNU -DMPI=TRUE -DCUDA=TRUE -DQUICK_USER_ARCH=volta \
	-DCMAKE_INSTALL_PREFIX=${QUICK_INSTALL}
	make
	make install

Where ``-DMPI`` and ``-DCUDA`` flags enable compiling MPI parallel and CUDA serial versions. Specifying both flags simultaneously will trigger compilation of the MPI-CUDA multi-GPU version. The serial version is compiled by default.

If you want to compile CUDA code for different microarchitectures, you can specify these as a string with space separation, e.g. ``-DQUICK_USER_ARCH='volta turing'`` to compile for Volta and Turing architectures.

If the microarchitecture is not specified, then QUICK will be compiled for multiple architectures based on your CUDA toolkit version. This will lead to a very time consuming compilation but to maximum compatibility of the ``quick.cuda`` and ``quick.cuda.MPI`` executables with different types of GPUs.

A full list of available flags and their defintions written by Jamie Smith can be found `here <cmake-options.html>`_. 


Environment Variables and Testing
---------------------------------

Once you have installed QUICK, you can add the location of the executables to your path and set relevant environment variables by sourcing the ``quick.rc`` script:

.. code-block:: none

		source ${QUICK_INSTALL}/quick.rc

Both build systems make use of a shell script (``runtest``, located in ``${QUICK_HOME}/tools`` but will be copied to the installation directory ``QUICK_INSTALL``) for testing QUICK. Below we describe the standard procedure to carry out tests; but if you are interested, see `here <runtest-options.html>`_ for more information on the ``runtest`` script.


Legacy build system
^^^^^^^^^^^^^^^^^^^

Once you have installed any version of QUICK, it is necessary to set environment variables.
This can be done by sourcing ``quick.rc`` in the installation directory.

.. code-block:: none

	source $(installdir)/quick.rc

If QUICK is built using the legacy build system, tests can be executed as follows from the ``$QUICK_HOME`` directory.

.. code-block:: none

	make test

This will run a series of short test cases and inform you which tests passed or failed. It is also possible to run a robust
test as follows. 

.. code-block:: none

	make fulltest

CMake build system
^^^^^^^^^^^^^^^^^^

If QUICK is built using the CMake build system, short tests can be run using the ``runtest`` shell script that you would find
inside install directory. 

.. code-block:: none

	source $(QUICK_INSTALL)/quick.rc
	cd ${QUICK_INSTALL}
	./runtest

Similarly, robust testing can be performed as follows. 

.. code-block:: none

	cd ${QUICK_INSTALL}
	./runtest --full

Uninstallation and Cleaning
---------------------------

Legacy build system
^^^^^^^^^^^^^^^^^^^

If QUICK was built using the legacy build system, uninstallation can be performed by executing the following from the QUICK home directory:

.. code-block:: none

	make uninstall

In order to clean a QUICK build, the following must be run from the QUICK home directory:

.. code-block:: none

	make clean

This will remove all the object files located inside ``QUICK_HOME/build``.

For a complete removal of object files, executables and .mod files, including  ``QUICK_HOME/bin``
and ``QUICK_HOME/build`` directories:

.. code-block:: none

	make distclean

CMake build system
^^^^^^^^^^^^^^^^^^

Simply delete contents inside build and install directories and / or delete the build and install directories.

*Last updated by Madu Manathunga on 03/20/2021.*
