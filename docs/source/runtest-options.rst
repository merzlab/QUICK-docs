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
• **--chk**: Run only checkpoint functionality tests.
• **--esp**: Run only ESP tests.
• **--full**: Run a large set of tests.
• **--nolog**: Disable writing output into runtest.log.
• **--help**: Prints help.

If the version flags are not specified, the script will try to detect the executables and test them.

Environment variables
*********************
• **DO_PARALLEL**: Command for launching MPI/MPI+X versions
                   Example: DO_PARALLEL="mpirun -np 2"
• **CUDA_VISIBLE_DEVICES**: Comma-seperated list of GPU IDs to use for GPU versions (CUDA/HIP)
                            Example: CUDA_VISIBLE_DEVICES="0,1"
• **HIP_VISIBLE_DEVICES**: Comma-seperated list of GPU IDs to use for GPU versions (HIP)
                           Example: HIP_VISIBLE_DEVICES="2,3"
• **PARALLEL_TEST_COUNT**: Number of tests to run in parallel using GNU parallel; can be combined with the above 3 variables for parallel testing with applicable QUICK versions
                           Example: PARALLEL_TEST_COUNT="2"

*Last updated by Kurt O'Hearn on 04/13/2026.*
