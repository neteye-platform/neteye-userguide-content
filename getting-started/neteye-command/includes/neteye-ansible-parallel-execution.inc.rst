To enhance speed and efficiency, the :command:`neteye install`,
:command:`neteye update`, :command:`neteye upgrade` and all :command:`neteye backup {run, restore, config apply}`
feature a parallel processing mechanism of Ansible playbooks. To maintain and manage the order of
operations among services that require a specific order, this process relies on
a dependency tree that is built based on the dependencies declared. The
installation tree differs from the one used for the update and upgrade (which
are the same). You may look at this dependencies graphs in the following images,
for the install and update/upgrade respectively:

.. figure:: /img/dependencies-install.svg

   A visual representation of the :command:`neteye install` services dependency graph


.. figure:: /img/dependencies-update_upgrade.svg

   A visual representation of the :command:`neteye update` and :command:`neteye upgrade` dependency graph

   .. figure:: /img/dependencies-backup.svg

   A visual representation of the :command:`neteye backup {run, restore, config apply}` dependency graph

Each box represents a |ne| service or component, and the arrows
represent the dependencies between them. For example, if service `A` depends on
service `B` (i.e. service `B` must have finished its configuration before
service `A` can start) the arrow goes from `B` to `A`.

The procedure basically works as follows:

#. The dependency tree is built based on the dependencies declared in the
   services configuration files (:file:`/usr/share/neteye/install/` and
   :file:`/usr/share/neteye/update_upgrade/` for the appropriate commands). For
   each subdirectory, which represents a service, the dependencies are read from
   the :file:`dependencies.json` file. If no dependencies are declared, the
   service is considered independent and is executed as soon as the procedure
   is started. Dependency names are the same as the service names (i.e. the
   directory names).

#. The tree is then traversed in a topological order. If all the dependencies
   of a service are met, the service is executed in parallel with the other
   services.

   #. The entrypoint for a service is the :file:`main.yml` playbook found in its config directory.

   #. For each service, a :file:`dropin.d` subdiectory may be specified. If present, every playbook found will be executed after the :file:`main.yml` playbook in a unspecified order.

   #. If present, the :file:`post.yml` playbook will be executed after all the other playbooks have finished.

   #. If any error occurs during the execution of a playbook, the procedure will let currently running playbooks finish, and then stop the execution of the remaining playbooks, even if they do not depend on the failed service.

   #. Logs for each service are stored in the :file:`/neteye/local/os/logs/neteye_command/[install,update,upgrade]/yyyyMMdd-hhmmss/` directory.
