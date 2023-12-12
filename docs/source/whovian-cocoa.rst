The whovian-cocoa docker container 
==================================
  
We provide the docker image `whovian-cocoa <https://hub.docker.com/r/vivianmiranda/whovian-cocoa>`_ to facilitate the installation of Cocoa on Windows and MacOS. This appendix assumes the users already have the docker engine installed on their local PC. For instructions on installing the docker engine in specific operating systems, please refer to `Docker's official documentation <https://docs.docker.com/engine/install/>`_.

To download and run the container for the first time, type:
  
.. code-block:: bash
  
    docker run --platform linux/amd64 --hostname cocoa --name cocoa2023 -it -p 8080:8888 -v $(pwd):/home/whovian/host/ -v ~/.ssh:/home/whovian/.ssh:ro vivianmiranda/whovian-cocoa

Following the command above, users should see the following text on the screen terminal:

.. image:: https://github.com/SBU-COSMOLIKE/cocoa/assets/3210728/a5b1f93c-b47c-4f25-a4e4-aae9b7be3d66
   :width: 873

📚 **expert** 📚 The flag ``-v $(pwd):/home/whovian/host/`` ensures that files on the host computer, where the user should install Cocoa to avoid losing work in case the docker image needs to be deleted, have been mounted to the directory ``/home/whovian/host/``. Therefore, the user should see the host files of the directory where the whovian-cocoa container was initialized after typing ``cd /home/whovian/host/; ls``

When running the container the first time, the user needs to init conda with ``conda init bash`` followed by ``source ~/.bashrc``, as shown below.

.. code-block:: bash

    whovian@cocoa:~$ conda init bash
    whovian@cocoa:~$ source ~/.bashrc

The container already comes with conda Cocoa environment pre-installed:

.. code-block:: bash

    whovian@cocoa:~$ conda activate cocoa

When the user exits the container, how to restart it? Type 

.. code-block:: bash

    $ docker start -ai cocoa2023

How to run Jupyter Notebooks remotely when using Cocoa within the whovian-cocoa container? First, type the following command:

.. code-block:: bash

    whovian@cocoa:~$ jupyter notebook --no-browser --port=8080

The terminal will show a message similar to the following template:

.. code-block:: bash

    [... NotebookApp] Writing notebook server cookie secret to /home/whovian/.local/share/jupyter/runtime/notebook_cookie_secret
    [... NotebookApp] WARNING: The notebook server is listening on all IP addresses and not using encryption. This is not recommended.
    [... NotebookApp] Serving notebooks from local directory: /home/whovian/host
    [... NotebookApp] Jupyter Notebook 6.1.1 is running at:
    [... NotebookApp] http://f0a13949f6b5:8888/?token=XXX
    [... NotebookApp] or http://127.0.0.1:8888/?token=XXX
    [... NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).

Below, we assume the user runs the container in a server with the URL ``your_sever.com``. We also presume the server can be accessed via ssh protocol. From a local PC, type:

.. code-block:: bash

    $ ssh your_username@your_sever.com -L 8080:localhost:8080

Finally, go to a browser and type ``http://localhost:8080/?token=XXX``, where ``XXX`` is the previously saved token.

PS: The docker container also has the conda environment ``cocoalite`` that is useful in the rare case someone want to install Cocoa via the slow/not-advisable instructions on section TODO Installation of Cocoa's required packages via Cocoa's internal cache
