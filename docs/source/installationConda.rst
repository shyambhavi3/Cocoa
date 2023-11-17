Installation of Cocoa's required packages via Conda
===================================================

There are two installation methods. Users must choose one of them:

#. Via Conda - easier, best overall.
#. Via Cocoa's internal cache - slow, not advisable. See Appendix Installation of Cocoa's required packages via Cocoa's internal cache TODO

We also provide the docker image whovian-cocoa to facilitate the installation of Cocoa on Windows and MacOS. For further instructions, refer to the Appendix whovian-cocoa docker container. TODO

We assume here the user has previously installed either `Minicoda <https://docs.conda.io/projects/miniconda/en/latest/>`_ or `Anaconda <https://www.anaconda.com/download>`_. If this is not the case, refer to the Appendix Miniconda Installation TODO for further instructions.

Type the following commands to create the cocoa Conda environment. 

.. code-block:: console

   conda create --name cocoapy38 python=3.8 --quiet --yes \
      && conda install -n cocoapy38 --quiet --yes  \
       'conda-forge::libgcc-ng=12.3.0' \
       'conda-forge::libstdcxx-ng=12.3.0' \
       'conda-forge::libgfortran-ng=12.3.0' \
       'conda-forge::gxx_linux-64=12.3.0' \
       'conda-forge::gcc_linux-64=12.3.0' \
       'conda-forge::gfortran_linux-64=12.3.0' \
       'conda-forge::openmpi=4.1.5' \
       'conda-forge::sysroot_linux-64=2.17' \
       'conda-forge::git=2.40.0' \
       'conda-forge::git-lfs=3.3.0' \
       'conda-forge::fftw=3.3.10' \
       'conda-forge::cfitsio=4.0.0' \
       'conda-forge::hdf5=1.14.0' \
       'conda-forge::lapack=3.9.0' \
       'conda-forge::openblas=0.3.23' \
       'conda-forge::lapack=3.9.0' \
       'conda-forge::gsl=2.7' \
       'conda-forge::cmake=3.26.4' \
       'conda-forge::xz==5.2.6' \
       'conda-forge::armadillo=11.4.4' \
       'conda-forge::boost-cpp=1.81.0' \
       'conda-forge::expat=2.5.0' \
       'conda-forge::cython=0.29.35' \
       'conda-forge::scipy=1.10.1' \
       'conda-forge::pandas=1.5.3' \
       'conda-forge::numpy=1.23.5' \
       'conda-forge::matplotlib=3.7.1' \
       'conda-forge::mpi4py=3.1.4' \
       'conda-forge::pip=23.1.2'

For those working on projects that utilize machine-learning-based emulators, the Appendix Setting-up conda environment for Machine Learning emulators TODO provides additional commands for installing the necessary packages.

When adopting this installation method, users must activate the Conda environment whenever working with Cocoa, as shown below.

.. code-block:: console

      $ conda activate cocoapy38

Furthermore, users must install GIT-LFS on the first loading of the Conda cocoa environment.

.. code-block:: console
      
      $(cocoapy38) git-lfs install

Users can now proceed to the section Installation of Cobaya base code. TODO


