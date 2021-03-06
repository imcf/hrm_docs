.. include:: global_directives.inc

.. toctree::
   :maxdepth: 1

*************************
Install the prerequisites
*************************

The HRM requires a few prerequisites for its functions.

Operating system
================

The HRM should work on any recent Linux distribution, but we only support |ubuntu| **Ubuntu** (with its derivatives) and |fedora| **Fedora** (that will apply to **RHEL** and **CentOS**). Distribution-specific differences are marked in the following with the corresponding distribution logos.

.. warning::

    Please notice that with release 3.1, we dropped support for |macosx| **Mac OS X**. HRM 3.0 is still known to work on Mac OS X from 10.5 (Leopard) onward, but no effort will be made to make future versions of the HRM compatible with Mac OS X. Also notice that the HRM was never tested on Mavericks. For reference, see `the old documentation <http://huygens-rm.org/home/?q=node/16>`_.

Huygens Core
============

The HRM is an interface to Scientific Volume Imaging's `Huygens Core <http://www.svi.nl/HuygensCore>`_. **Huygens Core** is is a fully scriptable compute engine intended to run image processing and deconvolution jobs on large 64 bit multiprocessor servers in headless mode, i.e. without a specific graphical interface. The HRM provides such an interface for multi-user, batch access to Huygens Core.

.. note::

    If the web and the processing server are not on the same machine, you will need an additional Huygens Core for the web server with a **reader license** (free of charge).

Apache2 web server
==================

|ubuntu|

.. code-block:: sh

    sudo apt-get install apache2

|fedora|

.. code-block:: sh

    sudo yum install httpd

Web pages can be installed globally or per-user.

|ubuntu| The Apache2 global document root is ``/var/www``, or ``/var/www/html`` in more recent versions (14.04 LTS and newer). 

|fedora| The Apache2 global document root is ``/var/www/html``.

If you plan to install the HRM in a specific user directory, use ``/home/<hrm_user>/public_html``.

Apache2 access handling
-----------------------

HRM uses ``.htaccess`` files to prevent access to configuration files. Make sure to set the ``AllowOverride`` directive in ``/etc/apache2/sites-available/default`` |ubuntu| resp. ``FIXME`` |fedora| to enable ``.htaccess`` files in the HRM on the web server (``AllowOverride All``), or at least make sure to prevent access to the subdirectories ``inc``, ``config``, ``run``, ``resources`` and ``setup``.

If you are installing the HRM in your user dir, make sure to change ``AllowOverride`` to ``All`` in ``/etc/apache2/mods-available/userdir.conf`` |ubuntu| resp. ``FIXME`` |fedora| (make sure to enable the userdir mod first by running ``sudo a2enmod userdir`` in the shell).

|ubuntu| See also `Enabling use of Apache htaccess files <https://help.ubuntu.com/community/EnablingUseOfApacheHtaccessFiles>`_.

PHP |ge| 5.3
============

The HRM is made of two parts, a web interface and a queue manager, both written in PHP but with different requirements. The web interface requires the PHP 5 module for Apache2, the queue manager requires the PHP 5 command line interpreter.

.. note::

    Minimum required PHP version is **5.3**.

|ubuntu|

.. code-block:: sh

    sudo apt-get install libapache2-mod-php5 php5 php5-cli php5-common php5-json

.. note::

    JSON support for PHP was moved into a separate package ``php5-json`` in Ubuntu 14.04LTS; in older versions, JSON support is part of the core ``php5`` package.

|fedora|

.. code-block:: sh

    sudo yum install php php-cli php-common php-process php-json 

Production php.ini settings
---------------------------

Please configure the HRM machine for production. Edit the ``php.ini`` configuration file (|ubuntu| ``/etc/php5/apache2/php.ini``, |fedora| ``/etc/php.ini``) and set at least the values below (more information can be found in the ``php.ini`` file itself).

.. code-block:: sh

    display_errors = Off
    display_startup_errors = Off
    error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT

A relational database
=====================

The HRM officially supports two relational databases: **MySQL** and **PostgreSQL**.

MySQL
-----

|ubuntu|

.. code-block:: sh

    sudo apt-get install php5-mysql mysql-server

|fedora|

.. code-block:: sh

    sudo yum install php-mysql php-pdo mysql-server

.. note::

    It is recommended to install a database management tool like ``phpmyadmin``.

PostgreSQL
----------

|ubuntu|

.. code-block:: sh

    sudo apt-get install php5-pgsql postgresql

|fedora|

.. code-block:: sh

    sudo yum install php-pgsql postgresql-server postgresql-contrib

You will need to manually enable PostgreSQL:

.. code-block:: sh

    sudo systemctl enable postgresql

.. note::

    It is recommended to install a database manageent tool like ``phppgadmin``.

Some additional information:

    * `This <https://help.ubuntu.com/community/PostgreSQL/>`_ is a good tutorial from the Ubuntu Community on how to set up PostgreSQL to use with the HRM.

(Optional) LDAP support
=======================

If you plan to configure the HRM to use either :ref:`activedir_auth` or :ref:`ldap_auth`, you will need to install the php-ldap package as well:

|ubuntu|

.. code-block:: sh

    sudo apt-get install php5-ldap

|fedora|

.. code-block:: sh

    sudo yum install php-ldap 

Sendmail (postfix)
==================

HRM uses the PHP ``mail()`` function to notify the users: 

    "For the Mail functions to be available, PHP must have access to the sendmail binary on your system during compile time. If you use another mail program, such as qmail or postfix, be sure to use the appropriate sendmail wrappers that come with them." `More... <http://www.php.net/mail>`_

