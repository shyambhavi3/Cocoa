Installation of Cocoa's required packages via Cocoa's internal cache
====================================================================

This method is slow and not advisable :stop_sign::thumbsdown:. When Conda is unavailable, the user can still perform a local semi-autonomous installation on Linux based on a few scripts we implemented. We provide a local copy of almost all required packages in Cocoa's cache folder named *cocoa_installation_libraries*. Pre-installation of the following packages is assumed:

- `Bash <https://www.amazon.com/dp/B0043GXMSY/ref=cm_sw_em_r_mt_dp_x3UoFbDXSXRBT>`__
- `Git <https://git-scm.com>`__ v1.8+
- `Git LFS <https://git-lfs.github.com>`__
- `gcc <https://gcc.gnu.org>`__ v12.*
- `gfortran <https://gcc.gnu.org>`__ v12.*
- `g++ <https://gcc.gnu.org>`__ v12.*
- `Python <https://www.python.org>`__ v3.8.*
- `PIP package manager <https://pip.pypa.io/en/stable/installing/>`__
- `Python Virtual Environment <https://www.geeksforgeeks.org/python-virtual-environment/>`__

The conda environment ``cocoalitepy38`` contains the minimum packages necessary for this installation method

.. code-block:: bash

    conda create --name cocoalitepy38 python=3.8 --quiet --yes \
    && conda install -n cocoalitepy38 --quiet --yes  \
        'conda-forge::libgcc-ng=12.3.0' \
        'conda-forge::libstdcxx-ng=12.3.0' \
        'conda-forge::libgfortran-ng=12.3.0' \
        'conda-forge::gxx_linux-64=12.3.0' \
        'conda-forge::gcc_linux-64=12.3.0' \
        'conda-forge::gfortran_linux-64=12.3.0' \
        'conda-forge::openmpi=4.1.5' \
        'conda-forge::sysroot_linux-64=2.17' \
        'conda-forge::git=2.40.0' \
        'conda-forge::git-lfs=3.3.0'

To perform the local semi-autonomous installation, users must modify flags written on the file *set_installation_options* because the default behavior corresponds to an installation via Conda. First, select the environmental key ``MANUAL_INSTALLATION`` as shown below:

.. code-block:: bash

  [Extracted from set_installation_options script] 

  # --------------------------------------------------------------------------------------
  # --------------------------------------------------------------------------------------
  # --------------------------------------------------------------------------------------
  # ----------------------- HOW COCOA SHOULD BE INSTALLED? -------------------------------
  # --------------------------------------------------------------------------------------
  # --------------------------------------------------------------------------------------
  # --------------------------------------------------------------------------------------
  #export MINICONDA_INSTALLATION=1
  export MANUAL_INSTALLATION=1

Finally, set the following environmental keys

