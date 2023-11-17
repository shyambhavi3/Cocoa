Installation of Cobaya base code
================================

Assuming the user opted for the easier *Conda installation*, type:

.. code-block:: console

      $ conda activate cocoapy38

      $(cocoapy38) git clone --depth 1 https://github.com/CosmoLike/cocoa.git cocoa

      $(cocoapy38) cd ./cocoa/

to clone the repository.

⚠️ Warning ⚠️ Cocoa developers should drop the shallow clone option ``--depth 1``; they should also authenticate to GitHub via ssh keys and use the command instead.

.. code-block:: console

    $(cocoapy38) git clone git@github.com:CosmoLike/cocoa.git cocoa

    $(cocoapy38) cd ./cocoa/

⚠️ Warning ⚠️ We have a limited monthly quota in bandwidth for Git LFS files, and therefore we ask users to use good judgment in the number of times they clone files from Cocoa's main repository.

Cocoa is made aware of the chosen installation method of required packages via special environment keys located on the *Cocoa/set_installation_options* script, as shown below:

.. code-block:: console

    [Extracted from set_installation_options script]
    # --------------------------------------------------------------------------------------
    # --------------------------------------------------------------------------------------
    # --------------------------------------------------------------------------------------
    # ----------------------- HOW COCOA SHOULD BE INSTALLED? -------------------------------
    # --------------------------------------------------------------------------------------
    # --------------------------------------------------------------------------------------
    # --------------------------------------------------------------------------------------
    export MINICONDA_INSTALLATION=1
    #export MANUAL_INSTALLATION=1

The user must uncomment the appropriate key (here, we assume ``MINICONDA_INSTALLATION`` which is the default option), and then type the following command

.. code-block:: console

    $(cocoapy38) cd ./Cocoa/
    $(cocoapy38) source setup_cocoa_installation_packages

The script ``setup_cocoa_installation_packages`` decompresses the data files, which only takes a few minutes, and installs any remaining necessary packages. Typical package installation time ranges, depending on the installation method, from a few minutes (installation via Conda) to ~1/2 hour (installation via Cocoa's internal cache). It is important to note that our scripts never install packages on ``$HOME/.local``. All requirements for Cocoa are installed at

.. code-block:: console

    Cocoa/.local/bin
    Cocoa/.local/include
    Cocoa/.local/lib
    Cocoa/.local/share

This behavior is critical to enable users to work on multiple instances of Cocoa simultaneously.

Finally, type

.. code-block:: console

    $(cocoapy38) source compile_external_modules

to compile CAMB, CLASS, Planck and Polychord. If the user wants to compile only a subset of these packages, then refer to the appendix Compiling Boltzmann, CosmoLike and Likelihood codes separately TODO.

📚 **expert** 📚 The script *set_installation_options script* contains a few additional flags that may be useful if something goes wrong. They are shown below:

.. code-block:: console

    [Extracted from set_installation_options script]
    # --------------------------------------------------------------------------------------
    # --------- IF TRUE, THEN COCOA USES CLIK FROM https://github.com/benabed/clik ---------
    # --------------------------------------------------------------------------------------
    export USE_SPT_CLIK_PLANCK=1

    # --------------------------------------------------------------------------------------
    # ----------------- CONTROL OVER THE COMPILATION OF EXTERNAL CODES ---------------------
    # --------------------------------------------------------------------------------------
    #export IGNORE_CAMB_COMPILATION=1
    export IGNORE_CLASS_COMPILATION=1
    #export IGNORE_COSMOLIKE_COMPILATION=1
    #export IGNORE_POLYCHORD_COMPILATION=1
    #export IGNORE_PLANCK_COMPILATION=1
    #export IGNORE_ACT_COMPILATION=1

    # --------------------------------------------------------------------------------------
    # IN CASE COMPILATION FAILS, THESE FLAGS WILL BE USEFUL. BY DEFAULT, THE COMPILATION'S -
    # OUTPUT IS NOT WRITTEN ON THE TERMINAL. THESE FLAGS ENABLE THAT OUTPUT ---------------- 
    # --------------------------------------------------------------------------------------
    #export DEBUG_PLANCK_OUTPUT=1
    #export DEBUG_CAMB_OUTPUT=1
    #export DEBUG_CLASS_OUTPUT=1
    #export DEBUG_POLY_OUTPUT=1
    #export DEBUG_ACT_OUTPUT=1




