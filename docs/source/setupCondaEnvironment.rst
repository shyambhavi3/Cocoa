Setting-up conda environment for Machine Learning emulators
===========================================================
  
If the user wants to add Tensorflow, Keras and Pytorch for an emulator-based project via Conda, then type

.. code-block:: bash

    $ conda activate cocoapy38 
  
    $(cocoapy38) $CONDA_PREFIX/bin/pip install --no-cache-dir \
        'tensorflow-cpu==2.12.0' \
        'keras==2.12.0' \
        'keras-preprocessing==1.1.2' \
        'torch==1.13.1+cpu' \
        'torchvision==0.14.1+cpu' \
        'torchaudio==0.13.1' --extra-index-url https://download.pytorch.org/whl/cpu

In case there are GPUs available, the following commands will install the GPU version of Tensorflow, Keras and Pytorch (assuming CUDA 11.6, click `here <https://pytorch.org/get-started/previous-versions/>`_ for additional information).
                                                                                                            
.. code-block:: bash
                                                                                                      
    $(cocoapy38) $CONDA_PREFIX/bin/pip install --no-cache-dir \
        'tensorflow==2.12.0' \
        'keras==2.12.0' \
        'keras-preprocessing==1.1.2' \
        'torch==1.13.1+cu116' \
        'torchvision==0.14.1+cu116' \
        'torchaudio==0.13.1' --extra-index-url https://download.pytorch.org/whl/cu116
                                                                                                                        
Based on our experience, we recommend utilizing the GPU versions to train the emulator while using the CPU versions to run the MCMCs. This is because our supercomputers possess a greater number of CPU-only nodes. It may be helpful to create two separate conda environments for this purpose. One could be named ``cocoa`` (CPU-only), while the other could be named ``cocoaemu`` and contain the GPU versions of the machine learning packages.

Commenting out the environmental flags shown below, located at *set_installation_options* script, will enable the installation of machine-learning-related libraries via pip.

 .. code-block:: bash                                                                                              
                                                                                                                   # IF TRUE, THEN COCOA WON'T INSTALL TENSORFLOW, KERAS and PYTORCH
    #export IGNORE_EMULATOR_CPU_PIP_PACKAGES=1
    #export IGNORE_EMULATOR_GPU_PIP_PACKAGES=1
                                                                                                                        
Unlike most installed pip prerequisites, which are cached at ``cocoa_installation_libraries/pip_cache.xz``, the installation of the Machine Learning packages listed above requires an active internet connection.