|ubuntu|

.. code-block:: sh

    sudo apt-get install postfix

|fedora|

.. code-block:: sh

    sudo yum install postfix

.. note::

    If your mail server is set up correctly and still PHP cannot send e-mails, SELinux might be blocking it. Query the status of ``httpd_can_sendmail`` as follows:

    ``$ /usr/sbin/getsebool httpd_can_sendmail``

    If ``httpd_can_sendmail`` is ``off``, you can enable it with:

    ``$ /usr/sbin/setsebool -P httpd_can_sendmail on``


PHP `date()` and default timezone
---------------------------------

Please make sure to set the default timezone in `php.ini` as follows:

.. code-block:: sh

    [Date]
    ; Defines the default timezone used by the date functions
    ; http://php.net/date.timezone
    date.timezone = "Europe/Zurich"

Otherwise you will get the following warning every time the PHP function `date()` is called within the HRM:

::

    PHP Warning: date(): It is not safe to rely on the system's timezone settings. You are
    required to use the date.timezone setting or the date_default_timezone_set() function. (...)

Click here for the `full list of supported time zones <http://us2.php.net/manual/en/timezones.php>`_.

SSH
===

If the queue manager and the image processing server are not on the same machine (see installation instructions), HRM transfers files via ssh between the two using ``sudo``. To allow HRM to login and run commands as sudo via remote, it is necessary to comment out the line ``'Defaults requiretty'`` in the ``/etc/sudoers`` file.

Compressors
===========

The HRM compresses files to be downloaded (such as deconvolution results). Several options are possible (and more can be added in the configuration files), but by default the HRM uses ``zip``.

|ubuntu|

.. code-block:: sh

    sudo apt-get install zip

|fedora|

.. code-block:: sh

    sudo yum install zip


.. _prerequisites-omero:

(Optional) OMERO support
========================

If you plan to use the :ref:`connector_omero`, you will need to download the
"server" package from the OMERO website that **matches** your existing OMERO
installation **and** the Ice version installed on your HRM system. To make it
work on your system, you also need Python 2.6 or 2.7 and (which is installed by
default on Fedora and Ubuntu) and Java 7.

.. warning::

    The connector requires at least the **5.0 series** of OMERO, the latest
    version is only tested against **OMERO 5.1** and newer!
  
.. note::

    It is **NOT** required to do any *installation* or *configuration* of the
    downloaded OMERO package! HRM just needs this package to communicate with
    your existing OMERO server. Thus, this package allows HRM to communicate
    with any OMERO server connected to the network.

Required OMERO Connector Components
-----------------------------------

This section explains the steps required if you want to use the optional
:ref:`connector_omero`.

As an example on how to download these packages, the commands used to fetch
``OMERO 5.0.3`` and ``Ice 3.4`` are shown below. For other combinations please
have a look at the `OMERO download site
<http://downloads.openmicroscopy.org/omero/>`_. We recommend placing the
downloaded OMERO "server" package into a subdirectory of ``/opt/OMERO``, as
follows:


|ubuntu|

.. code-block:: sh

    sudo apt-get install python-zeroc-ice libicessl34 openjdk-7-jre
    wget http://downloads.openmicroscopy.org/omero/5.0.3/artifacts/OMERO.server-5.0.3-ice34-b41.zip -O /tmp/OMERO.server.zip
    sudo mkdir -pv /opt/OMERO
    cd /opt/OMERO
    sudo unzip /tmp/OMERO.server.zip
    rm /tmp/OMERO.server.zip

|fedora|

.. code-block:: sh

    sudo yum install ice-python java-1.7.0-openjdk
    wget http://downloads.openmicroscopy.org/omero/5.0.3/artifacts/OMERO.server-5.0.3-ice34-b41.zip -O /tmp/OMERO.server.zip
    sudo mkdir -pv /opt/OMERO
    cd /opt/OMERO
    sudo unzip /tmp/OMERO.server.zip
    rm /tmp/OMERO.server.zip

.. note::

    From HRM 3.3.0 on, the OMERO connector requires the Python package
    ``argparse``, which is included by default from Python 2.7 on. In case you
    are running an older Python version, you have to install the package
    yourself. Please refer to the installation instructions of your
    distribution on how to set up Python argparse.

    Please follow the instructions in :ref:`configuring the OMERO connector
    <connector_omero>` about the optional configuration variable
    ``PYTHON_EXTLIB`` in case you have to place the package in a non-default
    location.

    This configuration variable also applies to the following section!

.. _prerequisites-omero-optional:

Additional Python Packages (Optional)
-------------------------------------

This section is about packages that are not mandatory for using the connector,
but add more functionality to it.

If the `Python Imaging Library (PIL) <https://pypi.python.org/pypi/PIL>`_ or
`Pillow <http://python-pillow.org/>`_ is installed, the connector will download
thumbnails as well to use them as previews in the HRM side bar. Those "simple"
previews can of course be replaced with the HRM ones by clicking on the
"Re-generate Preview" link.

In case the `Beautiful Soup <https://pypi.python.org/pypi/beautifulsoup4>`_
library is available on the system, the connector will generate parameter
summaries and attach them as a human-readable comment to any image uploaded to
OMERO.

|ubuntu|

.. code-block:: sh

    sudo apt-get install python-imaging python-bs4

|fedora|

.. code-block:: sh

    sudo yum install python-imaging python-beautifulsoup4
