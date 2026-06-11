.. _neteye-upgrade-single:

Single Node Upgrade from |neteye_previous_version| to |neteye_version|
======================================================================

This guide will lead you through the steps specific for upgrading
a **NetEye Single Node** installation from version |neteye_previous_version_bold| to
|neteye_version_bold|.

Upgrading a NetEye Single Node takes a nontrivial amount of
time. Granted the environment connectivity is seamless,
the upgrade procedure may take up to **30 minutes**.

.. warning:: Remember that you must upgrade sequentially without
   skipping versions, therefore an upgrade to |neteye_version_bold| is possible only
   from |neteye_previous_version_bold|; for example, if you have version **4.27**, you
   must first upgrade to the **4.28**, then **4.29**, and so on.


.. _single-breaking:

Breaking Changes
----------------

.. include:: /references/update-upgrade/upgrade/breaking_changes_cluster_and_single.inc.rst

.. _single-prerequisites:

Prerequisites
-------------

.. include:: /references/update-upgrade/upgrade/prerequisites.inc.rst


.. _single-upgrade:

1. Run the Upgrade
------------------

To perform the upgrade, run from the command line the following command::

  neteye# (nohup neteye upgrade &) && tail --retry -f nohup.out


After the command was executed, the output will inform if the upgrade was successful or not:

* In case of successful upgrade you might need to restart NetEye to properly apply the upgrades.
  If the reboot is not needed, please skip the next step.
* In case the command fails refer to the :ref:`troubleshooting section<update-ts>`.


.. _single-upgrade-reboot:

2. Reboot
---------

Restart NetEye to apply the upgrades correctly.

   .. code:: bash

      neteye# neteye node reboot


.. _upgrade-additional-steps-single:

3. Additional Tasks
-------------------

.. include:: /references/update-upgrade/upgrade/upgrade-additional-tasks.inc.rst
