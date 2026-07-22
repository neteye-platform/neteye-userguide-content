.. _neteye-command-basics:

Basic Concepts & Usage
**********************

The :command:`neteye` CLI command is used from the CLI to carry out a
few tasks related to the NetEye Installations, both Single Nodes and
Cluster. Various sub-commands are available and are analysed in this
section.

All output from each command execution is saved in a log file at :file:`/neteye/local/os/log/neteye_command/`.

A retention policy is applied to these files on a daily basis as follows:

- Files are compressed
- In case the size of the logs exceeds the configured maximum, the oldest files are deleted. This value is by default *500MB*.
- Files older than *2 years* are deleted

You can configure the retention policy in :file:`/neteye/local/os/conf/logscleaner.d/neteye_command_logs.toml`

.. _neteye-install:

``neteye install``
------------------

``neteye install`` is a wrapper around a number of scripts that take care of
the initial configuration of a NetEye installation and then start all services
that are required for NetEye to operate correctly. Please note that this
command **must never be used** in the update and upgrade procedures.

If ran on a cluster, it will automatically install |ne| on every node defined
in :file:`/etc/neteye-cluster` and must be called only once from any cluster node.

.. note:: In cluster environments, services are configured in parallel without
   requiring the cluster nodes to be in `standby` mode. This approach involves
   setting up services on all nodes simultaneously, rather than waiting for the
   configuration process to finish on the initial node.

Before making any changes, the secure install script will also run a
subset of :ref:`light and deep health checks <neteye-health-check>` to
ensure that NetEye will not be adversely affected due to a transient
problem like low disk space or custom configurations.

.. note:: This automatic set of check is not intended to replace the
   good practice of running a separate, manual deep health check both
   before and after an update or upgrade.

To run it manually, just type in the name of the script in a shell as
root::

   # neteye install

The install command accepts the following optional arguments:

.. csv-table::
   :widths: 30,70
   :header: "Option", "Description"

   ":command:`--restrict-services-to`", "Restrict the services to install to the ones specified in the commandline in the comma separated list format. The list of available services can be found by running ``neteye install --help``."
   ":command:`--dump-failed-logs`", "In case of failures during the command execution, dump the entire log of failed services instead of the last lines."
   ":command:`--throttle-parallel-jobs`", "Throttle the number of parallel jobs during the install process. This option is useful when you want to reduce the load on the system during this procedure. The value of this option is an integer that represents the maximum number of parallel jobs that can run at the same time. By default, there is no limit on the number of parallel jobs."

If you want to learn more about this command, please refer to the advanced
:ref:`neteye-install-advanced` section where you can find more details about the
underlying processes and execution steps.

.. _neteye-status:

``neteye status``
-----------------

:command:`neteye status` is used to list the NetEye services and their status,
either **UP** or **DOWN**.

.. _neteye-start:
.. _neteye-stop:

``neteye start | stop``
-----------------------

The :command:`neteye start` and :command:`neteye stop` commands are used to
start or stop all NetEye services at once.


.. _neteye-update-upgrade:

``neteye update`` and ``neteye upgrade``
----------------------------------------

The :command:`neteye update` and :command:`neteye upgrade` commands are used to update or upgrade your |ne|
installation to the latest version available for your current release or to the next major version respectively.
These commands are very similar in how they behave and for this reason options for these commands are described together
in the following sections, but please note the key differences between the two processes in terms of the expected outcome
and and other important aspects that you should be aware of before running these commands, after this table.

All of the following parameters are **optional**.

.. csv-table::
   :header: "Option", "Description"

   ":command:`--dump-failed-logs`", "In case of failures during the command execution, dump the entire log of failed services instead of the last lines."
   ":command:`--disable-rhel-repos`", "Disable RHEL repositories during the update/upgrade procedure. You can use this option to perform the update/upgrade procedure in case of problems contacting RHEL repositories."
   ":command:`--download-only`", "Only download the packages without installing them. This option is useful when you want to download updates in advance and install them later, for example during a maintenance window. Please note that if this option is used, the updates will not be installed and the system will not be updated until you run :command:`neteye update` or :command:`neteye upgrade` without this option."
   ":command:`--clear-dnf-cache`", "Discard and clean the DNF cache used by the :command:`--download-only` option."
   ":command:`--throttle-parallel-jobs`", "Throttle the number of parallel jobs during the update or upgrade process. This option is useful when you want to reduce the load on the system during these procedures. The value of this option is an integer that represents the maximum number of parallel jobs that can run at the same time. By default, there is no limit on the number of parallel jobs."
   ":command:`--enable-development-releases`", "Enable the usage of development releases for the update or upgrade. Please note that development releases may contain bugs and issues, so it is recommended to use this option only in non-production environments or after consulting |support|_."
   ":command:`--enable_beta_repo`", "Enable the usage of beta repositories, which may contain pre-release version of the packages. Please note that beta releases may contain bugs and issues."
   ":command:`--enable_staging_repo`", "Enable the usage of staging repositories, which may contain candidate versions of the packages that are not yet released. Please note that staging releases may contain bugs and issues."


