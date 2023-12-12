Compiling Boltzmann, CosmoLike and Likelihood codes separately
==============================================================

To avoid excessive compilation times during development, users can use specialized scripts located at ``Cocoa/installation_scripts/`` that compile only a specific module. A few examples of these scripts are:

.. code-block:: bash

    $(cocoapy38)(.local) source ./installation_scripts/compile_class
    $(cocoapy38)(.local) source ./installation_scripts/compile_camb
    $(cocoapy38)(.local) source ./installation_scripts/compile_planck
    $(cocoapy38)(.local) source ./installation_scripts/compile_act
    $(cocoapy38)(.local) source ./installation_scripts/setup_polychord
