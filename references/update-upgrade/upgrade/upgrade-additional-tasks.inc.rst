After upgrading to NetEye 4.47, you should complete the following additional tasks to fully
benefit from the new features and improvements.

IcingaDB Historical Data migration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

With the introduction of IcingaDB as the new backend for monitoring, the historical monitoring
events must be migrated from the legacy Icinga2 IDO database to IcingaDB. This ensures that
all past events are preserved in IcingaDB and hence accessible via the modules relying on IcingaDB,
maintaining continuity in monitoring data.

This migration can be performed any time after upgrading to NetEye 4.47,
and it is a **mandatory requirement before upgrading to NetEye 4.48**.

The migration is performed using the :ref:`neteye cluster upgrade-prerequisites ido-migration
<neteye-cluster-upgrade-prerequisites-ido-migration-start>` command.

Important considerations before starting
````````````````````````````````````````

Before starting the migration, please consider the following:

- **Time**: The migration process can take a **long time** to complete, depending on the amount
  of data to migrate and the performance of the database. It could take from a few hours to
  **several days**. Plan the migration activity in advance.

- **Disk space**: The migration requires **significant disk space** to store the migrated data
  in IcingaDB. Always check the available disk space before starting by using the ``--check-disk-space`` option.

- **Chunk-based approach**: We strongly recommend migrating data in chunks (e.g., 3 months at a time)
  rather than migrating all data at once. This allows you to:

  - Monitor the disk space consumption progressively
  - Reduce the impact on database performance
  - Recover more easily in case of issues

- **Selective migration**: You don't have to migrate all historical data. You can choose to migrate
  only the data from a specific date onwards using the ``--from`` parameter. Consider which timeframe
  is relevant for your needs:

  - **SLA data**: Usually you want to migrate more SLA data since it is used for
    SLM reporting.
  - **History data**: You may choose to migrate less history data (e.g., 2-3 months) since older
    host/service history events are typically less relevant.

Types of data to migrate
````````````````````````

There are two types of historical data that must be migrated:

- **SLA data**: Required for SLM (Service Level Management) reporting.
- **History data**: Used in the history pages of single hosts and services.

Both types of data must be migrated independently using the dedicated subcommands, and you can
choose different timeframes for each type based on your reporting needs.

.. important::

   Do not run SLA and History migrations in parallel. Complete the SLA migration first,
   then start the History migration.