.. _neteye-update:

``neteye update``
~~~~~~~~~~~~~~~~~

The :command:`neteye update` command is intended to update your |ne|
installation to the latest version available for your current release to get the
latest bug fixes or security patches. To learn more about the update process, or
if you want to successfully carry out the update, please refer to the advanced
:ref:`neteye-update-advanced` and :ref:`update-procedure` sections respectively.


.. _neteye-upgrade:

``neteye upgrade``
~~~~~~~~~~~~~~~~~~

The objective of the :command:`neteye upgrade` command is to bring your |ne|
installation to the next major version. The :command:`neteye upgrade` requires
the latest updates of the current NetEye version to be installed: if some
updates are available the command will stop with an error message. The upgrade
process is a complex operation and may take a long time to complete.

If the :command:`neteye upgrade` command is successful, a message will inform
you that the upgrade procedure concludes successfully. Otherwise, if the
commands breaks at some point, you need to fix the failed tasks manually and
then launch again the command. Check also the :ref:`update-ts` section for more
information and directions about fixing the problems. To learn more about the
upgrade process, or if you want to successfully carry out the upgrade for single
or cluster environments, please refer to the advanced
:ref:`neteye-upgrade-advanced`, :ref:`neteye-upgrade-single` and
:ref:`neteye-upgrade-cluster` sections respectively.


.. _neteye-config:

``neteye config``
-----------------

The neteye config command lets you interact with different fundamental parts of
the neteye configuration.

.. _neteye-config-cluster-check:

``neteye config cluster check``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The subcommand ``check`` allows you to check the cluster configuration
defined in the :file:`/etc/neteye-cluster` file. This command performs a
comprehensive validation of the cluster settings and the entire node role
configuration, ensuring that all assigned roles meet their specific requirements,
the roles distribution across cluster nodes follows proper configuration patterns,
and the overall cluster topology is valid for correct operation.

.. _neteye-config-cluster:

``neteye config cluster sync``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The subcommand ``sync`` allows you to copy the cluster config files (
:file:`/etc/neteye-cluster` and :file:`/etc/neteye-satellite.d/*`) from the
current node to all other nodes in the cluster to make sure all files are in
sync.


.. _neteye-config-auth-idp:

``neteye config auth idp``
~~~~~~~~~~~~~~~~~~~~~~~~~~


