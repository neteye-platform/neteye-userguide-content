SLM Filters migration to Icinga DB
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Starting from NetEye 4.48, Objects Filters configured in SLM Contracts accept only
Icinga DB filter expressions and no longer support the old IDO filter syntax.

Existing Object Filters will be automatically migrated to the new syntax during the upgrade process.
At the same time, any new SLM Contract created after the upgrade must use the new syntax for Object Filters.
To create Object Filters with the new syntax, it is recommended to use the search filter builder in the Icinga DB Overview
and then copy the generated expression.

For example, the old filter expression ``host_name=neteye*`` will now need to be written in the new syntax as ``host.name~neteye*``.

Monitoring module removed
~~~~~~~~~~~~~~~~~~~~~~~~~

Starting from NetEye 4.48, the Monitoring module will be removed. All modules that previously relied on the Monitoring module must be fully migrated to Icinga DB before upgrading.
The previously selected roles and permissions of the Monitoring module have been migrated to the equivalent Icinga DB roles and permissions.

IDO Support removed
~~~~~~~~~~~~~~~~~~~

Starting from NetEye 4.48, the IDO backend is no longer supported, so all monitoring data and integrations must be migrated to Icinga DB.
As a consequence, since IDO is no longer used, ``idoreports`` will also be removed in favor of Icinga DB reports.
Migration is needed before proceeding with the upgrade.
Before proceeding, ensure a backup of the IDO database has been created.

Log Manager module removed
~~~~~~~~~~~~~~~~~~~~~~~~~~

Starting from NetEye 4.48, the deprecated `Log Manager <https://neteye.guide/4.47/feature-modules/elastic-stack/deprecated-modules.html>`__
module and its UI will be removed.
As a result, the file ``/neteye/shared/rsyslog/conf/rsyslog.d/logmanager-hosts.conf``, which maps the IP addresses received from Rsyslog to
hostnames and host groups, will no longer be managed via the UI. Any future additions or changes to these mappings will therefore need to be
made manually by updating the file directly.

During the upgrade procedure, the ``logmanager`` database will be dropped.
If you want to preserve its data, we recommend backing it up before running the upgrade.

Additionally, the ``retention-policy-neteyelocal`` service will be removed during the upgrade.

The Log Manager module was responsible for managing the retention of the log files written by Rsyslog. After the upgrade, the Logscleaner
service handles log retention by compressing log files and deleting them after 30 days. For more information, including how to customize the
log retention policy, see the :ref:`Rsyslog section <rsyslog>`.

telegraf-local User Customization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

During the Upgrade process, the system will perform an optimization of the current `telegraf-local` configuration.
Currently, every |ne| tenant creates a separate Telegraf Consumer instance, reading metrics from the tenant's NATS topic
and writing them into the tenant's InfluxDB database. If the Alyvix module is installed and enabled for the tenant, an
additional Telegraf Consumer per tenant is created. For example, the Telegraf Consumer for Telegraf metrics is created
under :file:`/neteye/local/telegraf/conf/neteye_consumer_influxdb_<tenant>.conf`, with its own dropin directory in
:file:`/neteye/local/telegraf/conf/neteye_consumer_influxdb_<tenant>.d/`.

With |ne| 4.48, there will be only one Telegraf Consumer (two if Alyvix is installed, per PCS node) reading from all
tenants's NATS topics and writing the data into the respective tenant's InfluxDB database. This significantly reduces
the number of Telegraf Consumers and optimizes resource usage, especially in environments with many tenants.
Any customization made for a specific tenant's Telegraf Consumer, will be automatically backed up under
:file:`/root/telegraf_local_consumer_conf_backup/` and deleted during the Upgrade process.

If you have made any, you will need to adapt your previous custom configuration to the new architecture, which is now
tenant-agnostic, and prepare the newly updated customization into
:file:`/neteye/local/telegraf/conf/neteye_consumer_telegraf_metrics.d` (or
:file:`/neteye/local/telegraf/conf/neteye_consumer_alyvix_metrics.d` for Alyvix). The new architecture works in such a
way that for all InfluxDB Nodes (the default `influxdb.neteyelocal` and all the ones created under `InfluxDBOnlyNodes`
in case of a Cluster), there will be one Telegraf Output plugin configured to dynamically write into the correct
tenant's database, using the `tagpass <https://docs.influxdata.com/telegraf/v1/configuration/>`__ feature to filter the
metrics by tenant. Two tags are now added by Telegraf processors: `tenant_influxdb_db` which states the tenant's
InfluxDB database to write into, and `tenant` which states the tenant name. The tenant name will be discovered by the
name of the NATS subject, which is in the format `<tenant>.<module>.<data_type>` (i.e. `acme_tenant.telegraf.metrics`
refers to the subject for the `acme_tenant` tenant).

To migrate your previous custom configuration, you will need to exploit the same `tagpass` feature. For inputs, you need
to enrich the metrics with the `tenant` and `tenant_influxdb_db` tags, and for processors and outputs you need to use
the `tagpass` to filter the metrics by tenant. Below is an example of all the three configurations:

