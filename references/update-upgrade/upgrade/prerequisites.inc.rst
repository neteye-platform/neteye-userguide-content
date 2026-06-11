Before starting the upgrade, carefully read the latest release notes on `NetEye's blog <https://www.neteye-blog.com/category/neteye/release-notes-2/>`_ and check the features that will change or be deprecated.

#. All NetEye packages installed on a currently running version must be updated according to the
   :ref:`update procedure <update-procedure>` prior to running the upgrade.

#. NetEye must be up and running in a healthy state.

#. .. include:: /references/update-upgrade/update/free-disk-space.inc.rst

#. .. include:: /references/update-upgrade/update/elastic-prerequisites.inc.rst

#. Make sure you have migrated all your monitoring data from IDO to Icinga DB, because it's a mandatory requirement before upgrading to NetEye 4.48. The migration is performed using the :ref:`neteye cluster upgrade-prerequisites ido-migration <neteye-cluster-upgrade-prerequisites-ido-migration-start>` command.

#. If ``idoreports`` is in use, run ``icingacli reporting migrate idoreports`` on the node where Icinga Web 2 is running to migrate to Icinga DB reports before proceeding with the upgrade. It is highly recommended to first run ``icingacli reporting migrate idoreports --dry-run`` to verify compatibility with your existing report filters without applying any changes.

#. Before starting the upgrade, you must set the corresponding flags under **Configuration / Modules / neteye / Configuration** to disable the IDO DB, IDO reports and Monitoring module. You can proceed with the upgrade only after selecting these flags.

#. To confirm that you read the breaking changes regarding the removal of the Log Manager module, set the flag under **Configuration / Modules / logmanager / Configuration**.