``neteye config auth idp list``
```````````````````````````````

Lists all the configured identity providers with their configurable properties.


``neteye config auth idp set``
``````````````````````````````

This command allows you to overwrite certain fields of the identity providers.
The identity provider instance is addressable by its alias as a positional
argument.

Usage:

.. code:: bash

   neteye# neteye config auth idp set ALIAS [OPTIONS]

Where `ALIAS` is the alias of the identity provider you want to modify.
It can be retrieved by running the `neteye config auth idp list` command.

Options:

.. csv-table::
   :widths: 30,70
   :header: "Option", "Description"

   "-\ -domains", A list of comma-separated domains. Will overwrite the current domains.
   "-\ -force", Force the overwriting of the changes without a confirmation prompt.


.. _neteye-node:

``neteye node``
---------------

The command :command:`neteye node` is responsible for performing operations on
the node on which it is executed such as updating the operating system.

.. _neteye-node-system-upgrade:

``neteye node system-upgrade``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The command :command:`neteye node system-upgrade` executed on a specific node is
responsible for the upgrade of NetEye from version **4.22** to **4.23** and also
of the upgrade of the operating system from CentOS 7 to RHEL 8.

As described in the upgrade procedure it will upgrade the operating system from
NetEye 4.22 on CentOS 7 to RHEL 7 and then to RHEL 8 with NetEye 4.23.
After each change of operating system, a reboot is required.
In the case of a cluster, the command must be executed node by node and before
starting with a new node, the previous one must have finished the upgrade to RHEL 8.

This command does not carry out any task on versions **4.23** onwards.

.. _neteye-node-tags:

``neteye node tags``
~~~~~~~~~~~~~~~~~~~~

The sub-commands of :command:`neteye node tags` allow the management of the tags that are
used for the registration to Red Hat and that are uploaded to Red Hat Insights.

.. _neteye-node-tags-set:

``neteye node tags set``
````````````````````````

.. csv-table::
   :widths: 30,70
   :header: "Option", "Description"

   "-\ -customer_id", "The customer ID"
   "-\ -deployment", "The deployment type of the machine. One of ['prod', 'demo', 'dev', 'poc', 'qa']"
   "-\ -type", "The machine type. One of ['physical', 'virtual']"
   "-\ -nuuid", "The neteye UUID assigned to this machine. This option is not required with `dev` deployment type."

.. hint:: Please refer to the consultants, or |support|_ to obtain the NUUID and customer_id associated to your NetEye installation.

For all options that aren't provided with the command, you will be prompted by the wizard.

The :command:`neteye node tags set` command must be executed to successfully register a
|ne| Node to Red Hat and to `Red Hat Insights <https://www.redhat.com/en/technologies/management/insights>`_.
The command asks the user to enter all the necessary tags that can't be generated by the
system itself. After the user has entered all the tags, it will call
:command:`neteye node tags regenerate` to generate the file that will be used by Red Hat
Insights.

.. _neteye-node-tags-regenerate:

``neteye node tags regenerate``
```````````````````````````````

This command will generate some system tags automatically and then merges the custom tags
provided by the user and then merges them with the custom tags provided by the user into
a single file that will be used by Red Hat Insights. The system tags will be generated
on each run. If the custom tags are not provided, it will throw an error and ask the user
to first execute :command:`neteye node tags set` to generate the custom tags. This command
can be used if some system specifications change to propagate those tags to Red Hat
Insights.

This command will also be called during :command:`neteye install` if the system is
not registered to Red Hat Insights and the registration is not disabled.


.. _neteye-node-tags-list:

``neteye node tags list``
`````````````````````````

This command lists all the tags that are collected and sent to Red Hat Insight.

.. _neteye-node-register:

``neteye node register``
~~~~~~~~~~~~~~~~~~~~~~~~

The command :command:`neteye node register` registers the RHEL 8 subscription and sets up
Red Hat Insights. By default it uses the |ne| activation key. If the node is
registered with another organization, this behaviour can be changed by modifying
:file:`/neteye/local/os/conf/subscription-manager.toml`.

.. code:: toml

    [subscription-manager]
    enable_auto_rhel_subscription = false

If the setting `enable_auto_rhel_subscription` is set to `false` the Red Hat registration
and the Red Hat Insights subscription are skipped.


.. _neteye-node-reboot:

``neteye node reboot``
~~~~~~~~~~~~~~~~~~~~~~

.. include:: /getting-started/neteye-command/includes/neteye-node-reboot.inc.rst


.. _neteye-feature-module:

``neteye feature-module``
-------------------------

.. include:: /getting-started/neteye-command/includes/neteye-feature-module.inc.rst

.. _neteye-tenant:

``neteye tenant``
-----------------

The command :command:`neteye tenant` helps you to manage the configuration of the
|ne| Tenants.

.. _neteye-tenant-config-create:

``neteye tenant config create``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The :command:`neteye tenant config create` command configures a new |ne| Tenant.
The command takes care of creating the actual configuration of the Tenant, configures
the services dedicated to the Tenant and, if is issued on a |ne| Cluster,
takes care of synchronizing the Tenant configuration on all the Cluster Nodes.
The command can also be run on on a Single Node.

Usage:

.. code:: bash

   neteye# neteye tenant config create TENANT_NAME [OPTIONS]

Note that the name of the Tenant must respect the following constraints:

* Must match the following regex  ``/^[a-zA-Z0-9_]{1,32}$/``, i.e. it must contain only alphanumeric characters
  and underscores and must contain between 1 and 32 characters
* Can not contain the ``icinga2`` string

However, any type of strings or characters is allowed as an input for a "Tenant Name" if made using
``--display-name`` option.

