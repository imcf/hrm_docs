.. include:: global_directives.inc

.. toctree::
   :maxdepth: 1

.. _`download-hrm`:


***********
Get the HRM
***********

Current version
===============

Current version of the HRM is |HRM_VERSION|.

Download installation script
============================

The script downloads, installs and sets up the HRM and its requested dependences while allowing for a certain degree of configurability. Please follow the :ref:`script-install` instructions.

|ubuntu| |fedora| `Download the installation script <https://github.com/aarpon/hrm_installer/releases>`_.

Download standard archive
=========================

.. _download-hrm-standard:

The HRM archive is the standard installation approach: you have full control over the installation procedure, but you'll have to do all the work yourself. Just unpack the code and install it following the :ref:`manual_install` instructions.

.. note::

    This is the only way to install the HRM on platforms not supported by the installation script or to tweak the set up beyond the set of options provided by it.

|linux| `Download the archive <https://sourceforge.net/projects/hrm/files/latest/download>`_.

Clone git repository
====================

|github| If you want to follow the HRM development, you can also `clone <https://github.com/aarpon/hrm>`_ the read-only HRM repository from github.

.. warning::

    Please keep in mind that the latest development snapshot is **unstable software and should not be deployed on production machines!**
