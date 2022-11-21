User Manual
=================

QUICK uses a simple text based input file that consists of a line with keywords followed by an empty line, the molecular coordinates in xyz format, and potentially point charge information after another empty line. 
Please see the Hands-on Tutorials for details on the input file format. 

Units
^^^^^

The QUICK input file requires atomic coordinates in Angstrom and charges (molecular charge and point charges) in atomic units.

The QUICK output reports coordinates in Angstrom and charges, energies and gradients in atomic units. This means that energies are reported in Hartree and gradients in Hartree/Bohr.

Keywords for QUICK Input
^^^^^^^^^^^^^^^^^^^^^^^^

1. Hamiltonians
***************

   **HF**   : Hartree-Fock Hamiltonian to be used

   **DFT**  : Density Functional Theory to be used.

   NOTE 1 : One Hamiltonian must be selected. There is no default.

   NOTE 2 : UHF or UDFT calculations are under development and currently not available.

2. Density Functional Theory
****************************

   **BLYP** : Built in BLYP functional (A. D. Becke, Phys. Rev. A 38, 3098 (1988), C. Lee, W. Yang, and R. G. Parr, Phys. Rev. B 37, 785 (1988), B. Miehlich, A. Savin, H. Stoll, and H. Preuss, Chem. Phys. Lett. 157, 200 (1989))

   **B3LYP**: Built in B3LYP functional (P. J. Stephens, F. J. Devlin, C. F. Chabalowski, and M. J. Frisch, J. Phys. Chem. 98, 11623 (1994) )

   **BP86**: BP86 functional (A. D. Becke, Phys. Rev. A 38, 3098 (1988), J. P. Perdew, Phys. Rev. B 33, 8822 (1986))

   **B97**: B97 functional (A. D. Becke, J. Chem. Phys. 107, 8554 (1997))

   **B97-GGA1**: B97-GGA1 functional (A. J. Cohen and N. C. Handy, Chem. Phys. Lett. 316, 160 (2000))

   **PW91**: PW91 functional (J. P. Perdew, J. A. Chevary, S. H. Vosko, K. A. Jackson, M. R. Pederson, D. J. Singh, and C. Fiolhais, Phys. Rev. B 46, 6671 (1992))

   **OLYP**: OLYP functional (N. C. Handy and A. J. Cohen, Mol. Phys. 99, 403 (2001), C. Lee, W. Yang, and R. G. Parr, Phys. Rev. B 37, 785 (1988))

   **O3LYP**: O3LYP functional (A. J. Cohen and N. C. Handy, Mol. Phys. 99, 607 (2001))
   
   **PBE**: PBE functional (J. P. Perdew, K. Burke, and M. Ernzerhof, Phys. Rev. Lett. 77, 3865 (1996), J. P. Perdew, K. Burke, and M. Ernzerhof, Phys. Rev. Lett. 78, 1396 (1997))

   **REVPBE**: Revised PBE functional (Y. Zhang and W. Yang, Phys. Rev. Lett. 80, 890 (1998), J. P. Perdew, K. Burke, and M. Ernzerhof, Phys. Rev. Lett. 77, 3865 (1996), J. P. Perdew, K. Burke, and M. Ernzerhof, Phys. Rev. Lett. 78, 1396 (1997))

   **PBE0**: PBE0/PBEH functional (C. Adamo and V. Barone, J. Chem. Phys. 110, 6158 (1999), M. Ernzerhof and G. E. Scuseria, J. Chem. Phys. 110, 5029 (1999))

   Except for the built-in BLYP and B3LYP functionals QUICK makes use of the LIBXC density functional library (version 4.0.4). A LIBXC functional can be requested as follows.

   **LIBXC=FUNCTIONAL1,FUNCTIONAL2** : Where FUNCTIONAL1 and FUNCTIONAL2 are exchange and correlation functionals. *Note: Spaces near '=' or ',' are not allowed.*

   See here for a `list of available functionals in the current QUICK version <working_libxc_funcs.html>`_.

   If you are using LIBXC in your work, please make sure to cite the paper below.

   `Susi Lehtola, Conrad Steigemann, Micael J.T. Oliveira, and Miguel A.L. Marques, Recent developments
   in Libxc - A comprehensive library of functionals for density functional theory, Software X 7, 1 (2018) <https://www.sciencedirect.com/science/article/pii/S2352711017300602?via%3Dihub>`_.

   **XCCUTOFF=FLOAT**: user defined threshold for grid pruning in exchange correlation algorithm. Default: 1.0E-7  

