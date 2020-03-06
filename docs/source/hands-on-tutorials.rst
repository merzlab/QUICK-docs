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
*****************************************

The typical job command line for HF/DFT is as follows. Note that all keywords are seperated by
a single space.   

::

     HF BASIS=cc-pvDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 ENERGY
     ^     ^              ^             ^              ^
     |     |              |             |              |
     |     |              |             |            Compute energy
     |     |              |             Density matrix cutoff
     |     |              Integral cutoff
     |     Basis set
     Hamiltonian

For DFT, we replace *HF* hamiltonian with *DFT* and specify the functional name immediately 
after that. For example, we may ask for a B3LYP energy calculation with the following command line.

::

     DFT B3LYP BASIS=cc-pvDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 ENERGY
     ^     ^      ^              ^             ^              ^
     |     |      |              |             |              |
     |     |      |              |             |            Compute energy
     |     |      |              |             Density matrix cutoff
     |     |      |              Integral cutoff
     |     |      Basis set
     |     Functional
     Hamiltonian

Note that in the above command, we are using NATIVE B3LYP functional. If we want to use
LIBXC B3LYP functional, the functional command should change as follows. 

::

     DFT LIBXC=HYB_GGA_XC_B3LYP BASIS=cc-pvDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 ENERGY
     ^     ^                          ^           ^             ^              ^
     |     |                          |           |             |              |
     |     |                          |           |             |            Compute energy
     |     |                          |           |           Density matrix cutoff
     |     |                          |         Integral cutoff
     |     |                         Basis set
     |     Functional
     Hamiltonian

It is also possible to ask for exchange and correlation LIBXC functionals seperately. 
For instance, if we use BLYP, the functional command should change as follows.  

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

We now proceed with HF single point energy calculation for water molecule. Our input file, 
*water.in* is as follows. 

::

     HF BASIS=cc-pvDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 ENERGY

     O                 -0.06756756   -0.31531531    0.00000000
     H                  0.89243244   -0.31531531    0.00000000
     H                 -0.38802215    0.58962052    0.00000000

Executing QUICK will give us *.out* and additionally, we save the terminal output into a 
*.run.log* file. 

::

     ./quick.cuda water.in > water.run.log 

The content in *water.run.log* is not useful for users. If debug flags are enabled, output
useful for developers can be found here. 

The information reported in *.out* (from now on output) file are as follows. In the beginning of output
file, we can find information about job card and the GPU used for the calculation. The next section 
reports information from SAD initial guess. This will be followed by some information about our molecule 
such as input geometry, basis function information and etc.     

::

  =========== Molecule Input ==========
  | TOTAL MOLECULAR CHARGE  =    0    MULTIPLICITY                =    1
  | TOTOAL ATOM NUMBER      =    3    NUMBER OF ATOM TYPES        =    2
  | NUMBER OF HYDROGEN ATOM =    2    NUMBER OF NON-HYDROGEN ATOM =    1
  | NUMBER OF ELECTRONS     =   10

  -- INPUT GEOMETRY -- :
    O          -0.0676      -0.3153       0.0000
    H           0.8924      -0.3153       0.0000
    H          -0.3880       0.5896       0.0000

  -- DISTANCE MATRIX -- :
              1           2           3
      1     0.00000
      2     1.81414     0.00000
      3     1.81414     2.96300     0.00000

  ============== BASIS INFOS ==============
  | BASIS FUNCTIONS =   25
  | NSHELL =   12 NPRIM  =   32
  | JSHELL =   12 JBASIS =   32


Next we find information on SCF iterations. 

::
 
 ------------------------------------------------------------------------------------------------------------------------
 NCYC       ENERGY         DELTA_E      SCF_TIME  DII_CYC   DII_TIME   O_TIME  DIAG_TIME    MAX_ERR    RMS_CHG    MAX_CHG
 ------------------------------------------------------------------------------------------------------------------------
  1    -76.056050700      ------         0.307     1        0.29      0.02      0.00    0.1775E+01  0.5918E-01  0.3593E+00
  2    -75.980565869  -.754848E-01       0.010     2        0.00      0.01      0.00    0.2376E+00  0.1554E-01  0.1750E+00
  3    -76.017433601  0.368677E-01       0.010     3        0.00      0.01      0.00    0.1050E+00  0.4979E-02  0.6042E-01
  4    -76.025458827  0.802523E-02       0.010     4        0.00      0.01      0.00    0.2584E-01  0.1707E-02  0.1991E-01
  5    -76.026128208  0.669381E-03       0.010     5        0.00      0.01      0.00    0.4594E-02  0.7144E-03  0.5988E-02
  6    -76.026196776  0.685678E-04       0.010     6        0.00      0.01      0.00    0.9251E-03  0.1740E-03  0.1141E-02
  7    -76.026199618  0.284200E-05       0.010     7        0.00      0.01      0.00    0.1452E-03  0.3909E-04  0.2857E-03
  8    -76.026199744  0.126052E-06       0.010     8        0.00      0.01      0.00    0.3826E-04  0.7945E-05  0.7236E-04
  9    -76.026199750  0.583184E-08       0.010     9        0.00      0.01      0.00    0.9753E-05  0.2119E-05  0.1871E-04
 10    -76.026199750  0.388203E-09       0.011    10        0.00      0.01      0.00    0.2026E-05  0.4872E-06  0.4202E-05
 ------------------------------------------------------------------------------------------------------------------------
 REACH CONVERGENCE AFTER  10 CYLCES
 MAX ERROR = 0.202570E-05   RMS CHANGE = 0.487164E-06   MAX CHANGE = 0.420193E-05
 -----------------------------------------------

