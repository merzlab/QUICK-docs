Installation Guide
========================

1. Compatible Compilers and Hardware
------------------------------------

We have tested QUICK-20.03 with following Compilers.

• Serial version

 1. GNU/4.8.3: No issue detected so far.
 2. GNU/4.8.5: No issue detected so far. 

• MPI version 

 1. GNU/7.2.0, MPICH/3.2.1: No issue detected so far.                 

• CUDA version

 1. GNU/4.8.5, CUDA/10.2  : No issue detected so far.                 

We have tested QUICK-20.03 CUDA version on GTX1080, P100, V100, TitanV and RTX2080Ti. Note that this 
version is currently not capable of running on any Kepler type GPU or older since we are using 
double precision CUDA atomicAdd operators in our device code. 

2. Serial Installation 
--------------------------

You can use gnu or intel compilers for this purpose. Go to QUICK main folder and run the following
commands.  For GNU compiler (version 4.6 or later):

::

	cp ./makein/make.in.gnu ./make.in
	
For intel compiler (version 2011 or later):

::

	cp ./makein/make.in.intel ./make.in

Then, run the following command. 

::

        make quick
     
This will compile a serial version of QUICK. 

3. MPI Installation
-------------------

If you have  openmpi or MPICH installed, you can compile the MPI version by running 
following commands from quick main folder. 

::

	cp ./makein/make.in.MPI ./make.in
	
	make quick.MPI

Note: Intel mpi (2011 or later) is also supported, however, we havent extensively tested yet. 

4. CUDA Version Installation
----------------------------

If you want to install the GPU version, NVIDIA CUDA COMPILER is required. You may check your CUDA 
compiler version using 'nvcc --version'. 

a) Run the following command.

::

	cp ./makein/make.in.gnu.cuda ./make.in

b) Open up the make.in file and set CUDA_HOME. This is essential to link or compile CUBLAS and other libraries.

::

	CUDA_HOME=(your cuda home) 

c) You may have to change the "-gencode arch=compute_70,code=sm_70" options in CUDA_FLAGS 
variable depending on the type of your GPU. The default value (70) is for a Volta gpu. Use 60 
and 75 for Pascal and Turing GPUs respectively. 

::

	-gencode arch=compute_(your gpu),code=sm_(your gpu)

d) Then run

::
     
	make quick.cuda

in ./bin directory, you can find executable files. 

5. Environment Variables and Testing
------------------------------------

Once you have installed any version of QUICK following about instructions, it is necessary to set basis set path. 
In order to do so, add the following command into your .bash_profile or .bashrc. 

::

 export QUICK_BASIS=$(CUDA_HOME)/basis

You can test QUICK by simply running *testqk.sh* from QUICK home directory. 

::

 ./testqk.sh 

The script will ask you to select an executable. 

::

  Please select the QUICK executable you want to test (type the corresponding number and hit enter):
  1 --> quick
  2 --> quick.MPI
  3 --> quick.cuda

Once you enter the correct number and hit return, the script will automatically run several test cases and inform
you which tests passed or failed. 

6. Uninstallation
-----------------

To uninstall QUICK, simply run *make clean* from QUICK home directory. This will remove all the object files except the executables
inside *bin* folder. You should delete the executables manually. 


*Last updated by Madu Manathunga on 03/25/2020.*