3. Basis sets
*************

**BASIS=BASIS_SET_NAME** : Selects *BASIS_SET_NAME* basis set for the calculation. *BASIS_SET_NAME* could be
any of the following.

.. include:: ./basis-sets.rst

4. SCF Convergence
******************

   **SCF=Integer**    : user defined maximum self-consistent field cycles = Integer. Default: 200

   **DENSERMS=FLOAT** : user defined density matrix maximum RMS for convergence. Default : 1.0E-6.

   **CUTOFF=FLOAT**   : user defined integral cutoff. Default : 1.0E-8.

   **BASISCUTOFF=FLOAT**   : user defined cutoff for neglecting insignificant basis functions. Default : 1.0E-6.

   **COARSEINT**      : use coarse cutoffs. (i.e. DENSERMS=1.0E-5, CUTOFF=1.0E-6, BASISCUTOFF=1.0E-5, XCCUTOFF=1.0E-6)

   **TIGHTINT**       : use tight cutoffs.  (i.e. DENSERMS=1.0E-7, CUTOFF=1.0E-8, BASISCUTOFF=1.0E-7, XCCUTOFF=1.0E-8)

5. Atomic Charges
*****************

   **CHARGE=INT**     : A net charge is to be placed on system.

   **EXTCHARGES**     : External charges are included in the system. The point charges must be listed after the molecular Cartesian coordinates in the input file. See the corresponding section in the Hands-on Tutorials of this manual.

   **DIPOLE**       : Write dipole moments, Mulliken and Löwdin charges into the output file.

6. Gradient Calculation
***********************

   **GRADIENT**         : Calculates analytical gradients.

   **GRADCUTOFF=FLOAT** : user defined cutoff for gradients. Default : 1.0E-7 (automatically set to 1.0E-6 or 1.0E-8 if COARSEINT or TIGHTINT keyword is specified) 

7. Geometry Optimization
************************

   **OPTIMIZE=Integer** : Performs a geometry optimization with a maximum of *Integer* steps. Default: 3 x Number of atoms.

   **DLFIND**           : Use DL-FIND optimizer. This is the default. 

   **LOPT**             : Use legacy QUICK optimizer instead of DL-FIND.

   **GTOL**             : User defined maximum RMS value of the gradient vector. Default: 4.5E-4  

   **ETOL**             : User defined maximum energy change between two consecutive optimization cycles. Default: 1.0E-6

   **ICOORD=INTEGER**   : User defined coordinate system for DL-Find geometry optimization. Default: 3 (delocalized internal coordinates(DLC)). Other available option is 0 (cartesian).

   **ALLOW_BAD_SCF**  : Allow unconverged SCF in geometry optimization. By default, the optimization will not proceed if the SCF fails to converge. 

   If you are using DL-FIND optimizer in your work, please make sure to cite the paper below.

   `Johannes Kästner, Joanne M. Carr, Thomas W. Keal, Walter Thiel, Adrian Wander, and Paul Sherwood, DL-FIND: An Open-Source Geometry Optimizer for Atomistic Simulations, J. Phys. Chem. A, 2009, 113 (43), 11856-11865. <https://pubs.acs.org/doi/10.1021/jp9028968>`_.

8. Data Exporting
*****************

   **EXPORT=MOLDEN** : Generates a molden file that contains orbitals, charges, geometries, etc. 

*Last updated by Madu Manathunga on 09/29/2022.*
