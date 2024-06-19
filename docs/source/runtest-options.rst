Testing QUICK using runtest script
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This page gives a summary of options available in QUICK runtest script and how
to use it. If you want test QUICK manually, you must copy the script from
$QUICK_HOME/tools into $QUICK_HOME or installation directory ($installdir).
Then execute it as:

.. code-block:: none

	./runtest [flags]

Available flags
***************

• **--serial**: Tests a serial version. (Default)
• **--mpi**: Tests MPI parallel version.
• **--cuda**: Tests GPU version that utilizes a single NVIDIA GPU.
• **--cudampi**: Tests multi-GPU version that utilizes multiple NVIDIA GPUs (MPI+CUDA).
• **--hip**: Tests GPU version that utilizes a single AMD GPU.
• **--hipmpi**: Tests multi-GPU version that utilizes multiple AMD GPUs (MPI+HIP).
• **--ene**: Run only energy tests.
• **--grad**: Run only gradient tests.
• **--opt**: Run only geometry optimization tests.
• **--api**: Run only api tests.
• **--full**: Run a large set of tests.
• **--nolog**: Disable writing output into runtest.log.
• **--help**: Prints help.

If the version flags are not specified, the script will try to detect the executables and test them.

*Last updated by Madu Manathunga on 11/21/2022.*
