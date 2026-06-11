.. _update-procedure:

Update Procedure
================

This guide will lead you through the steps of updating NetEye.

Prerequisites
-------------

#. NetEye must be up and running in a healthy state.

#. .. include:: /references/update-upgrade/update/free-disk-space.inc.rst

#. .. include:: /references/update-upgrade/update/elastic-prerequisites.inc.rst


.. _neteye-update-single:

Update NetEye Single Instance
-----------------------------

#. Run the update command:

   .. code:: bash

      neteye# nohup neteye update

   After the command was executed, the output will inform if the update was successful or not:

   * In case of successful update you might need to restart NetEye to properly apply the updates.
     If the reboot is not needed, please skip the next step.
   * In case the command fails refer to the :ref:`troubleshooting section<update-ts>`.

#. Reboot the node to apply the updates correctly if required:

   .. code:: bash

      neteye# neteye node reboot

#. Finally, to ensure that any potentially stopped and/or newly installed NetEye
   services are running, use the command

   .. code:: bash

      neteye# neteye start


.. _neteye-update-cluster:

Update NetEye Cluster
---------------------

Updating a cluster will take a nontrivial amount of time, however no
downtime needs to be planned.
Granted the environment connectivity is seamless, the update procedure
may take up to **15 minutes per node**.


1. Run the Update
~~~~~~~~~~~~~~~~~

The Cluster Update is carried out by running the following command::

   cluster# (nohup neteye update &) && tail --retry -f nohup.out

.. warning:: If the NetEye Elastic Stack feature module is installed and a new version of Elasticsearch is available,
   please note that the procedure may take a while to update the Elasticsearch cluster. For more information on the
   Elasticsearch cluster update and how to customize the update process, please consult the :ref:`dedicated section <es-upgrade-and-shard-relocation>`.

After the command was executed, the output will inform if the update was successful or not:

* In case of successful update you might need to restart the nodes to properly apply the updates.
  If the reboot is not needed, please skip the next step.
* In case the command fails refer to the :ref:`troubleshooting section<update-ts>`.

2. Reboot Nodes
~~~~~~~~~~~~~~~

Restart each node, **one at a time**, to apply the updates correctly.

#. Run the reboot command

      .. code:: bash

         cluster-node-N# neteye node reboot

#.  In case of a standard NetEye node, put it back online once the reboot is finished

      .. code:: bash

         cluster-node-N# pcs node unstandby --wait=300

You can now reboot the next node.


3. Cluster Reactivation
~~~~~~~~~~~~~~~~~~~~~~~

At this point you can proceed to restore the cluster to high availability operation.

#. Run the checks in the section :ref:`Checking that the Cluster Status
   is Normal <check-cluster-status>`.  If any of the above checks fail,
   please call `our service and support team <https://siwuerthphoenix.atlassian.net/servicedesk/customer/portals>`__
   before proceeding.

#. Re-enable fencing on the last standard node, if it was enabled prior to
   the update::

     cluster# pcs property set stonith-enabled=true


.. _neteye-update-satellites:

NetEye Satellites
-----------------

.. include:: /references/update-upgrade/update/satellite-update.inc.rst

.. _neteye-update-dpo:

DPO Machine
-----------
It is possible to update the Docker image used on the DPO machine, by running, on a |ne| Master node,
the following command:

.. code:: bash

   neteye# neteye dpo setup

The command updates the container image at every execution, ensuring you are using the latest available
image matching your |ne| version, and restarts the already configured containers with the updated image.
