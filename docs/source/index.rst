Overview of the Cobaya-CosmoLike Joint Architecture (Cocoa)
===================================

**Lumache** (/lu'make/) is a Python library for cooks and food lovers
that creates recipes mixing random ingredients.
It pulls data from the `Open Food Facts database <https://world.openfoodfacts.org/>`_
and offers a *simple* and *intuitive* API.

Check out the :doc:`usage` section for further information, including
how to :ref:`installation` the project.

Cocoa allows users to run `CosmoLike <https://github.com/CosmoLike>` routines inside the Cobaya framework. 
CosmoLike can analyze data primarily from the Dark Energy Survey and simulate 
future multi-probe analyses for LSST and Roman Space Telescope. Besides 
integrating Cobaya and CosmoLike, Cocoa introduces shell scripts and readme 
instructions that allow users to containerize Cobaya. The container structure 
ensures that users will adopt the same compiler and libraries (including their 
versions), and that they will be able to use multiple Cobaya instances consistently. 
This readme file presents basic and advanced instructions for installing all Cobaya 
and CosmoLike components.

.. note::

   This project is under active development.

Contents
--------

.. toctree::

   usage
   api
