Miniconda Installation
======================

Download and run Miniconda installation script (please adapt ``CONDA_DIR``):

.. code-block:: bash

    export CONDA_DIR=/gpfs/home/vinmirandabr/miniconda

    mkdir $CONDA_DIR

    wget https://repo.continuum.io/miniconda/Miniconda3-py38_4.12.0-Linux-x86_64.sh

    /bin/bash Miniconda3-py38_4.12.0-Linux-x86_64.sh -f -b -p $CONDA_DIR

After installation, users must source conda configuration file

.. code-block:: bash

    source $CONDA_DIR/etc/profile.d/conda.sh \
        && conda config --set auto_update_conda false \
        && conda config --set show_channel_urls true \
        && conda config --set auto_activate_base false \
        && conda config --prepend channels conda-forge \
        && conda config --set channel_priority strict \
        && conda init bash
