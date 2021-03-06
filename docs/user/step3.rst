.. include:: global_directives.inc

.. _step3:

The Restoration Parameters (3/5)
================================

In step 3 - |RestorationParameters22x22| **Restoration Parameters** - the restoration settings can be specified and saved. As in :ref:`step2` the restoration parameters are grouped in templates. A template can be reused in future deconvolution jobs. This will be useful when other images need to be restored with the same settings. Moreover, the templates can be shared among HRM users, fostering collaboration and re-usage of good restoration settings.


Notice that the following selections are possible at this stage:

* **Admin restoration templates**: parameter templates created by the HRM 
  administrator to be used as references. They can be copied and edited by 
  the HRM users. In order to use them, they first need to be copied 
  to **Your restoration templates** by using |DownArrow22x22|.
   
* **Your restoration templates**: the restoration templates the user can 
  actually use directly. All templates in this area may be selected, edited, 
  removed, etc.

* **New/clone restoration template**: entry field for the name of a new 
  parameter template. First type a name for the new template and click on
  |CreateTemplate22x22|. A new template will be created. HRM will present you
  with empty template parameters so that meaningful values can be assigned
  to them.


.. note:: Notice that |CreateTemplate22x22| |EditTemplate22x22|
          |CloneTemplate22x22| |ShareTemplate22x22| |FavouriteTemplate22x22|
          |DeleteTemplate22x22| show tooltips stating their action w.r.t
          templates. Namely, **create**, **edit**, **clone**, **share**,
          **mark as favourite** and **delete**.

A restoration parameter template consists of a number of options which
provide Huygens Core with information on the restoration procedure. 
To get a **preview** of the template contents select
the template, the contents will be displayed on the right panel.

.. note:: When editing a template help links |HelpLink22x22| are displayed at
          key locations to point you to specific articles of the SVI-wiki.

The following parameters can be edited in a template.
          
.. toctree::
   :maxdepth: 1

   step3_decon
   step3_snr
   step3_crop
   step3_background
   step3_stopcriteria
   step3_zstabilization


  
