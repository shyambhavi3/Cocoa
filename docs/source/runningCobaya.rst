Running Cobaya Examples
=======================

Assuming the user opted for the easier *Conda installation* and located the terminal at the folder *where Cocoa was cloned*, this is how to run some example YAML files we provide (*no Cosmolike code involved*):

**Step 1 of 5**: activate the conda environment

.. code-block:: console

    $ conda activate cocoapy38

**Step 2 of 5**: go to the Cocoa main folder

.. code-block:: console

    $(cocoapy38) cd ./cocoa/Cocoa

**Step 3 of 5**: activate the private python environment

.. code-block:: console

    $(cocoapy38) source start_cocoa

Users will see a terminal that looks like this: ``$(cocoapy38)(.local)``. **This is a feature, not a bug!**

Why did we choose to have two separate bash environments? Users should be able to manipulate multiple Cocoa instances seamlessly, which is particularly useful when running chains in one instance while experimenting with code development in another. Consistency of the environment across all Cocoa instances is crucial, and the start_cocoa/stop_cocoa scripts handle the loading and unloading of environmental path variables for each Cocoa. All of them, however, depends on many of the same prerequisites, so it is advantageous to maintain the basic packages inside the shared conda cocoa environment.

**Step 4 of 5**: select the number of OpenMP cores

.. code-block:: console

    $(cocoapy38)(.local) export OMP_PROC_BIND=close; export OMP_NUM_THREADS=4

**Step 5 of 5**: run cobaya-run on a the first example YAML files we provide.

One model evaluation:

.. code-block:: console

    $(cocoapy38)(.local) mpirun -n 1 --oversubscribe --mca btl vader,tcp,self --bind-to core:overload-allowed --rank-by core --map-by numa:pe=${OMP_NUM_THREADS} cobaya-run  ./projects/example/EXAMPLE_EVALUATE1.yaml -f


PS: We offer the flag ``COCOA_RUN_EVALUATE`` as an alias (syntax-sugar) for ``mpirun -n 1 --oversubscribe --mca btl vader,tcp,self --bind-to core:overload-allowed --rank-by core --map-by numa:pe=4 cobaya-run``.

MCMC:

.. code-block:: console

    $(cocoapy38)(.local) mpirun -n 4 --oversubscribe --mca btl vader,tcp,self --bind-to core:overload-allowed --rank-by core --map-by numa:pe=${OMP_NUM_THREADS} cobaya-run ./projects/example/EXAMPLE_MCMC1.yaml -f


PS: We offer the flag ``COCOA_RUN_MCMC`` as an alias (syntax-sugar) for ``mpirun -n 4 --oversubscribe --mca btl vader,tcp,self --bind-to core:overload-allowed --rank-by core --map-by numa:pe=4 cobaya-run``.

📚 **expert** 📚 Why the ``--mca btl vader,tcp,self`` flag? Conda-forge developers don't `compile OpenMPI with Infiniband compatibility <https://github.com/conda-forge/openmpi-feedstock/issues/38>`_.

📚 **expert** 📚 Why the ``--bind-to core:overload-allowed --map-by numa:pe=${OMP_NUM_THREADS}`` flag? This flag enables efficient hybrid MPI + OpenMP runs on NUMA architecture.

Once the work is done, type:

.. code-block:: console

    $(cocoapy38)(.local) source stop_cocoa
    $(cocoapy38) conda deactivate cocoa

