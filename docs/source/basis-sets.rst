.. code-block:: none

  STO-3G      
  3-21G       
  6-31G       
  6-31G* or 6-31G(d)      
  6-31G** or 6-31G(d,p)    
  6-311G 
  6-311G* or 6-311G(d)
  6-311G** or 6-311G(d,p)
  6-311G(2df,2pd)
  6-31+G* or 6-31+G(d)
  6-31+G** or 6-31+G(d,p)
  6-31++G** or 6-31++G(d,p)
  6-311+G(2d,p)
  6-311++G(2d,2p)
  cc-pVDZ
  cc-pVTZ
  aug-cc-pVDZ
  aug-cc-pVTZ
  def2-SV(P)
  def2-SVP
  def2-SVPD
  def2-TZVP
  def2-TZVPD
  def2-TZVPP
  def2-TZVPPD
  PC-0
  PC-1
  PC-2
  aug-PC-1
  aug-PC-2

Note 1: We follow the same basis set names reported at the
`basis set exchange web page <https://www.basissetexchange.org/>`_. 

Note 2: The current version of the QUICK two elecron repulsion integral (ERI)
engine only support basis functions up to *f*. Therefore, energy and gradient
calculations with functions up to *f* are possible. By default, *f* functions
are disabled in the GPU code.  Open-shell gradient calculations with *f*
functions are not yet available on GPU.

Note 3: Effective core potentials (ECPs) are currently not supported by QUICK.
Due to this reason, we have excluded elements that require ECPs from the above
basis sets that are included with QUICK.
