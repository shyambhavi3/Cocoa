Running Cosmolike projects
==========================

The *projects* folder was designed to include Cosmolike projects. Similar to the previous section, we assume the user opted for the more direct *Conda installation* method. We also presume the user's terminal is in the folder where Cocoa was cloned.

**Step 1 of 5**: activate the Conda Cocoa environment

.. code-block:: console

    $ conda activate cocoapy38

**Step 2 of 5**: go to the project folder (``./cocoa/Cocoa/projects``) and clone a Cosmolike project, with fictitious name ``XXX``:

.. code-block:: console

    $(cocoapy38) cd ./cocoa/Cocoa/projects
    $(cocoapy38) $CONDA_PREFIX/bin/git clone git@github.com:CosmoLike/cocoa_XXX.git XXX

By convention, the Cosmolike Organization hosts a Cobaya-Cosmolike project named XXX at ``CosmoLike/cocoa_XXX``. However, our provided scripts and template YAML files assume the removal of the ``cocoa_`` prefix when cloning the repository.

Example of cosmolike projects: `lsst_y1 <https://github.com/CosmoLike/cocoa_lsst_y1>`_.

**Step 3 of 5**: go back to Cocoa main folder, and activate the private python environment

.. code-block:: console

    $(cocoapy38) cd ../
    $(cocoapy38) source start_cocoa

⚠️ (**warning**) ⚠️ Remember to run the start_cocoa script only after cloning the project repository. The script *start_cocoa* creates the necessary symbolic links and adds the *Cobaya-Cosmolike interface* of all projects to ``LD_LIBRARY_PATH`` and ``PYTHONPATH`` paths.

**Step 4 of 5**: compile the project

.. code-block:: console

    $(cocoapy38)(.local) source ./projects/XXX/scripts/compile_XXX

**Step 5 of 5**: select the number of OpenMP cores and run a template YAML file

.. code-block:: console

    $(cocoapy38)(.local) export OMP_PROC_BIND=close; export OMP_NUM_THREADS=4
    $(cocoapy38)(.local) mpirun -n 1 --oversubscribe --mca btl vader,tcp,self --bind-to core --rank-by core --map-by numa:pe=${OMP_NUM_THREADS} cobaya-run ./projects/XXX/EXAMPLE_EVALUATE1.yaml -f

⚠️ **Warning** ⚠️ Be careful when creating YAML for weak lensing projects in Cobaya using the |Ωm|/|Ωn|
parameterization. See Appendix warning about weak lensing YAML files TODO for further details.

.. |Ωm| replace:: Ω\ :sub:`m`\ 
.. |Ωn| replace:: Ω\ :sub:`n`\ 
