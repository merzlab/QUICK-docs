User Manual
=================

QUICK uses a simple text based input file that consists of a line with keywords
followed by an empty line, the molecular coordinates in xyz format, and
potentially point charge information after another empty line.  Please see the
Hands-on Tutorials for details on the input file format.

Feel free to ask questions or start a discussion on the Discussions section of our GitHub page:
`https://github.com/merzlab/QUICK/discussions <https://github.com/merzlab/QUICK/discussions>`_.

Units
^^^^^

The QUICK input file requires atomic coordinates in Angstrom and charges
(molecular charge and point charges) in atomic units.

The QUICK output reports coordinates in Angstroms, and charges, energies, and
gradients in atomic units. This means that energies are reported in Hartree and
gradients in Hartree/Bohr.

Keywords for QUICK Input
^^^^^^^^^^^^^^^^^^^^^^^^

1. Hamiltonians
***************

   **HF**   : Hartree-Fock Hamiltonian to be used

   **UHF**  : Unrestricted Hartree-Fock Hamiltonian to be used

   **DFT**  : Density Functional Theory to be used.

   **UDFT** : Unrestricted Density Functional Theory to be used.
   
   NOTE: One Hamiltonian must be selected. There is no default.


2. Density Functional Theory
****************************

When DFT is selected as the Hamiltonian, an additional keyword is required to
specify which DFT method to use, followed by optional additional keywords for
DFT-related parameters.

Built-in Methods
----------------

QUICK has two built-in DFT methods:

   **BLYP** : Built in BLYP functional (A. D. Becke, Phys. Rev. A 38, 3098 (1988), C. Lee, W. Yang, and R. G. Parr, Phys. Rev. B 37, 785 (1988), B. Miehlich, A. Savin, H. Stoll, and H. Preuss, Chem. Phys. Lett. 157, 200 (1989))

   **B3LYP**: Built in B3LYP functional (P. J. Stephens, F. J. Devlin, C. F. Chabalowski, and M. J. Frisch, J. Phys. Chem. 98, 11623 (1994) )

Functional Support via libxc
----------------------------

Except for the built-in BLYP and B3LYP functionals QUICK makes use of the Libxc
density functional library (version 4.0.4). The following functionals can be
requested by their popular names:

   **BP86**: BP86 functional (A. D. Becke, Phys. Rev. A 38, 3098 (1988), J. P. Perdew, Phys. Rev. B 33, 8822 (1986))

   **B97**: B97 functional (A. D. Becke, J. Chem. Phys. 107, 8554 (1997))

   **B97-GGA1**: B97-GGA1 functional (A. J. Cohen and N. C. Handy, Chem. Phys. Lett. 316, 160 (2000))

   **PW91**: PW91 functional (J. P. Perdew, J. A. Chevary, S. H. Vosko, K. A. Jackson, M. R. Pederson, D. J. Singh, and C. Fiolhais, Phys. Rev. B 46, 6671 (1992))

   **OLYP**: OLYP functional (N. C. Handy and A. J. Cohen, Mol. Phys. 99, 403 (2001), C. Lee, W. Yang, and R. G. Parr, Phys. Rev. B 37, 785 (1988))

   **O3LYP**: O3LYP functional (A. J. Cohen and N. C. Handy, Mol. Phys. 99, 607 (2001))
   
   **PBE**: PBE functional (J. P. Perdew, K. Burke, and M. Ernzerhof, Phys. Rev. Lett. 77, 3865 (1996), J. P. Perdew, K. Burke, and M. Ernzerhof, Phys. Rev. Lett. 78, 1396 (1997))

   **REVPBE**: Revised PBE functional (Y. Zhang and W. Yang, Phys. Rev. Lett. 80, 890 (1998), J. P. Perdew, K. Burke, and M. Ernzerhof, Phys. Rev. Lett. 77, 3865 (1996), J. P. Perdew, K. Burke, and M. Ernzerhof, Phys. Rev. Lett. 78, 1396 (1997))

   **PBE0**: PBE0/PBEH functional (C. Adamo and V. Barone, J. Chem. Phys. 110, 6158 (1999), M. Ernzerhof and G. E. Scuseria, J. Chem. Phys. 110, 5029 (1999))

All LDA, GGA and hybrid-GGA functionals available in Libxc can be requested. As
a reminder, (hybrid) meta-GGA and range-separated hybrid functionals are not
supported at present.  You must make sure to select a valid combination of
exchange and correlation functionals. A generic Libxc functional can be
requested as follows:

   **LIBXC=FUNCTIONAL1,FUNCTIONAL2** : Where FUNCTIONAL1 and FUNCTIONAL2 are exchange and correlation functionals. *Note: Spaces near '=' or ',' are not allowed.*

In addition to methods previously noted, a full list of the available and
tested functionals can be found here: `libxc supported functionals in the current QUICK version <working_libxc_funcs.html>`_.