This is followed by electronic, nuclear and total energies. 

::

 ELECTRONIC ENERGY    =   -85.183315734
 CORE_CORE REPULSION  =     9.157115983
 TOTAL ENERGY         =   -76.026199750

Finally, we find timing information of the calcualtion. 

The output of a DFT energy calculation is very similar to that of HF, however, with a couple of exceptions. 
In the job information section, we can find information about density functional being used. If the functional
is from LIBXC library, related information will show up. Furthermore, timing section at the very end of output
will contain time for performing grid operations (i.e. grid generation, octree run and etc.) and time for
computing exchange correlation energy and matrix elements of potential.  

2. HF/DFT gradient calculation
******************************

The HF/DFT gradient calculation input is the same that we used for single point energy, except the **ENERGY**
keyword is now replaced with **GRADIENT**. Our water example input is now modified as follows.  

::

     HF BASIS=cc-pvDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 GRADIENT

     O                 -0.06756756   -0.31531531    0.00000000
     H                  0.89243244   -0.31531531    0.00000000
     H                 -0.38802215    0.58962052    0.00000000    

In the calculation output, we can find the nuclear gradients immediately after the SCF cycles and energy information 
but before the timings. For above example the printed gradients will be as follows. 

::

 ANALYTICAL GRADIENT:
 ----------------------------------------
 COORDINATE    XYZ            GRADIENT
 ----------------------------------------
    1X    -0.0675675652     0.0126073406
    1Y    -0.3153153341     0.0180535055
    1Z     0.0000000000    -0.0000000303
    2X     0.8924325081    -0.0049459616
    2Y    -0.3153153341    -0.0099345180
    2Z     0.0000000000     0.0000000419
    3X    -0.3880221796    -0.0076370422
    3Y     0.5896205650    -0.0080873988
    3Z     0.0000000000    -0.0000000115
 ---------------------------------------- 

Note that current version of QUICK prints only the total gradients. Therefore, HF and DFT gradient printout remains the
same. Finally, in the timings section we can find gradient timings for 1e, 2e and exchange correlation gradient calculation.

3. HF/DFT geometry optimization calculation
*******************************************

For HF/DFT geometry optimizations, we should specify **OPTIMIZE** keyword in QUICK input. For instance, the geometry optimization input for our water molecule would be: 

::

     HF BASIS=cc-pvDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 OPTIMIZE

     O                 -0.06756756   -0.31531531    0.00000000
     H                  0.89243244   -0.31531531    0.00000000
     H                 -0.38802215    0.58962052    0.00000000

QUICK geometry optimization output will contain information of SCF, gradient and cartesian coordinates for each optimization step. As in the gradient calculation, the analytical gradients will be printed out immediately after the SCF information.

::

 ANALYTICAL GRADIENT:
 ----------------------------------------------------------------------------
 VARIBLES    OLD_X            OLD_GRAD        NEW_GRAD          NEW_X
 ----------------------------------------------------------------------------
    1X    -0.0876252350     0.0000197070     0.0000066787    -0.0876309328
    1Y    -0.3438974485     0.0000305134     0.0000178110    -0.3439209444
    1Z     0.0000002515    -0.0000000336    -0.0000000335     0.0000002992
    2X     0.8793809511    -0.0000260952     0.0000050220     0.8793829892
    2Y    -0.2849281308     0.0000367010     0.0000272289    -0.2849766745
    2Z    -0.0000003515     0.0000000484     0.0000000483    -0.0000004202
    3X    -0.3550174280     0.0000168026    -0.0000012871    -0.3550285769
    3Y     0.5874160803    -0.0000028098     0.0000193489     0.5873964801
    3Z     0.0000001020    -0.0000000153    -0.0000000153     0.0000001238
 ---------------------------------------------------------------------------- 

Next we find information essential for optimization convergence.

::

  OPTIMIZATION STATISTICS:
  ENERGY CHANGE           =   -0.9827189729E-08 (REQUEST= 0.10000E-05)
  MAXIMUM GEOMETRY CHANGE =    0.4854368017E-04 (REQUEST= 0.18000E-02)
  GEOMETRY CHANGE RMS     =    0.1958922994E-04 (REQUEST= 0.12000E-02)
  GRADIENT NORM           =    0.1292934393E-04 (REQUEST= 0.30000E-03)

The cartesian coordinates of the molecular structures pertaining to each optimization step are printed next. 

::

 OPTIMIZED GEOMETRY IN CARTESIAN
 ELEMENT      X         Y         Z
  O       -0.0876   -0.3439    0.0000
  H        0.8794   -0.2850   -0.0000
  H       -0.3550    0.5874    0.0000 

We can also find the energy of the minimum structure at the end of output but before the timings are printed out. 

*Last updated by Madu Manathunga on 03/05/2020.*