Options:

.. csv-table::
   :widths: 30,70
   :header: "Option", "Description"

   "-\ -display-name", "(Mandatory) a more user-friendly name of the Tenant used for visualization purposes:
   It must be unique and can contain spaces and special characters."
   "-\ -enable-module", "(Optional) allows users to enable :ref:`NetEye Feature Module <neteye-modules>` at **tenant level**
   starting from the set of installed Feature Modules on the |ne| Master. The enable option accepts multiple values, for example:
   ``--enable-module neteye-asset --enable-module neteye-cmd --enable-module neteye-elastic-stack``. Note that the ``enable-module`` option can be also used in the
   ``neteye tenant config modify`` for expanding the set of already enabled modules. Should be defined for all tenants, including Master Tenant."
   "-\ -influxdb-node", "(Optional) the hostname of the InfluxDB-only node, as defined in the :file:`/etc/neteye-cluster`,
   on which Tenant data will be stored. Defaults to the InfluxDB instance **influxdb.neteyelocal**,
   i.e. the InfluxDB instance run by the |ne| Master."
   "-\ -alyvix-metrics-retention", "(Optional) the retention, in days, of the Alyvix Test Cases performance metrics
   stored in the InfluxDB database dedicated to the Tenant"
   "-\ -custom-override-grafana-org", "(Optional) the custom Grafana organization associated with the Tenant. By
   default it matches the ``display-name``."
   "-\ -custom-override-glpi-entity", "(Optional) the custom GLPI entity associated with the Tenant. By default it
   is ``Root entity > 'Tenant Entity'``, using the ``display-name`` as 'Tenant Entity'. in case of setting a
   custom-override-glpi-entity parameter, the full entity path should always be used as input and the entity
   should be first manually created in GLPI. i.e. If the custom entity 'Bolzano' is created as child of the entity
   'Enterprise s.r.l.' that is under the 'Root entity' of GLPI, the ``custom-override-glpi-entity`` to add should be
   ``Root entity > Company s.r.l. > Bolzano``."
   "-\ -override-glpi-asset-active-statuses", "(Optional) option to set the values to be considered as **active status** when fetching GLPI assets
   in the monitoring host page to reduce the number of results. Values should be a comma-separated list of statuses,
   i.e. ``active,enabled,ok``. This option is used to filter the assets retrieved by active status only, to reduce the number of results."
   "-\ -set-glpi-asset-hidden-fields", "(Optional) option to set what fields to hide in the monitoring host page. Values should be a comma-separated
   list of fields. Please refer to :ref:`asset-information-in-monitoring-host` for more information."
   "-\ -force", "(Optional) option to force the command to overwrite the existing configuration of the Tenant.
   This option will completely overwrite the existing configuration, please refer to :ref:`neteye-tenant-config-modify`
   if you need to partially modify the configuration of an existing Tenant."


.. _neteye-tenant-config-apply:

``neteye tenant config apply``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The :command:`neteye tenant config apply` command sets up the services dedicated to the Tenant
passed as argument. This command is used internally by :ref:`neteye-tenant-config-create`, so |ne| users do not
need to run this command explicitly.

Usage:

.. code:: bash

   neteye# neteye tenant config apply TENANT_NAME

Options:

.. csv-table::
   :widths: 30,70
   :header: "Option", "Description"

   "-\ -all", "(Optional) Apply the configuration of all the configured |ne| Tenants.
   This option is mutually exclusive with *TENANT_NAME*."

.. _neteye-tenant-config-modify:

``neteye tenant config modify``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The :command:`neteye tenant config modify` command allows you to modify the configuration
of an existing |ne| Tenant. The command can also be run on on a Single Node.

Usage:

.. code:: bash

   neteye# neteye tenant config modify TENANT_NAME [OPTIONS]

Options:

.. csv-table::
   :widths: 30,70
   :header: "Option", "Description"

   "-\ -influxdb-node", "(Optional)"
   "-\ -enable-module", "(Optional)"
   "-\ -alyvix-metrics-retention", "(Optional)"
   "-\ -custom-override-grafana-org", "(Optional)"
   "-\ -custom-override-glpi-entity", "(Optional)"

Please refer to :ref:`neteye-tenant-config-create` for a detailed description of the available options.

