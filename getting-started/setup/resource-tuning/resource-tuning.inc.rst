
This section will contain a collection of suggested settings for various
services running on NetEye.

MariaDB
~~~~~~~

**MariaDB** is started with default upstream settings. If the size of
an installation requires it, resource usage of MariaDB can be adjusted
to meet the higher requirements for performance. The following
settings can be added to a file
``/neteye/shared/mysql/conf/my.cnf.d/custom.conf``::

  [mysqld]
  innodb_buffer_pool_size=16G
  tmp_table_size = 512M
  max_heap_table_size = 512M
  innodb_sort_buffer_size=16000000
  sort_buffer_size=32M

Icingaweb2 GUI
~~~~~~~~~~~~~~

Performance of the **Icingaweb2** Graphical User Interface, can
significantly be improved in high load environments by adding INDEX
and updating the COLUMN definition of hostgroups and history related
tables. To do this, execute the below queries manually::

  ALTER TABLE icinga_hostgroups MODIFY hostgroup_object_id bigint(20) unsigned NOT NULL;
  ALTER TABLE icinga_hostgroups ADD UNIQUE INDEX idx_hostgroups_hostgroup_object_id (hostgroup_object_id);
  ALTER TABLE icinga_commenthistory ADD INDEX idx_icinga_commenthistory_entry_time (entry_time);
  ALTER TABLE icinga_downtimehistory ADD INDEX idx_icinga_downtimehistory_entry_time (entry_time);
  ALTER TABLE icinga_notifications ADD INDEX idx_icinga_notifications_start_time (start_time);
  ALTER TABLE icinga_statehistory ADD INDEX idx_icinga_statehistory_state_time (state_time);

Icinga 2
~~~~~~~~

When loading its configuration, **Icinga 2** validates objects and
evaluates ``apply for`` rules in parallel. The number of worker
threads used for this work is controlled by the internal
``Configuration.Concurrency`` value, which upstream Icinga 2 sets to
one thread per available CPU core. On systems with more than eight
CPU cores that default is not optimal: synchronization overhead
between threads can outweigh the CPU-time benefit of wider
parallelism and even worsen validation performance, as also noted in
the `Icinga 2 troubleshooting guide
<https://icinga.com/docs/icinga-2/latest/doc/15-troubleshooting/#try-reducing-concurrency-threads>`_.

On NetEye, because Icinga 2 shares CPU with
Elasticsearch and the other platform components, a good compromise between
validation duration and CPU footprint is necessary, so the ``Configuration.Concurrency``
default value is set to 8, or to the number of available CPU cores
on systems with fewer than 8 cores.

This choice comes from a benchmark of ``time icinga2 daemon -C
-DConfiguration.Concurrency=N``, run with thread counts from 1 to the
number of available CPU cores, on three realistic configurations with
hosts importing template chains and services created mostly through
apply rules, with 1,000, 5,000 and 10,000 hosts, resulting in roughly
93,000, 466,000 and 932,000 configuration objects.

As the following graphs show, the default value 8 keeps validation
duration close to the best achievable one, while keeping CPU usage reasonable.
It can still be changed per environment, following the procedure below.

.. figure:: img/time-vs-threads-32-cores.png
   :align: center
   :alt: Configuration validation time versus number of threads

   Elapsed time of ``icinga2 daemon -C`` for
   different values of ``Configuration.Concurrency``, on a 32-core system.

.. figure:: img/time-vs-threads-16-cores.png
   :align: center
   :alt: Configuration validation time versus number of threads on a 16-core system

   Elapsed time of ``icinga2 daemon -C`` for
   different values of ``Configuration.Concurrency``, on a 16-core system.

To persist a different value in your environment, add the following line to
``/neteye/shared/icinga2/conf/sysconfig/icinga2``::

   ICINGA2_CONCURRENCY=<number-of-threads>

Replace ``<number-of-threads>`` with a value between 1 and the number of
available CPU cores in your system.

Then reload systemd, restart Icinga 2, and verify that the override is active::

  systemctl daemon-reload
  systemctl restart icinga2.service
  systemctl show icinga2.service -p ExecStart
