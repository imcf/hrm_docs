.. include:: global_directives.inc

.. warning:: As of version 3.2.0, the HRM expects specific file permissions on the file area! Please see :ref:`update_hrm_data_permissions` below. The deprecated fall-back mechanism of version 3.2 **no longer works in 3.3**!

***************
Version upgrade
***************

If you have upgraded the HRM in the past, you will know that some steps must be performed in addition to replacing the old HRM code with the new one: some entries might have been added or changed in the configuration files (``hrm_{server|client}_config.inc``), and the database structure might have been changed.

Stop the Queue Manager
======================

Significant parts of the configuration as well as the database are usually changed during an upgrade, so the Queue Manager needs to be stopped first.

.. code-block:: sh

    sudo /etc/init.d/hrmd stop

In some rare situations, the Queue Manager might get stuck. To ensure the *stop* command did work properly, run the following check which should return empty:

.. code-block:: sh

    ps aux | grep -i [r]unHuygens

Download and extract the new HRM release
========================================

To install the new HRM version you need to download the ``.tar.gz`` file from
the website or github as explained in :ref:`downloading the standard archive
<download-hrm-standard>`.

The downloaded package then needs to be extracted inside the HRM's installation
directory, overwriting updated files but not touching your configuration files
etc. Assuming your downloaded version is ``3.3.0`` and you were placing the
package in your home directory, this can be done like this:

.. code-block:: sh

    cd $HRM_HOME
    tar xzf $HOME/hrm-3.3.0.tar.gz --strip=1

Clean up previous installations
===============================

Sometimes files were part of a previous release of the HRM but they are not
contained in the current one any more. With the above described method those
files don't get removed as only new files are extracted from the tar package.
To avoid cluttering up the installation they should be removed according to the
versions involved.

HRM 3.3 uses new init scripts. Please delete the old files
``$HRM_BIN/hrm_user_manager``, ``$HRM_BIN/hrm_user_manager_old``,
``$HRM_BIN/hrmd`` and ``$HRM_BIN/ome_hrm`` and then follow the instructions in
:ref:`Upgrade the init script <upgrade_the_init_script>`.

.. note:: Please follow :ref:`these instructions <upgrade_clean_previous>` **first** if you are upgrading from older versions.

.. _upgrade_the_init_script:

Upgrade the init script
=======================

.. note:: If your Linux installation is using the ``systemd`` init system,
          please have a look at the :ref:`instructions about how to set up the
          HRM daemon with systemd <install_hrmd_systemd>`. Please make sure to
          remove the old init script at ``/etc/init.d/hrmd`` in this case!

          Otherwise, proceed as described here for the init script.

This step is basically identical to the initial installation of the init script
as described in :ref:`installing the daemon <hrm_daemon>`. You need to copy the
new script from ``$HRM_RESRC/sysv-init-lsb/hrmd`` to ``/etc/init.d/`` and make
sure it is executable. This can be done using the following commands:

.. code-block:: sh

    sudo cp -v $HRM_RESRC/sysv-init-lsb/hrmd /etc/init.d/hrmd
    sudo chmod +x /etc/init.d/hrmd

.. _update_hrm_data_permissions:

Update the data folder permissions
==================================

As of HRM 3.2.0, the system users running the Queue Manager and the web server
are expected to have full read-write access to ``$HRM_DATA``. The supported way
of doing this is explained in :ref:`setting up the HRM user and group
<setup_hrm_user_and_group>`. Briefly:

  * a user ``hrm`` and its corresponding group ``hrm`` are created
  * the web server user (|ubuntu| ``www-data``, |fedora| ``apache``) is added
    to the ``hrm`` group
  * the variable ``SUSER`` is set to ``hrm`` in /etc/hrm.conf
  * the ``$HRM_DATA`` and ``$HRM_LOG`` group ownership is set to ``hrm`` with
    the ``setgid`` bit set

For detailed instructions, please see :ref:`setting up the HRM user and group
<setup_hrm_user_and_group>`.