.. code-block:: toml

    # Example of custom configuration for a tenant named `acme_tenant` with InfluxDB database `acme_tenant-alyvix`

    # Input plugin configuration, with the addition of the tenant tags
    [[inputs.cpu]]
      # other configuration options...

      # Add the tenant tags to the metrics
      [inputs.cpu.tags]
        tenant = "acme_tenant"
        tenant_influxdb_db = "acme_tenant-alyvix"


    # Processor plugin configuration, with the addition of the tagpass to filter by tenant
    [[processors.override]]
      # other configuration options...

      # Add the tagpass to filter the metrics by tenant
      [processors.override.tagpass]
        tenant = ["acme_tenant"]

    # Output plugin configuration, with the addition of the tagpass to filter by tenant
    [[outputs.influxdb_v2]]
      # other configuration options...

      # Optionally, you can also exploit yourself the `tenant_influxdb_db` tag
      database_tag = "tenant_influxdb_db"

      # Add the tagpass to filter the metrics by tenant
      [outputs.influxdb_v2.tagpass]
        tenant = ["acme_tenant"]

.. warning:: You now must ensure that any custom configuration for the Telegraf Consumer is compatible with the new
   setup, where a single Consumer handles metrics for all tenants. To check before the Upgrade, you can use the
   :command:`telegraf --config ./telegraf.conf --config-directory ./telegraf.d --test` command, which will validate the
   configuration and print any error found.

Once you are ready, and you have ensured that the current configuration is compatible with the new setup, you need to do
two steps:

#. First, you need to prepare the new configuration for the Telegraf Consumers, by creating the drop in directory
   :file:`/neteye/local/telegraf/conf/neteye_consumer_telegraf_metrics.d` (or
   :file:`/neteye/local/telegraf/conf/neteye_consumer_alyvix_metrics.d` for Alyvix) and creating there the newly created
   custom configuration files after adapting them to the new setup as explained above on all PCS nodes in case of a
   Cluster.
#. Update the owner and mode of the new configuration files to match the previous ones, which are `telegraf:telegraf`
   and `0640` respectively and `telegraf:telegraf` and `0750` for the drop in directories.
#. Then, enable the acknowledgement flag in the UI, specifically under
   `Configuration > Modules > neteye > Configuration`.

.. note:: There is no need to remove the previous configuration, since it will be automatically disabled and cleaned up
   during the Upgrade.

During the Upgrade, Tenant-specific read-only InfluxDB users will be created. If you previously have created read only
users for visualization, such as a Grafana Data Source, and it happens that you have named them `<db_name>_ro`, an
upgrade prerequisite check will fail, asking you to add these users's passwords to the corresponding password file,
following this pattern: :file:`/root/.pwd_influxdb_${influxdb_host}_${influxdb_user}`, where `${influxdb_host}` is the
hostname of the InfluxDB node (i.e. `influxdb.neteyelocal`) and `${influxdb_user}` is the username of the read only user
(i.e. `acme_tenant_ro`). Password will be validated against the correct InfluxDB node according to the tenant's
configured InfluxDB node, otherwise they will be automatically generated if no such user is found.

OCS Inventory Module removed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Starting from |ne| 4.48, the deprecated OCS Inventory module will be removed in favour of the GLPI Asset Management
module, which provides more advanced features and better integration with the rest of the NetEye ecosystem. This only
applies to installations with the `neteye-asset` Feature Moduele installed.

.. warning:: All of the configuration related to OCS Inventory will be automatically removed, including the shared
   directories (:file:`/neteye/shared/ocsinventory-server` and :file:`/neteye/shared/ocsinventory-ocsreports`), the OCS
   Inventory database (`ocsweb`) and all the PCS and DRBD resources if on a cluster. If you want to preserve any of
   these, we recommend backing them up before running the upgrade.

You will be finally asked to confirm the removal of OCS Inventory by enabling the acknowledgement flag in the UI,
specifically under `Configuration > Modules > assetmanagement > Configuration`, before proceeding with the upgrade.

Elastic Stack upgrade to 9.4.1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In NetEye 4.48, Elastic Stack upgrades from version ``9.3.3`` to ``9.4.1``. To ensure compatibility, review the official breaking changes linked below:

* `Elasticsearch <https://www.elastic.co/docs/release-notes/elasticsearch/breaking-changes#elasticsearch-9.4.0-breaking-changes>`__

* `Kibana <https://www.elastic.co/docs/release-notes/kibana/breaking-changes#kibana-9.4.0-breaking-changes>`__

* `Elastic Agent <https://www.elastic.co/docs/release-notes/elastic-agent/breaking-changes#elastic-agent-9.4.0-breaking-changes>`__

* `Logstash <https://www.elastic.co/docs/release-notes/logstash/breaking-changes#logstash-940-breaking-changes>`__

* `Beats <https://www.elastic.co/docs/release-notes/beats/breaking-changes#beats-9.4.0-breaking-changes>`__
