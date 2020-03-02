Hands-on Tutorials
==================

QUICK home directory contains some test runs and examples. These can be found inside *examples* directory.
For this tutorial we will use water as an example. The structure of a typical QUICK input is as follows. 

::

     HF BASIS=cc-pvDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 ENERGY  <== Job command
                                                             <== Empty line
     O              -0.06756756   -0.31531531    0.00000000 | 
     H               0.89243244   -0.31531531    0.00000000 |<== Coordinates
     H              -0.38802215    0.58962052    0.00000000 |                                                                


Depending on the installation, you may run QUICK as folllows.  

Single CPU version:

::

     ./quick input.in

MPI version:

::

     mpirun -np num_cores ./quick.MPI input.in

where num_cores is the number of cores that you wish to use.

CUDA version:

::

     ./quick.cuda input.in

Now assume that we have successfully installed CUDA version and set the basis set path. 

1. HF/DFT single point energy calculation 

The typical job commands for HF/DFT is as follows. 

::

     HF BASIS=cc-pvDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 ENERGY
     ^     ^              ^             ^              ^
     |     |              |             |              |
     |     |              |             |            Compute energy
     |     |              |             Density matrix cutoff
     |     |              Integral cutoff
     |     Basis set
     Hamiltonian

For DFT, the above job commands change as follows.


::

     DFT B3LYP BASIS=cc-pvDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 ENERGY
     ^     ^      ^       ^             ^              ^
     |     |      |       |             |              |
     |     |      |       |             |            Compute energy
     |     |      |       |             Density matrix cutoff
     |     |      |       Integral cutoff
     |     |      Basis set
     |     Functional
     Hamiltonian

Note that in the above command, we are using NATIVE B3LYP functional. If we want to use
LIBXC B3LYP functional, the command changes as follows. 

::

     DFT LIBXC=HYB_GGA_XC_B3LYP BASIS=cc-pvDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 ENERGY
     ^     ^      ^       ^             ^              ^
     |     |      |       |             |              |
     |     |      |       |             |            Compute energy
     |     |      |       |             Density matrix cutoff
     |     |      |       Integral cutoff
     |     |      Basis set
     |     Functional
     Hamiltonian

It is also possible to ask for exchange and correlation LIBXC functionals seperately. 
For instance, if we use BLYP the above command would change as follows.  

::

     DFT LIBXC=GGA_X_B88,GGA_C_LYP BASIS=cc-pvDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 ENERGY
     ^        ^                          ^           ^             ^              ^
     |        |                          |           |             |              |
     |        |                          |           |             |            Compute energy
     |        |                          |           |             Density matrix cutoff
     |        |                          |       Integral cutoff
     |        |                       Basis set
     |        Functionals (Functional_1, Functional_2 seperated by a comma)
     Hamiltonian

Note that currently, QUICK cannot handle more than two functions at a time. 

::

     HF BASIS=cc-pvDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 ENERGY

     O                 -0.06756756   -0.31531531    0.00000000
     H                  0.89243244   -0.31531531    0.00000000
     H                 -0.38802215    0.58962052    0.00000000

2. HF/DFT gradient calculation

::

     HF BASIS=cc-pvDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 GRADIENT

     O                 -0.06756756   -0.31531531    0.00000000
     H                  0.89243244   -0.31531531    0.00000000
     H                 -0.38802215    0.58962052    0.00000000    

3. HF/DFT geometry optimization calculation

::

     HF BASIS=cc-pvDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 OPTIMIZE

     O                 -0.06756756   -0.31531531    0.00000000
     H                  0.89243244   -0.31531531    0.00000000
     H                 -0.38802215    0.58962052    0.00000000


