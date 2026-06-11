.. _vsphere-import:

vSphereDB Import Source configuration
`````````````````````````````````````

The vSphereDB interface allows you to import hosts directly from a vCenter
server.

The documentation below assumes that you are already familiar with
:ref:`importing and synchronization in Director
<automating-imports>`. Before using the vSphereDB import interface,
ensure that:

* the module has been enabled under :menuselection:`Configuration / Modules / vspheredb`

* at least one Connection to a vCenter was configured in the **VMD** Module

To create a new **VMware vSphereDB** import source, go to :menuselection:`Director /
Import data sources`, click on the **Add** action, then enter a name and
description for this import source. As **Source Type**, choose the
*VMware vSphereDB* option as in :numref:`figure-vsphere-vmware-import`.

.. _figure-vsphere-vmware-import:

.. figure::  /core-modules/director/img/vmware-import01.png
   :alt: Choose Import Source name

   Choosing the VMware vSphereDB option.

As soon as you’ve chosen the correct Source Type, it will ask you for
more details, including the type of Objects you want to import and the
vCenter Connection to use for the the import, as shown in
:numref:`figure-vsphere-connection`.

.. _figure-vsphere-connection:

.. figure::  /core-modules/director/img/vmware-import03.png
   :alt: Configure the parameters for the VMware vSphereDB source

   Configuring the parameters for the VMware vSphereDB source.

That's it. Once you've confirmed that you want to add this new Import
Source, you're all done with the configuration.

.. note:: As described on :ref:`the importing page
   <property-modifiers>`, the value of the **key column name** is used
   as the ID during the matching phase.

You can now click on the **Preview** tab to see what the results look
like (See :numref:`figure-vsphere-import-preview`) before deciding
whether to run the full import.

.. _figure-vsphere-import-preview:

.. figure::  /core-modules/director/img/vmware-import04.png
   :alt: Previewing the results of importing from the source

   Previewing the results of importing from the source

Be sure to define a **Synchronization Rule** for your new import
source, as explained in the related :ref:`Director documentation
<sync-rules>`.

If you prefer to use the Icinga2 CLI commands instead of the web
interface, see :ref:`VSphere CLI reference documentation
<vsphere-cli-commands>`.
