
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
