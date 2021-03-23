User Manual
=================

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

   QUICK also makes use of LIBXC density functional library (version 4.0.4). A LIBXC functional can be requested as follows.

   **LIBXC=FUNCTIONAL1,FUNCTIONAL2** : Where FUNCTIONAL1 and FUNCTIONAL2 are exchange and correlation functionals. *Note: Spaces near '=' or ',' are not allowed.*

   See here for `a list of working functionals in current QUICK version <working_libxc_funcs.html>`_.

   If you are using LIBXC in your work, please make sure to cite the paper below.

   `Susi Lehtola, Conrad Steigemann, Micael J.T. Oliveira, and Miguel A.L. Marques, Recent developments
   in Libxc - A comprehensive library of functionals for density functional theory, Software X 7, 1 (2018) <https://www.sciencedirect.com/science/article/pii/S2352711017300602?via%3Dihub>`_.

3. Basis sets
*************

**BASIS=BASIS_SET_NAME** : Selects *BASIS_SET_NAME* basis set for the calculation. *BASIS_SET_NAME* could be
any of the following.

.. include:: ./basis-sets.rst

4. SCF Convergence
******************

   **SCF=Integer**    : user defined maximum self-consistent field cycles = Integer. Default: 200

   **DENSERMS=FLOAT** : user defined density matrix maximum RMS for convergence. Default : 1.0E-6.

5. Atomic Charges
*****************

   **CHARGE=INT**     : A net charge is to be placed on system.

   **EXTCHARGES**     : External charges are included in the system.

   **DIPOLE**       : Write dipole moments, Mulliken and Löwdin charges into the output file.

6. Geometry Optimization
************************

   **OPTIMIZE=Integer** : Performs a maximum of *Integer* cycles of optimization. Default: 3 x Number of atoms.

   **GRADIENT**         : Calculates analytical gradients.

*Last updated by Madu Manathunga on 03/22/2021.*
