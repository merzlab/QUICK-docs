A List of Available Basis Sets
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

QUICK-21.03 is currently equipped with following basis sets. 

.. code-block:: none

  STO-3G      
  3-21G       
  6-31G       
  6-31G* or 6-31G(d)      
  6-31G** or 6-31G(d,p)    
  6-311G 
  6-311G* or 6-311G(d)
  6-311G** or 6-311G(d,p)
  6-31+G* or 6-31+G(d)
  6-31+G** or 6-31+G(d,p)
  6-31++G** or 6-31++G(d,p)
  6-311+G(2d,p)
  6-311++G(2d,2p)
  cc-pVDZ
  def2-svp
  def2-sv(p)
  def2-svpd
  PC-0
  PC-1

Note 1: We follow the same basis set names reported in `basis set exchange web page <https://www.basissetexchange.org/>`_. 

Note 2: Current version of QUICK ERI engine only support basis functions up to *d*. Therefore, energy and gradient calculations with functions up to *d* are possible.

Note 3: ECPs are currently not supported by QUICK. Due to this reason, we have excluded elements that require ECPs from the above basis sets.