Certain values can only be set once. If they are already set, they have to be
overwritten either manually or with :command:`neteye tenant config create --force`.
This, however, does NOT clean up the old configuration or migrate any data.
Right now the fields that can be set only once are: *display-name*,
*influxdb-node*, *custom-override-grafana-org* and *custom-override-glpi-entity*.
For more information, please refer to the official channels: sales, consultants,
or |support|_.


.. _neteye-satellite:

``neteye satellite``
--------------------

The command :command:`neteye satellite` helps you to manage the configuration of the
|ne| Satellites.

.. _neteye-satellite-config-create-command:

``neteye satellite config create``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The :command:`neteye satellite config create` command generates the configuration for one or more satellites.

Usage:

.. code:: bash

   neteye# neteye satellite config create <SATELLITE_NAME> <SATELLITE_NAME2> ...

Options:

.. csv-table::
   :widths: 30,70
   :header: "Option", "Description"

   "-\ -all", "(Optional) Generate the configuration for all the Satellites defined in :file:`/etc/neteye-satellites.d/`."
   "-\ -tenant", "(Optional) The Tenant to which the Satellite belongs. Useful when the name of a satellite is not unique across Tenants."
   "-\ -skip-director-deploy", "(Optional) Avoids deploying the Icinga2 Satellite configuration to the Director. Manual deployment is required afterwards."


.. _neteye-satellite-config-send-command:

``neteye satellite config send``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The :command:`neteye satellite config send` command sends the configuration
files required to set up a new |ne| Satellite.

Usage:

.. code:: bash

   neteye# neteye satellite config send <SATELLITE_NAME>

Options:

.. csv-table::
   :widths: 30,70
   :header: "Option", "Description"

   "-\ -all", "(Optional) Sends the configuration to all the Satellites defined in :file:`/etc/neteye-satellites.d/`."
   "-\ -tenant", "(Optional) The Tenant to which the Satellite belongs. Useful when the name of a satellite is not unique across Tenants."

.. _neteye-satellite-install-command:

``neteye satellite install``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The :command:`neteye satellite install` command is used to install the |ne|
Satellite. It must be run on the Satellite machine itself, and after having
generated and shipped the configuration files using the
:ref:`neteye satellite config create <neteye-satellite-config-create>` and
:ref:`neteye satellite config send <neteye-satellite-config-send>`
commands.

.. _neteye-satellite-update-command:

``neteye satellite update``
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The :command:`neteye satellite update` command is used to update the |ne|
Satellite to the latest version available for your current release to get the
latest bug fixes or security patches. This command accepts the same options
as described in the :ref:`neteye-update-upgrade` section. To learn more about
the update process, or if you want to successfully carry out the update, please
refer to the :ref:`neteye-satellite-update` section.

.. _neteye-satellite-upgrade-command:

``neteye satellite upgrade``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The :command:`neteye satellite upgrade` command is used to bring your |ne|
Satellite to the next major version. The :command:`neteye satellite upgrade`
requires the latest updates of the current NetEye version to be installed: if
some updates are available the command will stop with an error message. The
satellite upgrade requires the NetEye Master to be already upgraded to the
target version, and to have generated and shipped the new configuration files
using the :ref:`neteye satellite config create <neteye-satellite-config-create>`
and :ref:`neteye satellite config send <neteye-satellite-config-send>` commands.
This command accepts the same options as described in the
:ref:`neteye-update-upgrade` section. For more information, please refer to the
:ref:`satellite-upgrade` section.

.. _neteye-dpo:

``neteye dpo``
--------------

.. _neteye-dpo-setup:

``neteye dpo setup``
~~~~~~~~~~~~~~~~~~~~
The :command:`neteye dpo setup` command sets up, directly from |ne|, the DPO machine to run the El Proxy verification.
The setup is based on the configuration which is to be specified in :file:`/etc/neteye-dpo`, as described in :ref:`ebp-setup-verify`.