.. code-block:: bash

  [Extracted from set_installation_options script]
  
  if [ -n "${MANUAL_INSTALLATION}" ]; then
      # --------------------------------------------------------------------------------------
      # IF SET, COCOA DOES NOT USE SYSTEM PIP PACKAGES (RELIES EXCLUSIVELY ON PIP CACHE FOLDER)
      # --------------------------------------------------------------------------------------
      export DONT_USE_SYSTEM_PIP_PACKAGES=1
  
      # --------------------------------------------------------------------------------------
      # IF SET, COCOA WILL NOT INSTALL TENSORFLOW, KERAS, PYTORCH, GPY
      # --------------------------------------------------------------------------------------
      export IGNORE_EMULATOR_CPU_PIP_PACKAGES=1
      export IGNORE_EMULATOR_GPU_PIP_PACKAGES=1
  
      # --------------------------------------------------------------------------------------
      # WE USE CONDA COLASLIM ENV WITH JUST PYTHON AND GCC TO TEST MANUAL INSTALLATION
      # --------------------------------------------------------------------------------------
      #conda create --name cocoalite python=3.8 --quiet --yes \
      #   && conda install -n cocoapy38 --quiet --yes  \
      #   'conda-forge::libgcc-ng=12.3.0' \
      #   'conda-forge::libstdcxx-ng=12.3.0' \
      #   'conda-forge::libgfortran-ng=12.3.0' \
      #   'conda-forge::gxx_linux-64=12.3.0' \
      #   'conda-forge::gcc_linux-64=12.3.0' \
      #   'conda-forge::gfortran_linux-64=12.3.0' \
      #   'conda-forge::openmpi=4.1.5' \
      #   'conda-forge::sysroot_linux-64=2.17' \
      #   'conda-forge::git=2.40.0' \
      #   'conda-forge::git-lfs=3.3.0'
      # --------------------------------------------------------------------------------------
  
      export GLOBAL_PACKAGES_LOCATION=$CONDA_PREFIX
      export GLOBALPYTHON3=$CONDA_PREFIX/bin/python${PYTHON_VERSION}
      export PYTHON_VERSION=3.8
  
      # --------------------------------------------------------------------------------------
      # COMPILER
      # --------------------------------------------------------------------------------------
      export C_COMPILER=$CONDA_PREFIX/bin/x86_64-conda-linux-gnu-cc
      export CXX_COMPILER=$CONDA_PREFIX/bin/x86_64-conda-linux-gnu-g++
      export FORTRAN_COMPILER=$CONDA_PREFIX/bin/x86_64-conda-linux-gnu-gfortran
      export MPI_CC_COMPILER=$CONDA_PREFIX/bin/mpicxx
      export MPI_CXX_COMPILER=$CONDA_PREFIX/bin/mpicc
      export MPI_FORTRAN_COMPILER=$CONDA_PREFIX/bin/mpif90
  
      # --------------------------------------------------------------------------------------
      # USER NEEDS TO SPECIFY THE FLAGS BELOW SO COCOA CAN FIND PYTHON / GCC
      # --------------------------------------------------------------------------------------
      export PATH=$CONDA_PREFIX/bin:$PATH
  
      export CFLAGS="${CFLAGS} -I$CONDA_PREFIX/include"
  
      export LDFLAGS="${LDFLAGS} -L$CONDA_PREFIX/lib"
  
      export C_INCLUDE_PATH=$CONDA_PREFIX/include:$C_INCLUDE_PATH
      export C_INCLUDE_PATH=$CONDA_PREFIX/include/python${PYTHON_VERSION}m/:$C_INCLUDE_PATH
  
      export CPLUS_INCLUDE_PATH=$CONDA_PREFIX/include:$CPLUS_INCLUDE_PATH
      export CPLUS_INCLUDE_PATH=$CONDA_PREFIX/include/python${PYTHON_VERSION}m/:$CPLUS_INCLUDE_PATH
  
      export PYTHONPATH=$CONDA_PREFIX/lib/python$PYTHON_VERSION/site-packages:$PYTHONPATH
      export PYTHONPATH=$CONDA_PREFIX/lib:$PYTHONPATH
  
      export LD_RUN_PATH=$CONDA_PREFIX/lib/python$PYTHON_VERSION/site-packages:$LD_RUN_PATH
      export LD_RUN_PATH=$CONDA_PREFIX/lib:$LD_RUN_PATH
  
      export LIBRARY_PATH=$CONDA_PREFIX/lib/python$PYTHON_VERSION/site-packages:$LIBRARY_PATH
      export LIBRARY_PATH=$CONDA_PREFIX/lib:$LIBRARY_PATH
  
      export CMAKE_INCLUDE_PATH=$CONDA_PREFIX/include/:$CMAKE_INCLUDE_PATH
      export CMAKE_INCLUDE_PATH=$CONDA_PREFIX/include/python${PYTHON_VERSION}m/:$CMAKE_INCLUDE_PATH    
  
      export CMAKE_LIBRARY_PATH=$CONDA_PREFIX/lib/python$PYTHON_VERSION/site-packages:$CMAKE_LIBRARY_PATH
      export CMAKE_LIBRARY_PATH=$CONDA_PREFIX/lib:$CMAKE_LIBRARY_PATH
  
      export INCLUDE_PATH=$CONDA_PREFIX/include/:$INCLUDE_PATH
  
      export INCLUDEPATH=$CONDA_PREFIX/include/:$INCLUDEPATH
  
      export INCLUDE=$CONDA_PREFIX/x86_64-conda-linux-gnu/include:$INCLUDE
      export INCLUDE=$CONDA_PREFIX/include/:$INCLUDE
  
      export CPATH=$CONDA_PREFIX/include/:$CPATH
  
      export OBJC_INCLUDE_PATH=$CONDA_PREFIX/include/:OBJC_INCLUDE_PATH
  
      export OBJC_PATH=$CONDA_PREFIX/include/:OBJC_PATH
  
      # --------------------------------------------------------------------------------------
      # DEBUG THE COMPILATION OF PREREQUISITES PACKAGES. BY DEFAULT, THE COMPILATION'S -------
      # OUTPUT IS NOT WRITTEN ON THE TERMINAL. THESE FLAGS ENABLE THAT OUTPUT ---------------- 
      # --------------------------------------------------------------------------------------
      #export DEBUG_CPP_PACKAGES=1
      #export DEBUG_C_PACKAGES=1
      #export DEBUG_FORTRAN_PACKAGES=1
      #export DEBUG_PIP_OUTPUT=1
      #export DEBUG_XZ_PACKAGE=1
      #export DEBUG_CMAKE_PACKAGE=1
      #export DEBUG_OPENBLAS_PACKAGE=1
      #export DEBUG_DISTUTILS_PACKAGE=1
      #export DEBUG_HDF5_PACKAGES=1

The fine-tunning over the use of system-wide packages instead of our local copies can be set via the environmental flags

.. code-block::

    export IGNORE_XZ_INSTALLATION=1
    export IGNORE_HDF5_INSTALLATION=1
    export IGNORE_CMAKE_INSTALLATION=1
    export IGNORE_DISTUTILS_INSTALLATION=1
    export IGNORE_C_GSL_INSTALLATION=1
    export IGNORE_C_CFITSIO_INSTALLATION=1
    export IGNORE_C_FFTW_INSTALLATION=1
    export IGNORE_CPP_BOOST_INSTALLATION=1 
    export IGNORE_OPENBLAS_INSTALLATION=1
    export IGNORE_FORTRAN_LAPACK_INSTALLATION=1
    export IGNORE_CPP_ARMA_INSTALLATION=1

Users can now proceed to the section TODO Installation of Cobaya base code
