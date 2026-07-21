Before starting the upgrade, carefully read the latest release notes on `NetEye's blog <https://www.neteye-blog.com/category/neteye/release-notes-2/>`_ and check the features that will change or be deprecated.

#. All NetEye packages installed on a currently running version must be updated according to the
   :ref:`update procedure <update-procedure>` prior to running the upgrade.

#. NetEye must be up and running in a healthy state.

#. .. include:: /references/update-upgrade/update/free-disk-space.inc.rst

#. .. include:: /references/update-upgrade/update/elastic-prerequisites.inc.rst

#. To prepare for integration of RKE2, please ensure that the following requirements are met:

   - The system must have at least 12GB of disk space available on the :file:`/neteye/local/rke2` directory for the RKE2 installation.

   .. note:: It is recommended to create a separate logical volume for the :file:`/neteye/local/rke2` directory to avoid running out of disk space on the root filesystem.

   - The following ports must be available for RKE2 to function properly on `0.0.0.0`:

     - TCP 6442: load balancer for the Kubernetes API server
     - TCP 6443: Kubernetes API server
     - TCP 6444: used in case of restore procedures
     - TCP 9345: RKE2 local supervisor
     - TCP 9346: load balancer for the RKE2 local supervisor

   - RKE2 official RPM repository must be reachable from the |ne| system. The repository URL is :file:`https://rpm.rancher.io/`.

   - In case of cluster installations, you must ensure the correct roles are assigned to the nodes in order to meet the minimum requirements for a Kubernetes cluster.
     For more information on the roles and their requirements, please refer to the :ref:`kubernetes-roles` section.

   - You should have created and synced on all nodes the :file:`/etc/neteye-environment.yaml`. For more information please refer to *Step 9* of the :ref:`ne-setup-part-one` section.
