Accuracy and Performance
========================

Accuracy of energies and gradients
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We have compared energies and gradients computed by QUICK with values computed by
other quantum chemical packages. HF energies and gradients have displayed
accuracies up to 1.0E-6 Hartree and 1.0E-4 Hartree/Bohr for test systems (see `https://github.com/merzlab/QUICK-tests <https://github.com/merzlab/QUICK-tests>`_ for
test cases). DFT energies and gradients have shown similar accuracies in most cases, however, we have observed
larger deviations for some molecular systems. Such deviations usually arise due to differences in exchange 
correlation quadrature.     

Performance of QUICK CUDA serial and MPI parallel versions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. image:: bench1.png
    :width: 650px
    :align: center
    :height: 460px
    :alt: bench1  

Performance of QUICK CUDA MPI version
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. image:: bench2.png
    :width: 1067px
    :align: center
    :height: 450px
    :alt: bench2

*Last updated by Madu Manathunga on 03/23/2021.*
