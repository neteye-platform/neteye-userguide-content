Advanced Topics
===============

In this section, we will cover some advanced topics about the |ne| command.

.. _neteye-install-advanced:

``neteye install``
------------------

In a nutshell, the tasks carried out by the script are:

* To register the machine to RHEL 8
* To set up Red Hat Insights
* To reconfigure NetEye services and/or migrate configurations and databases
  after important changes
* To restart services that were stopped or modified
* To create certificates for secure communication

This command features a parallel execution of the configuration tasks, which is
explored in detail in the :ref:`ne-ansible-parallel` section.

The :command:`neteye install` command can be also limited to a specific service
instead of running the install procedure for all the services in the NetEye installation.
You can use the :command:`--restrict-services-to <services>` option to achieve this,
where *services* is the name of the service you want to install or a list of a
subset of services separated by commas. A list of available services in |ne| can be
found in :ref:`ne-ansible-parallel`.
As an example of the usage for this option, to install only the *mariadb*
service, you can run:

.. code-block:: bash

   neteye install --restrict-services-to mariadb

.. warning::

    If the specified service has parent dependencies, it will be asked for
    confirmation before proceeding with the installation. In this specific case,
    it is possible to execute also the install of all the parent dependencies of
    the specified service.

    For more information about the services dependencies during the install procedure
    please refer to the section :ref:`ne-ansible-parallel`.

.. _neteye-update-advanced:

``neteye update``
-----------------

The :command:`neteye update` command runs a number of tasks as listed below in
order of execution.

.. csv-table::
   :header: "Task", "Order", "Single Node", "Cluster", "Description"
   :widths: 25, 10, 10, 10, 55

   "Health checks", "#1", "yes", "yes", "Carry out health checks to verify that
   NetEye installation is healthy and eligible for update"
   "Parallel update configuration", "#2", "yes", "yes", "Execute the update for
   the supported services in parallel, you can find more information in the
   :ref:`ne-ansible-parallel` section"
   "Unmanage DRBD resources", "#3", "no", "yes", "Sets all drbd resources to
   unmanaged state"
   "Serial update configuration", "#4", "yes", "yes", "Execute the update for
   the remaining services in serial"
   "``.rpmsave`` and ``.rpmnew`` check", "#5", "yes", "yes", "Searches for any
   ``.rpmsave`` and ``.rpmnew`` files"
   "Secure install", "#6", "yes", "yes",  "Execute ``neteye_secure_install`` to
   complete update and initialise all NetEye modules"

If any of these tasks is unsuccessful, a message will explain where the command
failed, allowing you to manually fix the corresponding step, then launch again
the :command:`neteye update` command. Check also the :ref:`update-ts` section
for more information and directions about fixing the problems. To explore the
parallel execution of the configuration tasks, please refer to the
:ref:`ne-ansible-parallel`.

.. _neteye-upgrade-advanced:

``neteye upgrade``
------------------

.. warning:: The :command:`neteye upgrade` command may take a long time
   before it completes successfully, so please do not interrupt it
   until it exits.

The tasks carried out by the :command:`neteye upgrade` command are
listed below in order of execution. It is also mentioned if they run
on Clusters or Single Nodes.

.. rubric:: Single Nodes and Cluster

.. csv-table::
   :header: "Task", "Order", "Single Node", "Cluster", "Description"
   :widths: 25, 10, 10, 10, 55

   "Health checks", "#1", "yes", "yes", "Carry out health checks to verify that
   NetEye installation is healthy and eligible for update"
   "Check update status", "#2", "yes", "yes", "NetEye is fully updated and there
   are no minor (bugfix) updates to be installed. If there are, the upgrade
   process will be interrupted and the user will be asked to run the
   :command:`neteye update` command first."
   "Upgrade eligibility", "#3", "yes", "yes",  "Verify that NetEye is eligible
   for upgrade: it checks which is the installed version (e.g., **4.20**) and
   that the last upgrade :ref:`was finalized <neteye_scripts>`"
   "Repo update", "#4", "yes", "yes", "Update all the NetEye repositories to the
   next version to which it is possible to upgrade (e.g., **4.21**)"
   "Packages check", "#5", "yes", "yes", "Check for new software packages in the
   repositories"
   "Parallel upgrade configuration", "#6", "yes", "yes", "Execute the upgrade
   for the supported services in parallel, you can find more information in the
   :ref:`ne-ansible-parallel` section"
   "Unmanage DRBD resources", "#7", "no", "yes", "Sets all drbd resources to
   unmanaged state"
   "Serial upgrade configuration", "#8", "yes", "yes", "Execute the upgrade for
   the remaining services in serial"
   "``.rpmsave`` and ``.rpmnew`` check", "#9", "yes", "yes", "Searches for any
   ``.rpmsave`` and ``.rpmnew`` files"
   "Finalise installation", "#10", "yes", "yes", "The ``neteye_secure_install``
   and ``neteye_finalize_installation`` scripts are executed"

To explore the parallel execution of the configuration tasks, please refer to
the :ref:`ne-ansible-parallel`.

``neteye update`` vs. ``neteye upgrade``
----------------------------------------

The main difference between the two commands is that the
:command:`neteye update` installs all available packages in the
**current** version of NetEye. On the other side, :command:`neteye
upgrade` installs all available packages in **next** version of
NetEye.

For example, given a NetEye version **4.20**, :command:`neteye update`
fully updates NetEye 4.20 with the latest packages in the **4.20
repository**, while :command:`neteye upgrade` installs and configures
all new packages available in the **4.21** repository.

What ``neteye update`` and ``neteye upgrade`` do *not* do on Clusters
---------------------------------------------------------------------

The following tasks are required to bring a cluster back to the
correct operative status after an update or an upgrade and need
to be carried out manually:

* Restore `stonith` on cluster, which may be left disabled by the update/upgrade procedure
* Restore `DRBD` PCS resources to `managed` state, in case the update/upgrade procedure failed

Additionally, the commands **can not be launched** on `Elastic-only` or a
`Voting-only` nodes. Please note that, however, even if the two commands
can be executed on operative nodes only, the update/upgrade procedure **is
performed** also on `Elastic-only` and `Voting-only` nodes.

Moreover, the commands currently **do not** update/upgrade `InfluxDB-only`
nodes. The maintenance of `InfluxDB-only` nodes should be performed on the user's part.

.. _ne-ansible-parallel:

Parallel execution of automated configuration with Ansible
----------------------------------------------------------

.. include:: /getting-started/neteye-command/includes/neteye-ansible-parallel-execution.inc.rst
