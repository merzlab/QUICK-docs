Known Issues of Current Version
===============================

QUICK is under continous development and as of version 21.03, we have detected
the issues listed below. If you find anything other than these, please feel free to
report any bugs or issues through our Git Hub page: `https://github.com/merzlab/QUICK/issues <https://github.com/merzlab/QUICK/issues>`_.

Runtime
^^^^^^^

1. Input file open error
**************************

::

 Error: Failed to open Unit=  15,file name=
 At line 181 of file quick_molspec_module.f90
 Fortran runtime error: Attempt to DEALLOCATE unallocated 'xyz'

You are trying to run quick without an input file.

Solution: Run quick executable as "./quick input.in". See hands on tutorial section for more information.

2. Basis file open error
************************

::

 Error: Failed to open Unit=  20,file name= /6-31GSS
 At line 326 of file quick_calculated_module.f90
 Fortran runtime error: Attempt to DEALLOCATE unallocated 'self'

You havent set the basis file path.
Solution: Set QUICK_BASIS variable using "export QUICK_BASIS=your_quick_home/basis".

3. Missing basis set information
********************************

If your calculation unexpectedly crashes during SAD initial guess and error messages are printed, you
likely used a basis set that does not have information for a particular atom.

::


 For Atom Kind =    3
 ELEMENT = S
 BASIS FUNCTIONS =    0

 @ Begin Energy Calculation

For example, the above calculation crashed while trying to use a basis set that does not have sulfur.

If you are using serial or MPI version, you may also see a fortran error on the terminal.

::

 Backtrace for this error:
 #0  0x2b4cf76ba27f in ???
 #1  0x2b4cf76ba207 in ???
 #2  0x2b4cf76bb8f7 in ???
 #3  0x2b4cf76fcd26 in ???
 #4  0x2b4cf7705488 in ???
 #5  0x41fa34 in fullx_
        at src/quick_one_electron_integral.f90:26
 #6  0x4232bc in getenergy_
        at src/getEnergy.f90:29
 #7  0x406beb in getmolsad_
        at src/getMolSad.f90:112
 #8  0x403623 in quick
        at src/main.f90:123
 #9  0x403623 in main
        at src/main.f90:28

Solution: You should obtain basis set information for the missing atom from
`https://www.basissetexchange.org/ <https://www.basissetexchange.org/>`_ in *Gaussian* basis file format
and paste this info into correct basis set file inside *basis* folder. Make sure to maintain the format. Otherwise,
you will end up with the same error again.


*Last updated by Madu Manathunga on 01/19/2021.*
