Accuracy and Performance
========================

The data reported in this section is solely intended for informative purpose. Readers shall not use them for comparison with other quantum chemical codes as they are. If you are interested in the latter, we highly encourage you to download QUICK, compile and perform your own benchmarks.    

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

See the following preprint for more benchmarks of QUICK multi-GPU version: `Manathunga, M.; Chi, J.; Cruzeiro, V. W. D.; Miao, Y.; Mu, D.; Arumugam, K.; Keipert, K.; Aktulga, H. M.; Merz, K. M.; Götz, A. W. Harnessing the Power of Multi-GPU-Acceleration into the Quantum Interaction Computational Kernel Program. ChemRxiv Preprint, 2021. <https://doi.org/10.26434/chemrxiv.13769209.v1>`_.

*Last updated by Madu Manathunga on 03/23/2021.*
