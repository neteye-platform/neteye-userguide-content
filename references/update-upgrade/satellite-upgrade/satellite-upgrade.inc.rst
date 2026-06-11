.. _satellite-upgrade:

.. _neteye-upgrade-satellites-cluster:

.. _neteye-upgrade-satellites-single:

Satellite Upgrade from |neteye_previous_version| to |neteye_version|
====================================================================

This guide will lead you through the steps specific for upgrading
a **NetEye Satellite** installation from version |neteye_previous_version_bold| to
|neteye_version_bold|.

.. warning:: Remember that you must upgrade sequentially without
   skipping versions, therefore an upgrade to |neteye_version_bold| is possible only
   from |neteye_previous_version_bold|; for example, if you have version **4.21**, you
   must first upgrade to the **4.22**, then **4.23**, and so on.

Before starting an upgrade, you should read very carefully the latest
release notes on `NetEye's blog
<https://www.neteye-blog.com/category/neteye/release-notes-2/>`_ and
check out the features that will be changed or deprecated after the upgrade.
You should check also the whole section
:ref:`satellite-breaking` below.


.. _satellite-breaking:

Breaking Changes
----------------

This release does not introduce any breaking changes.

.. _satellite-prerequisites:

Prerequisites
-------------

Before starting the upgrade, you should read very carefully the latest
release notes on `NetEye's blog
<https://www.neteye-blog.com/category/neteye/release-notes-2/>`_ and
check out the features that will be changed or deprecated after the upgrade.

#. All NetEye packages installed on a currently running version must be
   updated according to the :ref:`update procedure <update-procedure>` prior
   to running the upgrade.
#. NetEye Master must be version |neteye_version_bold| fully updated

    #. If the |ne| Master is running a :ref:`Sprint Release<sprint-releases>` version,
       the |ne| Master must be on the latest Sprint Release available of |ne| |neteye_version_bold|
       (usually |neteye_version|-sr4).
#. NetEye Satellite must be version |neteye_previous_version_bold| fully updated
#. The |neteye_version| NetEye Satellite configuration archive can be found on the satellite at |satellite_config_file|.
   To generate and send the Satellite configuration archive see the section
   :ref:`Satellite Configuration <neteye-satellite-configuration>`

.. _satellite-run-upgrade:

1. Run the Upgrade
------------------

To automatically download the latest upgrade you can run the following command on the **Satellite**:

.. code:: bash

   sat# (nohup neteye satellite upgrade &) && tail --retry -f nohup.out

After the command was executed, the output will inform if the upgrade was successful or not:

* In case of successful upgrade you might need to restart NetEye to properly apply the upgrades.
  If the reboot is not needed, please skip the next step.
* In case the command fails refer to the :ref:`troubleshooting section<update-ts>`.

.. _satellite-upgrade-reboot:

2. Reboot
---------

Restart NetEye to apply the upgrades correctly.

   .. code:: bash

      sat# neteye node reboot
