Hands-on Tutorials
==================

Examples of QUICK input files can be found at QUICK_HOME/test and at  this `link <https://github.com/merzlab/QUICK-tests>`_.

For this tutorial we will use one water molecule as an example. The structure of a typical QUICK input is as follows.

.. code-block:: none

     HF BASIS=cc-pVDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 ENERGY  <== Job command
                                                             <== Empty line
     O              -0.06756756   -0.31531531    0.00000000 |
     H               0.89243244   -0.31531531    0.00000000 |<== Coordinates
     H              -0.38802215    0.58962052    0.00000000 |


Before running QUICK, make sure to set the necessary environmental variables as follows:

.. code-block:: none

 source $(installdir)/quick.rc

Where *installdir* is the folder where QUICK was installed. Depending on the installation, you may run QUICK as follows:

Serial version:

.. code-block:: none

     quick input.in

MPI version:

.. code-block:: none

     mpirun -np N quick.MPI input.in

where N is the number of processes that you wish to launch.

CUDA version:

.. code-block:: none

     quick.cuda input.in

CUDA MPI version:

.. code-block:: none

     mpirun -np M quick.cuda.MPI input.in

where M (M <= number of GPUs) is the number of processes that you wish to launch.

Now assume that we have successfully installed CUDA version and set the basis set path.

1. HF/DFT single point energy calculation
*****************************************

The typical keyword line in the input file for a HF/DFT calculation looks like this.
Note that all keywords are separated by a single space.

.. code-block:: none

     HF BASIS=cc-pVDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 ENERGY
     ^     ^              ^             ^              ^
     |     |              |             |              |
     |     |              |             |            Compute energy
     |     |              |             Density matrix cutoff
     |     |              Integral cutoff
     |     Basis set
     Hamiltonian

For DFT, we replace *HF* hamiltonian with *DFT* and specify the functional name separated by a
space. For example, we may ask for a B3LYP energy calculation with the following keyword line.

.. code-block:: none

     DFT B3LYP BASIS=cc-pVDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 ENERGY
     ^     ^      ^              ^             ^              ^
     |     |      |              |             |              |
     |     |      |              |             |            Compute energy
     |     |      |              |             Density matrix cutoff
     |     |      |              Integral cutoff
     |     |      Basis set
     |     Functional
     Hamiltonian

Note that in the above line, we are using NATIVE B3LYP functional. If we want to use
the B3LYP functional through LIBXC, the keyword line should be specified as follows.

.. code-block:: none

     DFT LIBXC=HYB_GGA_XC_B3LYP BASIS=cc-pVDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 ENERGY
     ^     ^                          ^           ^             ^              ^
     |     |                          |           |             |              |
     |     |                          |           |             |            Compute energy
     |     |                          |           |           Density matrix cutoff
     |     |                          |         Integral cutoff
     |     |                         Basis set
     |     Functional
     Hamiltonian

It is also possible to ask for exchange and correlation LIBXC functionals separately.
For instance, if we use BLYP, the keyword line is specified as follows.

.. code-block:: none

     DFT LIBXC=GGA_X_B88,GGA_C_LYP BASIS=cc-pVDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 ENERGY
     ^        ^                          ^           ^             ^              ^
     |        |                          |           |             |              |
     |        |                          |           |             |            Compute energy
     |        |                          |           |             Density matrix cutoff
     |        |                          |       Integral cutoff
     |        |                       Basis set
     |        Functionals (Functional_1, Functional_2 separated by a comma)
     Hamiltonian

**Note:** Currently, QUICK cannot handle more than two functionals at a time.

We now proceed with HF single point energy calculation for a water molecule. Here is the input file,
called *water.in*.

.. code-block:: none

     HF BASIS=cc-pVDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 ENERGY

     O                 -0.06756756   -0.31531531    0.00000000
     H                  0.89243244   -0.31531531    0.00000000
     H                 -0.38802215    0.58962052    0.00000000

Executing QUICK will give us an *water.out* file. Here is how to run using the CUDA version of QUICK.

.. code-block:: none

     ./quick.cuda water.in

The information reported in the *water.out* file are as follows. In the beginning of the output
file, we can find information about job card and the GPU used for the calculation. The next section
reports information from SAD initial guess. This will be followed by some information about the molecule
such as input geometry, basis function information, etc.

.. code-block:: none

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


Next we find information about the SCF iterations.

.. code-block:: none

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

.. code-block:: none

 ELECTRONIC ENERGY    =   -85.183315734
 CORE_CORE REPULSION  =     9.157115983
 TOTAL ENERGY         =   -76.026199750

Finally, we find timing information about the calculation.

2. HF/DFT gradient calculation
******************************

For a HF/DFT gradient calculation input the **ENERGY** flag is replaced by **GRADIENT**.
Our water example input is now modified as follows.

.. code-block:: none

     HF BASIS=cc-pVDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 GRADIENT

     O                 -0.06756756   -0.31531531    0.00000000
     H                  0.89243244   -0.31531531    0.00000000
     H                 -0.38802215    0.58962052    0.00000000

In the calculation output, we can find the gradient immediately after the SCF cycles and energy information,
and before the timings. The above example will print the following gradient.

.. code-block:: none

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

Finally, the timings section also shows gradient timings for 1e, 2e and exchange correlation calculations.

3. HF/DFT geometry optimization calculation
*******************************************

For HF/DFT geometry optimizations, we should specify the **OPTIMIZE** flag in the QUICK input.
For instance, the geometry optimization input for our water molecule would be:

.. code-block:: none

     HF BASIS=cc-pVDZ CUTOFF=1.0d-10 DENSERMS=1.0d-6 OPTIMIZE

     O                 -0.06756756   -0.31531531    0.00000000
     H                  0.89243244   -0.31531531    0.00000000
     H                 -0.38802215    0.58962052    0.00000000

QUICK geometry optimization output will contain information of SCF, gradient and cartesian coordinates for
each optimization step. As in the gradient calculation, the analytical gradients will be printed out immediately
after the SCF information.

.. code-block:: none

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

Next we find information essential for the convergence of geometry optimization.

.. code-block:: none

  OPTIMIZATION STATISTICS:
  ENERGY CHANGE           =   -0.9827189729E-08 (REQUEST= 0.10000E-05)
  MAXIMUM GEOMETRY CHANGE =    0.4854368017E-04 (REQUEST= 0.18000E-02)
  GEOMETRY CHANGE RMS     =    0.1958922994E-04 (REQUEST= 0.12000E-02)
  GRADIENT NORM           =    0.1292934393E-04 (REQUEST= 0.30000E-03)

The cartesian coordinates of the molecular geometry on each optimization step are printed next.

.. code-block:: none

 OPTIMIZED GEOMETRY IN CARTESIAN
 ELEMENT      X         Y         Z
  O       -0.0876   -0.3439    0.0000
  H        0.8794   -0.2850   -0.0000
  H       -0.3550    0.5874    0.0000

We can also find the energy of the minimum structure at the end of output, right before the timings are printed out.

*Last updated by Madu Manathunga on 02/05/2021.*