The configuration file in JSON format contains the following attributes:

   #. **dpo_host**: the IP address or hostname of the DPO machine you would like to configure
   #. **dpo_user**: the user performing the SSH connection to the DPO machine
   #. **elastic_blockchain_proxy_d_path** (Optional): the path to a subfolder under :file:`/neteye/shared/elastic-blockchain-proxy`
      containing additional :command:`toml` configuration files, expected to be found at :file:`/neteye/shared/elastic-blockchain-proxy/conf/elastic_blockchain_proxy.d/`
      on the DPO machine. Default path is :file:`/neteye/shared/elastic-blockchain-proxy/conf/elastic_blockchain_proxy.d`.
   #. **retention_policy_d_path** (Optional): the path to a subfolder under :file:`/neteye/shared/elastic-blockchain-proxy`
      containing customised retention policies. Default path is :file:`/neteye/shared/elastic-blockchain-proxy/conf/retention_policies.d/`.
   #. **es_ca** (Optional): the path to Elasticsearch root CA, needed for the connection when verifying the blockchain.
      If not specified, the default CA file :file:`/neteye/local/elasticsearch/conf/certs/root-ca.crt` will be used.
   #. **blockchains_verification**: an array containing a JSON object for each verification that you would like to
      configure. Each verification object in its turn contains the following attributes:

         #. **tenant**: the Tenant of the blockchain you would like to verify
         #. **retention**: the retention of the blockchain you would like to verify
         #. **tag**: the tag of the blockchain you would like to verify
         #. **webhook_host**: the FQDN of the host where your Tornado Webhook Collector is running
         #. **webhook_token**: the secret token chosen for the Tornado Webhook Collector
         #. **logs_file_size_limit_in_megabytes**: This is the maximum size that the DPO logs and reports can occupy.
            The most recent log and report will be preserved regardless of their size.
         #. **cron_scheduling**: a JSON object that specifies the scheduling of the verification. For more information
            about the values each property can assume, you can consult this `online guide <https://cloud.google.com/scheduler/docs/configuring/cron-job-schedules#cron_job_format>`__

            #. **minute**: minute of the day on which the verification should take place
            #. **hour**: hour of the day on which the verification should take place
            #. **day**: day of the month on which the verification should take place
            #. **month**: month on which the verification should take place
            #. **week_day**: day of the week on which the verification should take place

         #. **es_client_cert_path** (Optional): path of the client certificate used to connect with Elasticsearch for
            the verification. If not specified, the default path :file:`/neteye/local/elasticsearch/conf/certs/neteye_ebp_verify_<tenant>.crt.pem`
            will be used
         #. **es_client_key_path** (Optional): path of the client private key used to connect with Elasticsearch for
            the verification. If not specified, the default path :file:`/neteye/local/elasticsearch/conf/certs/private/neteye_ebp_verify_<tenant>.key.pem`
            will be used
         #. **web_server_ca** (Optional): path to the certificate used by the webserver hosting the Tornado Webhook Collector
         #. **additional_parameters** (Optional): a list of additional parameters, as strings, that will be passed to El Proxy
            verify command

To check out an example of a configuration in :file:`/etc/neteye-dpo` file please consult :ref:`Step 1. Configure the blockchain verification <ebp-verify-neteye-setup>`.

Moreover, the command is also used to update/upgrade the verification containers image on the DPO machine after
a |ne| update/upgrade, which will then cause the restart of the previously configured containers.

The final step of this procedure involves verifying any inconsistencies between the current state of
:file:`/etc/neteye-dpo` and the `DPO` machine. This process is intended to remove any containers (along with the
corresponding blockchain keys and certificates) present in the `DPO` machine but not documented in your configuration
file. This process is also designed to facilitate the removal of any blockchain verification. Before each removal,
you will be prompted to confirm the action.

.. _neteye-alyvix-node:

``neteye alyvix-node``
----------------------

.. _neteye-alyvix-node-setup:

``neteye alyvix-node setup``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The :command:`neteye alyvix-node setup` command sets up the specified Alyvix node connection to |ne|.
Additionally, the :command:`--all` flag can be used to configure all available Alyvix nodes.
Moreover, please note that regardless of any options included, this command must be executed from the |ne| Master.

Usage:

.. code:: bash

   neteye# neteye alyvix-node setup [ALYVIX_NODE_HOSTNAME | --all]


Options:

.. csv-table::
   :widths: 30,70
   :header: "Option", "Description"

   "-\ -all", "(Optional) Execute the setup for all the Alyvix nodes listed in the Director. This option is mutually exclusive with `ALYVIX_NODE_HOSTNAME`"

.. _neteye-cluster:

``neteye cluster``
------------------

The :command:`neteye cluster` command is used to manage the cluster configuration and to perform operations on the cluster nodes.

.. _neteye-cluster-install:

``neteye cluster install``
~~~~~~~~~~~~~~~~~~~~~~~~~~

