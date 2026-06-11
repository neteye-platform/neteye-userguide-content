Geomap migration to IcingaDB
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Starting with the next release, the Geomap module is being migrated from the IDO database to IcingaDB.
As part of this migration, some IDO fields previously available in the Map fields section of the Maps Configurator
will be deprecated and will no longer have corresponding fields in IcingaDB.
As a result, if a map was configured to display any of these deprecated fields, they will no longer be visible after the upgrade.
It is recommended to update map configurations in order to use other available fields in IcingaDB.

**List of removed IDO fields:**

- Host Active Checks Enabled Changed
- Host Alias
- Host Contact
- Host Current Notification Number
- Host Event Handler Enabled Changed
- Host Flap Detection Enabled Changed
- Host Last Notification
- Host Modified Host Attributes
- Host Notifications Enabled Changed
- Host Obsessing
- Host Obsessing Changed
- Host Passive Checks Enabled Changed
- Host Percent State Change
- Instance Name

Moreover, all the fields related to services and service groups will not be selectable anymore in the Map fields section of the Maps Configurator.

Elastic Stack upgrade to 9.3.3
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In NetEye 4.47, Elastic Stack upgrades from version ``9.2.6`` to ``9.3.3``. To ensure compatibility, review the official breaking changes linked below:

* `Elasticsearch <https://www.elastic.co/docs/release-notes/elasticsearch/breaking-changes#elasticsearch-9.3.0-breaking-changes>`__

* `Kibana <https://www.elastic.co/docs/release-notes/kibana/breaking-changes#kibana-9.3.0-breaking-changes>`__

* `Elastic Agent <https://www.elastic.co/docs/release-notes/elastic-agent/breaking-changes#elastic-agent-9.3.0-breaking-changes>`__

* `Logstash <https://www.elastic.co/docs/release-notes/logstash/breaking-changes#logstash-900-breaking-changes>`__

* `Beats <https://www.elastic.co/docs/release-notes/beats/breaking-changes#beats-9.3.0-breaking-changes>`__
