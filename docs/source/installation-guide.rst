Installation Guide
========================

1. Compatible Compilers and Hardware
------------------------------------

We have tested QUICK-20.06 with following Compilers.

• Serial version

 1. GNU/4.8.5, 7.2.0 : No issue detected so far. 
 2. Intel/2018.1.163 : No issue detected so far. 

• MPI version 

 1. GNU/7.2.0; MVAPICH2/2.3.2             : No issue detected so far.                 
 2. Intel/2018.1.163; Intelmpi/2018.1.163 : No issue detected so far.

• CUDA version

 1. GNU/4.8.5, GNU/7.2.0; CUDA/10.1, 10.2 : No issue detected so far.                 
 2. Intel/2018.1.163; CUDA/10.1           : No issue detected so far.   

We have tested QUICK-20.06 CUDA version on V100, TitanV and RTX2080Ti. Note that this 
version is currently not capable of running on any Kepler type GPU or older since we are using 
double precision CUDA atomicAdd operators in our device code. 

2. Installation 
--------------------------

Go to QUICK home folder and run configure script to generate the corresponding input file for Make. 

For serial version installation:

::

	./configure -serial --prefix <installdir> compiler

possible options for compiler are *gnu* or *intel*. 

For MPI parallel version installation:

::

        ./configure -mpi --prefix <installdir> compiler

For CUDA version installation:

::

        ./configure -cuda -arch <micro-arch> --prefix <installdir> compiler

possible options for <micro-arch> are *pascal*, *volta* and *turing*. 
Note that if you dont set the *-arch* option, QUICK will be compiled for multiple architectures based on your CUDA toolkit version.
This will lead to a very time consuming compilation.   

Once the configuration script has been successfully executed, you will have a make.in file in QUICK home directory. 
At this point simply run:

::	

	make install
 	
This will compile the QUICK version you requested and place an executable inside *installdir/bin*. 


3. Environment Variables and Testing
------------------------------------

Once you have installed any version of QUICK following about instructions, it is necessary to set basis set path. 
In order to do so, add the following command into your .bash_profile or .bashrc. 

::

 export QUICK_BASIS=$(installdir)/basis

You can test QUICK by simply running the following command from QUICK home directory. 

::

 make test 

This will run a series of test cases and inform you which tests passed or failed. 

4. Uninstallation
-----------------

To uninstall QUICK, run *make uninstall* from QUICK home directory. Alternatively, you can delete all content in your
*installdir*. 


*Last updated by Madu Manathunga on 07/13/2020.*