The :command:`neteye cluster install` creates a basic Corosync/Pacemaker
cluster with a floating ip starting from the configuration file located at
:file:`/etc/neteye-cluster`. If :command:`--force` is specified, the command
tries to create a new cluster destroying any potential existing cluster.

This command automatically updates on each node the :code:`hacluster`
user's password, used for authenticating the nodes to the cluster. In case
:file:`/root/.pwd_hacluster` does not contain a password, a newly created
password will be saved in the file.

Usage:

.. code:: bash

   neteye# neteye cluster install [-y] [--force]


Options:

.. csv-table::
   :widths: 30,70
   :header: "Option", "Description"

   "-y", "(Optional) Interactive-less installation"
   "-\ -force", "(Optional) Force the creation of the cluster (there will be no way to recover your data lost as a result of this action)"

.. _neteye-cluster-recovery:

``neteye cluster recovery``
~~~~~~~~~~~~~~~~~~~~~~~~~~~

``neteye cluster recovery mariadb-galera``
``````````````````````````````````````````

The :command:`neteye cluster recovery mariadb-galera` command supports the recovery of a MariaDB Galera cluster when
not all nodes are reachable. Under normal conditions, NetEye automatically bootstraps the cluster from the most
up-to-date node. However, if one or more nodes are unreachable, NetEye may not have sufficient information to safely
determine the correct bootstrap candidate. In this case, administrative intervention is required.

This command collects recovery information from the available nodes, presents the bootstrap candidates ordered by
their recovery state, and guides the administrator through the recovery procedure.

.. warning:: Executing a cluster bootstrap from a node that is not the most up-to-date one can lead to permanent data loss,
   as transactions present on more advanced nodes may be discarded. Before running this command, it is strongly recommended
   to verify node availability, confirm the selected bootstrap candidate, and ensure that recent backups of the database are available.

.. _neteye-cluster-upgrade-prerequisites-glpi-pcs-resources:

``neteye cluster upgrade-prerequisites glpi-pcs-resources``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The :command:`neteye cluster upgrade-prerequisites glpi-pcs-resources` command
allows to configure the virtual IP address that will be used for the GLPI PCS resources in a cluster environment.
This is a required step before upgrading to a version of NetEye that includes GLPI in the cluster resources.

.. _neteye-cluster-upgrade-prerequisites-glpi-pcs-resources-set:

``neteye cluster upgrade-prerequisites glpi-pcs-resources set``
```````````````````````````````````````````````````````````````

The :command:`neteye cluster upgrade-prerequisites glpi-pcs-resources set` command
sets the virtual IP address for the GLPI PCS resources.

Usage:

.. code:: bash

   neteye# neteye cluster upgrade-prerequisites glpi-pcs-resources set

During execution, the command will suggest an IP address that appears to be available in the same subnet as other cluster resources.
The administrator can then confirm the suggested IP or provide a different one when prompted.
However, it is the administrator's responsibility to ensure that the chosen IP address is not already assigned to other devices or PCS resources.

.. _neteye-cluster-upgrade-prerequisites-neteye-domain:

``neteye cluster upgrade-prerequisites neteye-domain``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This command group migrates Keycloak from dynamically resolving its public hostname from incoming
requests (``KC_HOSTNAME_STRICT=false``, explicitly set after an update or upgrade) to a fixed,
strictly validated hostname. See :ref:`Keycloak Hostname Configuration <hostname-configuration-upgrade-migration>` for more details.

.. note:: This requires ``neteye.frontend_domain`` to be configured in
   :file:`/etc/neteye-environment.yaml`. On a Cluster, remember to run
   :ref:`neteye config cluster sync <cluster-management-commands>` after creating or editing that
   file, since it is not stored on shared storage.

.. _neteye-cluster-upgrade-prerequisites-neteye-domain-apply:

``neteye cluster upgrade-prerequisites neteye-domain apply``
````````````````````````````````````````````````````````````

The :command:`neteye cluster upgrade-prerequisites neteye-domain apply` command sets
``KC_HOSTNAME=https://<frontend_domain>/auth``, ``KC_HOSTNAME_STRICT=true``, and
``KC_HOSTNAME_BACKCHANNEL_DYNAMIC=true`` in :file:`/neteye/shared/keycloak/conf/keycloak.env`, then
restarts Keycloak to apply the new configuration.

Usage:

.. code:: bash

   neteye# neteye cluster upgrade-prerequisites neteye-domain apply

Before applying any change, the command:

- reads ``neteye.frontend_domain`` from :file:`/etc/neteye-environment.yaml`
- verifies that a valid HTTPS certificate is served at ``https://<frontend_domain>/auth``
- creates a backup copy of the current :file:`keycloak.env` file, named
  :file:`keycloak.env.bak_no_public_domain`

.. note:: The command can be run only once: if ``KC_HOSTNAME`` is already configured, it fails
   with an error instead of overwriting the existing configuration.

.. _neteye-cluster-upgrade-prerequisites-neteye-domain-rollback:

``neteye cluster upgrade-prerequisites neteye-domain rollback``
```````````````````````````````````````````````````````````````

