A List of Available Basis Sets
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

QUICK-20.03 is currently equipped with following basis sets. 

::

  STO-3G      
  3-21G       
  6-31G       
  6-31G*      
  6-31G**     
  6-311G      
  6-311G(d,p) 
  6-311G*     
  6-311G**    
  cc-pVDZ       

Note 1: We follow the same basis set names reported in `basis set exchange web page <https://www.basissetexchange.org/>`_. 

Note 2: Current version of QUICK ERI engine only support basis functions upto *f*. Therefore, energy calculations with *g* 
functions and above, gradient calculations with *g* functions and above are not possible.  

Adding a Basis Set into QUICK 
*****************************

In addition to above basis sets, it is also possible to add a basis set into QUICK by yourself. In order to do so, you should download a basis set from `basis set exchange web page <https://www.basissetexchange.org/>`_ in *Gaussian* software format and save it into your *basis* folder. Then, link this basis set to QUICK by updating the *basis_link* file inside the *basis* folder. The *basis_link* file contains a table in the following format.

::

 _________________________________________________________________________ 
 | Keyword                           | Filename                            |
 |-------------------------------------------------------------------------|
 | #STO-3G                           | STO-3G.BAS                          |
 | #3-21G                            | 3-21G.BAS                           |
 | #6-31G                            | 6-31G.BAS                           |
 | #6-31G*                           | 6-31GS.BAS                          |
 | #6-31G**                          | 6-31GSS.BAS                         |
 | #6-311G                           | 6-311G.BAS                          |
 | #6-311G(d,p)                      | 6-311GDP.BAS                        |
 | #6-311G*                          | 6-311GS.BAS                         |
 | #6-311G**                         | 6-311GSS.BAS                        |
 | #cc-pVDZ                          | CC-PVDZ.BAS                         |
 | #cc-pVTZ                          | CC-PVTZ.BAS                         |
 |_________________________________________________________________________|    
         
You should update this table by adhering to the rules below. 

 1. Add a keyword for your basis set. This must be followed by a single space and '#' character.

 2. Keyword size must be less than 32 characters.

 3. Filename must start at 24th position of the line and must not be longer than 36 characters.

 4. DO NOT CHANGE THE TABLE/COLUMN WIDTH! VERTICAL BORDERS MUST REMAIN THE SAME.  

