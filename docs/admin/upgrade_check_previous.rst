.. include:: global_directives.inc

.. toctree::
   :maxdepth: 1

.. _upgrade_check_previous:

*************************************************
Check the configuration files (previous versions)
*************************************************

.. warning:: These instructions refer to older versions of HRM!

An easy way to check for modifications is by running the ``$HRM_HOME/resources/checkConfig.php`` script. From the shell, run:

.. code-block:: sh

    cd $HRM_HOME
    php resources/checkConfig.php config/hrm_server_config.inc

3.1.x to 3.2.x
--------------

Checking the 3.1.x files with the 3.2.x ``checkConfig.php`` script will result in the following output:

.. code-block:: sh

    Check against HRM v3.2.x.
    Check completed successfully! Your configuration file is valid!

3.0.x to 3.1.x
--------------

Checking the 3.0.x files with the 3.1.x ``checkConfig.php`` script will result in the following output:

.. code-block:: sh

    Check against HRM v3.1.x.
    Check completed successfully! Your configuration file is valid!

2.1.x to 3.0.x
--------------

Checking the 2.1.x files with the 3.0 ``checkConfig.php`` script will result in the following output:

.. code-block:: sh

    Check against HRM v3.0.x.
    * * * Error: variable omero_transfers not set or empty.
    Check completed with errors! Please fix your configuration!


1.2.x to 2.1.x
--------------

Checking the 1.2.x files with the 2.1.x ``checkConfig.php`` script will result in the following output:

.. code-block:: sh

    Check against HRM v2.1.x.
    * * * Error: variable max_upload_limit not set or empty.
    * * * Error: variable max_post_limit not set or empty.
    * * * Error: variable email_list_separator not set or empty. 
    * * * Error: variable resultImagesOwnedByUser must be removed from the configuration files!
    * * * Error: variable resultImagesRenamed must be removed from the configuration files! 
    * * * Error: variable enable_code_for_huygens must be removed from the configuration files! 
    Check completed with errors! Please fix your configuration!

Please make sure to fix all problems. The sample files and the :ref:`manual_install` instructions will help you set the correct parameters.