The :command:`neteye cluster upgrade-prerequisites neteye-domain rollback` command reverts the
changes performed by :ref:`apply <neteye-cluster-upgrade-prerequisites-neteye-domain-apply>`,
restoring :file:`keycloak.env` from the :file:`keycloak.env.bak_no_public_domain` backup and
restarting Keycloak.

Usage:

.. code:: bash

   neteye# neteye cluster upgrade-prerequisites neteye-domain rollback


``neteye backup``
-----------------

.. _neteye-backup-config-apply:

``neteye backup config apply``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


The :command:`neteye backup config apply` command applies the backup configuration
defined in :file:`/etc/neteye-backup.d/` and also prepares the systemd timer for
the automatic execution of the backup according to the defined schedule.

More information about the backup configuration can be found in the
:ref:`backup-restore-configuration` section.

Usage:

.. code:: bash

   neteye# neteye backup config apply

Options:

.. csv-table::
   :widths: 30,70
   :header: "Option", "Description"

   ":command:`--throttle-parallel-jobs`", "(Optional) Throttle the number of parallel jobs during the config apply process. This option is useful when you want to reduce the load on the system during this procedure. The value of this option is an integer that represents the maximum number of parallel jobs that can run at the same time. By default, there is no limit on the number of parallel jobs."


``neteye backup run``
~~~~~~~~~~~~~~~~~~~~~


The :command:`neteye backup run` command executes a manual backup based on the current backup configuration.

More information about triggering manual backups can be found in the
:ref:`backup-restore-manual-backup` section.

Usage:

.. code:: bash

   neteye# neteye backup run

Options:

.. csv-table::
   :widths: 30,70
   :header: "Option", "Description"

   ":command:`-y`", "(Optional) Interactive-less installation"
   ":command:`--restrict-services-to`", "(Optional) Restrict the backup to specific services"

.. note:: Contrary to other |ne| procedures, to throttle the parallel backup jobs run by this command, please see :ref:`backup-restore-configuration`.


``neteye backup restore``
~~~~~~~~~~~~~~~~~~~~~~~~~


The :command:`neteye backup restore` command executes a manual restore based on the most recent backup.

More information about restoring backups can be found in the
:ref:`backup-restore-restore` section.

Usage:

.. code:: bash

   neteye# neteye backup restore

Options:

.. csv-table::
   :widths: 30,70
   :header: "Option", "Description"

   ":command:`-y`", "(Optional) Interactive-less installation"
   ":command:`--restrict-services-to`", "(Optional) Restrict the backup to specific services"
   ":command:`--throttle-parallel-jobs`", "(Optional) Throttle the number of parallel jobs during the restore process. This option is useful when you want to reduce the load on the system during this procedure. The value of this option is an integer that represents the maximum number of parallel jobs that can run at the same time. By default, there is no limit on the number of parallel jobs."

.. _neteye_scripts:

Supporting Scripts
------------------

Two scripts complement the abilities of the :command:`neteye update`
and :command:`neteye upgrade` commands:

For more details, refer to the next two sections.

.. _neteye-secure-install:

``neteye_secure_install``
~~~~~~~~~~~~~~~~~~~~~~~~~

This utility script has been deprecated starting from |ne| 4.36 and you must now
use :ref:`neteye install <neteye-install>` instead.

The :command:`neteye_secure_install` will still be available only for internal
procedures such as the :command:`neteye update` and :command:`neteye upgrade`
commands.

.. _finalize-neteye-upgrade:

``neteye_finalize_installation``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. include:: /getting-started/neteye-command/includes/finalize-neteye-upgrade.inc.rst
