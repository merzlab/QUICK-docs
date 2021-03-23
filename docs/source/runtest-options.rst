Testing QUICK using runtest script
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This page gives a summary of options available in QUICK runtest script and how to use it. If you want test QUICK manually, you must copy the script from $QUICK_HOME/tools into $QUICK_HOME or installation directory ($installdir). Then execute it as:

.. code-block:: none

	./runtest [flags]

Available flags
***************

• **--serial**: Builds a serial version. (Default)
• **--mpi**: Compiles MPI parallel version.
• **--cuda**: Builds GPU version that utilizes a single NVIDIA GPU.
• **--cudampi**: Builds multi-GPU version that utilizes multiple NVIDIA GPUs.
• **--ene**: Run only energy tests.
• **--grad**: Run only gradient tests.
• **--opt**: Run only geometry optimization tests.
• **--api**: Run only api tests.
• **--full**: Run a large set of tests.
• **--nolog**: Disable writing output into runtest.log.
• **--help**: Prints help.

If the version flags are not specified, the script will try to detect the executables and test them.

*Last updated by Madu Manathunga on 03/21/2021.*