Migration procedure
```````````````````

To perform the migration, follow these steps:

#. **Check available disk space**

   Before starting the migration, verify that there is enough disk space available:

   .. code:: bash

      neteye cluster upgrade-prerequisites ido-migration sla start --from <YYYY-MM-DD> --check-disk-space
      neteye cluster upgrade-prerequisites ido-migration history start --from <YYYY-MM-DD> --check-disk-space

   This command performs only the disk space check without starting the migration.
   If the check fails, increase the available disk space before proceeding.

#. **Start the SLA data migration (in chunks)**

   We recommend migrating data in chunks of approximately 3 months. Start from the most recent data and then proceed backwards in time.
   For example, if it is early 2026 and you want to migrate SLA data starting from January 1, 2025 you can decide to migrate in the following chunks:

   .. code:: bash

      neteye cluster upgrade-prerequisites ido-migration sla start --from 2025-09-01

   Wait for this chunk to complete, verify disk space, then continue with the next chunk:

   .. code:: bash

      neteye cluster upgrade-prerequisites ido-migration sla start --from 2025-06-01

   Repeat until you have migrated all the SLA data you need.

#. **Start the History data migration (in chunks)**

   Apply the same chunk-based approach for history data. You may choose a more recent start date
   if older history data is not needed:

   .. code:: bash

      neteye cluster upgrade-prerequisites ido-migration history start --from 2026-01-01

#. **Monitor the migration progress**

   You can monitor the migration progress using the following commands:

   - To view the logs:

     .. code:: bash

        neteye cluster upgrade-prerequisites ido-migration sla logs
        neteye cluster upgrade-prerequisites ido-migration history logs

   - To check the status:

     .. code:: bash

        neteye cluster upgrade-prerequisites ido-migration sla status
        neteye cluster upgrade-prerequisites ido-migration history status

   The status command shows the state of each migration run. Possible states are:

   - **running**: The migration is still in progress.
   - **completed**: The migration has been completed successfully.
   - **stopped**: The migration has been stopped by the user.
   - **failed**: The migration has failed due to an error.

#. **Wait for both migrations to complete**

   The migration process runs in a tmux session in the background, so it will continue even if
   the SSH connection is closed.

#. **Enable the IcingaDB Historical Data feature flag**

   Once both migrations are completed, enable the "IcingaDB Historical Data" feature flag so that
   NetEye starts using the migrated data stored in IcingaDB.

   To enable the feature flag, navigate in the NetEye web interface to:
   **Configuration > Modules > neteye > Configuration > IcingaDB Historical Data Feature Flag**.

Managing the migration process
``````````````````````````````

If you need to stop or resume the migration, use the following commands:

- **Stop a running migration**:

  .. code:: bash

     neteye cluster upgrade-prerequisites ido-migration sla stop
     neteye cluster upgrade-prerequisites ido-migration history stop

- **Resume a stopped or failed migration**:

  .. code:: bash

     neteye cluster upgrade-prerequisites ido-migration sla resume
     neteye cluster upgrade-prerequisites ido-migration history resume

Performance tuning
``````````````````

Depending on the cluster setup and the amount of data to migrate, the migration process could
stress the database. The ``--bulk`` parameter helps to find a good compromise between speed and
database load.

The default bulk size (100) is a conservative value that works in most cases, but in some
environments it could be too high. We recommend the following approach to find the optimal
bulk size for your setup:

#. Start the migration of a shorter time period (e.g., one month) with the default bulk size:

   .. code:: bash

      neteye cluster upgrade-prerequisites ido-migration sla start --from 2026-01-01

#. Monitor the database status during the migration.

#. If the database is stressed, stop the migration and restart with a lower bulk size (e.g., 50):

   .. code:: bash

      neteye cluster upgrade-prerequisites ido-migration sla stop
      neteye cluster upgrade-prerequisites ido-migration sla start --from 2026-01-01 --bulk 50

#. Once you find a suitable bulk size, proceed with the full migration using the selected value.

Migration of IDO Grafana dashboards
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you have Grafana dashboards that query the legacy Icinga IDO MySQL database, you must migrate
them to use the IcingaDB data sources **before upgrading to NetEye 4.48**. Upgrade prerequisite
checks will block the upgrade if any legacy IDO data sources or dashboards are still in use.

Depending on how the IDO database is referenced in your Grafana setup, there are two cases
to address.

Case 1: Data sources using the ``icinga`` MySQL database
````````````````````````````````````````````````````````

If you created a Grafana data source that connects to the ``icinga`` MySQL database, you must:

#. **Create a replacement data source** that connects to the ``icingadb`` database
   instead of ``icinga``.

#. **Migrate all dashboards** that use the old data source. For each affected dashboard, update
   the queries to use the new IcingaDB data source and adapt the queries to the IcingaDB database
   schema. See the example in `Example: Migrating a Grafana dashboard to IcingaDB`_ below
   for a step-by-step walkthrough.

#. **Delete the old data source** that connects to the ``icinga`` database.

.. important::

   An upgrade prerequisite check for NetEye 4.48 verifies that no Grafana data source exists
   that uses the ``icinga`` MySQL database. The upgrade will not proceed until all such data
   sources have been removed.

Case 2: Tenant-specific IDO data sources
````````````````````````````````````````

If you have dashboards that use the default Tenant data sources
``icinga-<tenant_name>-mysql`` provided by NetEye, you must migrate those dashboards to the
corresponding IcingaDB Tenant data sources:

#. **Identify the replacement data source**: For each Tenant, a new data source
   ``icingadb-<tenant_name>-mysql`` is available in the respective Grafana Organization of
   the Tenant.

#. **Migrate all affected dashboards** in each Tenant's Grafana Organization. Update the
   dashboards to use the ``icingadb-<tenant_name>-mysql`` data source and adapt the queries
   to the IcingaDB database schema. See the example in
   `Example: Migrating a Grafana dashboard to IcingaDB`_ below for a step-by-step walkthrough.

#. **Delete the old dashboards** that still reference ``icinga-<tenant_name>-mysql``. Once a
   dashboard has been recreated or updated to use the new data source, remove the old version.

.. important::

   An upgrade prerequisite check for NetEye 4.48 verifies that no dashboard exists that uses
   an ``icinga-<tenant_name>-mysql`` data source. The upgrade will not proceed until all such
   dashboards have been migrated and removed.

Example: Migrating a Grafana dashboard to IcingaDB
``````````````````````````````````````````````````

The following example walks through migrating a specific Grafana dashboard from the legacy
IDO data source to IcingaDB. While your dashboards will certainly differ, the same concepts
and approach apply to any dashboard migration: replace the data source, and rewrite the SQL
queries to match the IcingaDB schema.

The example focuses on a dashboard that displays the state of Alyvix hosts, which are hosts
with the custom variable ``alyvix_node`` set to ``y_singletenant_direct_to_master``.
The dashboard uses two types of SQL queries that need to be migrated:

- A **template variable query** that builds the list of Alyvix hosts by looking up hosts
  with a specific custom variable. This query populates the host selector dropdown at the
  top of the dashboard.
- A **panel query** that retrieves the current state (UP/DOWN) for each selected host and
  displays it in a stat panel.

Both queries must be rewritten to match the IcingaDB schema, as shown in Step 3.

.. figure:: /references/update-upgrade/upgrade/example_grafana_dashboard.png
   :alt: Example Alyvix Hosts Overview dashboard in Grafana

   The Alyvix Hosts Overview dashboard showing the host selector dropdown (template variable)
   and the host state panels.

**Step 1: Create the IcingaDB data source (if it does not exist)**

#. In Grafana, go to **Configuration > Data Sources > Add data source**.
#. Select **MySQL** as the type and set the database to ``icingadb``. Configure the
   remaining connection fields (host, user, password) according to your environment.

**Step 2: Copy the existing dashboard**

#. Open the existing dashboard that uses the legacy IDO data source.
#. Go to **Edit** mode > **Save Dashboard** > **Save as copy** to create a copy of the dashboard.
   Give it a new name (e.g., append ``(IcingaDB)``).
#. In the copied dashboard, update every panel and template variable to use the new IcingaDB
   data source created in Step 1.

**Step 3: Adapt the SQL queries to the IcingaDB schema**

The IcingaDB database uses a different schema than the legacy IDO database. The following
examples show how to rewrite each query and explain the key differences.

*Migrating template variable queries*

The template variable query that retrieves Alyvix host names must be rewritten. The IDO
schema uses ``icinga_objects`` and ``icinga_customvariables``, while IcingaDB uses the
``host``, ``host_customvar``, and ``customvar_flat`` tables.

IDO query:

.. code:: sql

   SELECT `name1`
   FROM `icinga_objects` O
   JOIN `icinga_customvariables` V ON V.object_id = O.object_id
   WHERE O.objecttype_id = 1
     AND V.`varname` LIKE 'alyvix_node'
     AND V.varvalue = "y_singletenant_direct_to_master"
     AND O.is_active = 1

IcingaDB equivalent:

.. code:: sql

   SELECT H.`name`
   FROM `host` H
   JOIN `host_customvar` HC ON HC.`host_id` = H.`id`
   JOIN `customvar_flat` CV ON CV.`customvar_id` = HC.`customvar_id`
   WHERE CV.`flatname` = 'alyvix_node'
     AND CV.`flatvalue` = 'y_singletenant_direct_to_master'

Key changes:

- ``icinga_objects`` is replaced by the ``host`` table (IcingaDB stores hosts directly,
  there is no centralized objects table).
- ``icinga_customvariables`` is split into ``host_customvar`` (linking hosts to custom
  variables) and ``customvar_flat`` (storing flattened variable names and values).
- The ``objecttype_id = 1`` and ``is_active = 1`` filters are no longer needed because
  the ``host`` table only contains active host objects.

*Migrating panel queries*

The panel query that retrieves the current host state must also be rewritten. The IDO
schema uses ``icinga_hoststatus`` and ``icinga_hosts``, while IcingaDB uses ``host_state``
and ``host``.

IDO query:

.. code-block:: sql
   :force:

   SELECT `current_state`
   FROM `icinga_hoststatus` S
   JOIN `icinga_hosts` H ON H.`host_object_id` = S.`host_object_id`
   WHERE `display_name` LIKE CONCAT($host, '%')

IcingaDB equivalent:

.. code-block:: sql
   :force:

   SELECT S.`hard_state` AS `current_state`
   FROM `host_state` S
   JOIN `host` H ON H.`id` = S.`host_id`
   WHERE H.`display_name` LIKE CONCAT($host, '%')

Key changes:

- ``icinga_hoststatus`` is replaced by ``host_state``.
- ``icinga_hosts`` is replaced by ``host``.
- ``current_state`` is replaced by ``hard_state`` (or ``soft_state`` if you want the
  current soft state). The values remain the same: ``0`` = UP, ``1`` = DOWN.
- The join key changes from ``host_object_id`` to ``host_id`` / ``id``.

**Step 4: Verify and clean up**

#. Verify that the migrated dashboard displays the correct data by comparing it with the
   original dashboard.
#. Once you have confirmed that the new dashboard works correctly, delete the old dashboard
   that uses the legacy IDO data source.
#. If no other dashboards reference the old IDO data source, delete the old data source as
   well.

.. tip::

   The following table summarizes the most common IDO-to-IcingaDB table mappings useful
   when migrating dashboard queries:

   .. list-table::
      :header-rows: 1
      :widths: 40 40

      * - IDO Table
        - IcingaDB Table
      * - ``icinga_objects``
        - ``host`` / ``service`` (no centralized objects table)
      * - ``icinga_hosts``
        - ``host``
      * - ``icinga_hoststatus``
        - ``host_state``
      * - ``icinga_services``
        - ``service``
      * - ``icinga_servicestatus``
        - ``service_state``
      * - ``icinga_customvariables``
        - ``host_customvar`` + ``customvar_flat`` (or ``service_customvar`` + ``customvar_flat``)