.. warning:: With **HRM 3.2** it was possible to preserve the old behavior by
             setting a configuration variable ``$change_ownership``. This is
             not supported with **HRM 3.3** any more!

Update the configuration files
==============================

As stated in the previous section, the ``$change_ownership`` option is not
supported in version **3.3** any more. If the variable is present in the
configuration files ``hrm_{server|client}_config.inc`` it will be silently
ignored. For consistency reasons it is therefore recommend to remove this
setting from the config file(s).

Group authentication
--------------------

As of HRM 3.3, external authentication via :ref:`Active Directory <activedir_auth>` or :ref:`generic LDAP <ldap_auth>` now supports **group authorization**. An additional array ``$AUTHORIZED_GROUPS`` can be set to define the set of :ref:`Active Directory <activedir_auth>` or :ref:`generic LDAP <ldap_auth>` groups that are granted access to HRM.

.. note:: Please follow :ref:`these instructions <upgrade_conf_previous>` **first** if you are upgrading from older versions.

Check the configuration files
=============================

An easy way to check for modifications is by running the ``$HRM_HOME/resources/checkConfig.php`` script. From the shell, run:

.. code-block:: sh

    cd $HRM_HOME
    php resources/checkConfig.php config/hrm_server_config.inc

Checking the 3.2.x files with the 3.3.x ``checkConfig.php`` script will result in the following output:

.. code-block:: sh

    Check against HRM v3.3.x.
    * * * Error: variable change_ownership must be removed from the configuration files!
    Check completed with errors! Please fix your configuration!

Please make sure to fix all problems. The sample files and the :ref:`manual_install` instructions will help you set the correct parameters.

.. note:: Please follow :ref:`these instructions <upgrade_check_previous>` **first** if you are upgrading from older versions.

Update the database
===================

Newer versions of the HRM might use slightly different/updated versions of the database back-end than previous ones.

+-------------+------------------+
| HRM version | Database version |
+=============+==================+
| 3.3         | 14               |
+-------------+------------------+
| 3.2         | 13               |
+-------------+------------------+
| 3.1         | 12               |
+-------------+------------------+
| 3.0.3       | 11               |
+-------------+------------------+
| 3.0         | 10               |
+-------------+------------------+
| 2.1         | 9                |
+-------------+------------------+
| 2.0         | 8                |
+-------------+------------------+
| 1.2.3       | 7                |
+-------------+------------------+

For this reason, the first time you run the HRM after an update you will be told that the database must be updated and that you are not allowed to continue until this has been done!

.. note:: Database updates are supported across HRM versions, i.e. it is possible to upgrade the database from revision 7 to 14 in one step.

The following describes two possible ways to update the database.

.. note:: Although we test this procedure quite carefully, it is **highly recommended to backup the database before updating!**

Updating from the web interface
-------------------------------

Login to the HRM as the admin user: you will be brought directly to the Database update page. Click on the update button. If everything works properly (as it should...), the following message should be displayed.

.. code-block:: sh

    Needed database revision for HRM v3.3 is number 14.
    Current database revision is number 13.
    Updating...

    Database successfully updated to revision 14.

The database is now at the latest revision.

Updating from the console
-------------------------

Alternatively, the database can be updated from the console (see :ref:`create or update database <create-database>`). Please pay attention to what the update process will report! The output should be the same as the one listed in the previous section, but if the update fails, you might want to `report it <http://hrm.svi.nl:8080/redmine/projects/hrmdev/issues/new>`_.

Re-start the Queue Manager
==========================

After processing the described upgrade steps, the Queue Manager needs to be started again.

.. code-block:: sh

    sudo /etc/init.d/hrmd start

Upgrade from previous releases
==============================

The following pages are linked to from the relevant sections above, but are listed here again for convenience.

.. toctree::
    :maxdepth: 1

    upgrade_clean_previous
    upgrade_conf_previous
    upgrade_check_previous
