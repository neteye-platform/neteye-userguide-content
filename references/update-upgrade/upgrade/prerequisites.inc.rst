Before starting the upgrade, carefully read the latest release notes on `NetEye's blog <https://www.neteye-blog.com/category/neteye/release-notes-2/>`_ and check the features that will change or be deprecated.

#. All NetEye packages installed on a currently running version must be updated according to the
   :ref:`update procedure <update-procedure>` prior to running the upgrade.

#. NetEye must be up and running in a healthy state.

#. .. include:: /references/update-upgrade/update/free-disk-space.inc.rst

#. .. include:: /references/update-upgrade/update/elastic-prerequisites.inc.rst

#. Before upgrading, if you are in a Cluster environment, you must ensure that the virtual IP for the cluster resources for GLPI is correctly configured.
   You must run the command :ref:`neteye cluster upgrade-prerequisites glpi-pcs-resources set <neteye-cluster-upgrade-prerequisites-glpi-pcs-resources-set>` to set the virtual IP address that will be used for the GLPI PCS resources.
   When running the command, NetEye will propose an IP address that it detects as free, but it is the user's responsibility to verify that this IP is not already in use by custom services on their cluster.
   This command is required because NetEye needs a dedicated virtual IP address to manage GLPI resources in the cluster through PCS.