If you are using Libxc in your work, please cite following paper:

   `Susi Lehtola, Conrad Steigemann, Micael J.T. Oliveira, and Miguel A.L. Marques, Recent developments
   in Libxc - A comprehensive library of functionals for density functional theory, Software X 7, 1 (2018) <https://www.sciencedirect.com/science/article/pii/S2352711017300602?via%3Dihub>`_.

Grimme Dispersion Corrections
-----------------------------

Grimme type dispersion corrections are requested by adding one of the following keywords:

   **D2** : use Grimme's D2 dispersion correction in DFT.

   **D3** : use Grimme's D3 dispersion correction with zero damping.

   **D3BJ** : use Grimme's D3 dispersion correction with Becke-Johnson damping.
 
   **D3M** : use Grimme's D3 dispersion correction with modified zero damping by Sherrill and coworkers.

   **D3MBJ** - use Grimme's D3 dispersion correction with modified Becke-Johnson damping by Sherrill and coworkers.

   **XCCUTOFF=Float**: user defined threshold for grid pruning in exchange correlation algorithm. Default: 1.0E-7  

3. Basis sets
*************

**BASIS=BASIS_SET_NAME** : Selects *BASIS_SET_NAME* basis set for the calculation. *BASIS_SET_NAME* could be any of the following. The basis set name is not case sensitive.

.. include:: ./basis-sets.rst

4. SCF Convergence
******************

   **SCF=Integer**    : user defined maximum self-consistent field cycles = Integer. Default: 200

   **NCYC=Integer**   : user defined self-consistent field cycles to turn on incremental Fock build = Integer. Default: 3

   **DENSERMS=Float** : user defined density matrix maximum RMS for convergence. Default : 1.0E-6.

   **CUTOFF=Float**   : user defined integral cutoff. Default : 1.0E-7.

   **BASISCUTOFF=Float**   : user defined cutoff for neglecting insignificant basis functions. Default : 1.0E-6.

   **COARSEINT**      : use coarse cutoffs. (i.e. DENSERMS=1.0E-5, CUTOFF=1.0E-6, BASISCUTOFF=1.0E-5, XCCUTOFF=1.0E-6)

   **TIGHTINT**       : use tight cutoffs.  (i.e. DENSERMS=1.0E-7, CUTOFF=1.0E-8, BASISCUTOFF=1.0E-7, XCCUTOFF=1.0E-8)

5. Charge and Spin Multiplicity
*******************************

   **CHARGE=Integer** : A net charge is to be placed on system. Default: 0

   **MULT=Integer**   : Spin multiplicity of the system. Default: 1

6. Atomic Charges
*****************

   **EXTCHARGES**     : External charges are included in the system. The point charges must be listed after the molecular Cartesian coordinates in the input file. See the corresponding section in the Hands-on Tutorials of this manual.

   **DIPOLE**       : Write dipole moments, Mulliken and Löwdin charges into the output file.

7. Gradient Calculation
***********************

   **GRADIENT**         : Calculates analytical gradients.

   **GRADCUTOFF=Float** : user defined cutoff for gradients. Default : 1.0E-7 (automatically set to 1.0E-6 or 1.0E-8 if COARSEINT or TIGHTINT keyword is specified) 

8. Geometry Optimization
************************

   **OPTIMIZE=Integer** : Performs a geometry optimization with a maximum of *Integer* steps. Default: 3 x Number of atoms.

   **DLFIND**           : Use DL-FIND optimizer. This is the default. 

   **LOPT**             : Use legacy QUICK optimizer instead of DL-FIND.

   **GTOL**             : User defined maximum RMS value of the gradient vector. Default: 4.5E-4 (using tighter convergence criteria may require tightening SCF convergence, see TIGHTINT keyword)

   **ETOL**             : User defined maximum energy change between two consecutive optimization cycles. Default: 1.0E-6

   **ICOORD=Integer**   : User defined coordinate system for DL-Find geometry optimization. Default: 3 (delocalized internal coordinates(DLC)). Other available option is 0 (Cartesian).

   **ALLOW_BAD_SCF**  : Allow unconverged SCF in geometry optimization. By default, the optimization will not proceed if the SCF fails to converge. 

If you are using the DL-FIND optimizer in your work, please make sure to cite the following paper:

   `Johannes Kästner, Joanne M. Carr, Thomas W. Keal, Walter Thiel, Adrian Wander, and Paul Sherwood, DL-FIND: An Open-Source Geometry Optimizer for Atomistic Simulations, J. Phys. Chem. A, 2009, 113 (43), 11856-11865. <https://pubs.acs.org/doi/10.1021/jp9028968>`_.

9. Data Exporting
*****************

   **EXPORT=MOLDEN** : Generates a molden file that contains orbitals, charges, geometries, etc. 

*Last updated by Andy Goetz on 04/25/2024.*
