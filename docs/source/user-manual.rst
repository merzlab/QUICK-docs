User Manual
=================

Keywords for QUICK Input
^^^^^^^^^^^^^^^^^^^^^^^^

1. Hamiltonians
***************

   **HF**   : Hartree-Fock Hamiltonian to be used

   **DFT**  : Density Functional Theory to be used.

   NOTE 1 : One Hamiltonian must be selected. There is no default.
   
   NOTE 2 : UHF or UDFT calculations are currently not available. We are working hard to enable them ASAP.

2. Density Functional Theory
****************************

   **BLYP** : Built in BLYP functional

   **B3LYP**: Built in B3LYP functional

   QUICK also makes use of LIBXC density functional library. A LIBXC functional can be requested as follows.

   **LIBXC=FUNCTIONAL1,FUNCTIONAL2** : Where FUNCTIONAL1 and FUNCTIONAL2 are exchange and correlation functionals. 

   See here for `a list of working functionals in current QUICK version <user-manual.html#a-list-of-available-dft-functionals>`_.

   If you are using LIBXC in your work, please make sure to cite the paper below.
  
   `Susi Lehtola, Conrad Steigemann, Micael J.T. Oliveira, and Miguel A.L. Marques, Recent developments 
   in Libxc - A comprehensive library of functionals for density functional theory, Software X 7, 1 (2018) <https://www.sciencedirect.com/science/article/pii/S2352711017300602?via%3Dihub>`_.

3. Basis sets
*************

**BASIS=BASIS_SET_NAME** : Selects *BASIS_SET_NAME* basis set for the calculation. *BASIS_SET_NAME* could be
`any of these available basis sets <basis-sets.html#a-list-of-available-basis-sets>`_ or a basis set added to
*basis* folder by yourself (`see here for more information <basis-sets.html#adding-a-basis-set-into-quick>`_ ).

4. SCF Convergence
******************

   **SCF=Integer**    : user defined maximum self-consistent field cycles = Integer. Default: 200

   **DENSERMS=FLOAT** : user defined density matrix maximum RMS for convergence. Default : 1.0D-8.

5. Atomic Charges
*****************

   **CHARGE=INT**     : A net charge is to be placed on system.

   **MULLIKEN**       : Write Mulliken charges to charge output file.

6. Geometry Optimization
************************

   **OPTIMIZE=Integer** : Performs a maximum of *Integer* cycles of optimization. Default: 3 x Number of atoms.

   **GRADIENT**         :Calculates analytical gradients.

.. include:: ./basis-sets.rst

.. include:: ./working_libxc_funcs.rst
