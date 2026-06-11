.. _neteye-upgrade-cluster:

Cluster Upgrade from |neteye_previous_version| to |neteye_version|
==================================================================

This guide will lead you through the steps specific for upgrading
a **NetEye Cluster** installation from version |neteye_previous_version_bold| to
|neteye_version_bold|.

Granted the environment connectivity is seamless, the upgrade
procedure may take up to **30 minutes per node**.

.. warning:: Remember that you must upgrade sequentially without
   skipping versions, therefore an upgrade to |neteye_version_bold| is possible only
   from |neteye_previous_version_bold|; for example, if you have version **4.27**, you
   must first upgrade to the **4.28**, then **4.29**, and so on.

.. _cluster-breaking:

Breaking Changes
----------------

.. include:: /references/update-upgrade/upgrade/breaking_changes_cluster_and_single.inc.rst


.. _cluster-prerequisites:

Prerequisites
-------------

.. include:: /references/update-upgrade/upgrade/prerequisites.inc.rst


.. _cluster-upgrade:

1. Run the Upgrade
------------------

The Cluster Upgrade is carried out by running the following command::

   cluster# (nohup neteye upgrade &) && tail --retry -f nohup.out

.. warning:: If the NetEye Elastic Stack feature module is installed and a new version of Elasticsearch is available,
   please note that the procedure may take a while to upgrade the Elasticsearch cluster. For more information on the
   Elasticsearch cluster upgrade and how to customize the upgrade process, please consult the :ref:`dedicated section <es-upgrade-and-shard-relocation>`.

After the command was executed, the output will inform if the upgrade was successful or not:

* In case of successful upgrade you might need to restart the nodes to properly apply the upgrades.
  If the reboot is not needed, please skip the next step.
* In case the command fails refer to the :ref:`troubleshooting section<update-ts>`.

.. _cluster-upgrade-reboot:

2. Reboot Nodes
---------------

Restart each node, **one at a time**, to apply the upgrades correctly.

#. Run the reboot command

      .. code:: bash

         cluster-node-N# neteye node reboot

#.  In case of a standard NetEye node, put it back online once the reboot is finished

      .. code:: bash

         cluster-node-N# pcs node unstandby --wait=300

You can now reboot the next node.

.. _cluster-reactivation:

3. Cluster Reactivation
-----------------------

At this point you can proceed to restore the cluster to high availability operation.

#. Run the checks in the section :ref:`Checking that the Cluster Status
   is Normal <check-cluster-status>`.  If any of the above checks fail,
   please contact `our service and support team <https://siwuerthphoenix.atlassian.net/servicedesk/customer/portals>`__
   before proceeding.

#. Re-enable fencing on the last standard node, if it was enabled prior to
   the upgrade::

     cluster# pcs property set stonith-enabled=true


.. _upgrade-additional-steps-cluster:

4. Additional Tasks
-------------------

.. include:: /references/update-upgrade/upgrade/upgrade-additional-tasks.inc.rst
