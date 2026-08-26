GLPI Bundled Plugins
~~~~~~~~~~~~~~~~~~~~

|ne| 4.50 comes with a set of bundled plugins for GLPI, available by default
in the GLPI container. These plugins are shipped in the folder ``/usr/share/glpi/plugins`` and
can be installed and enabled directly from the GLPI web interface.
The complete list of bundled plugins is available in the :ref:`glpi-plugins` section.

During the upgrade, all the plugins present in ``/usr/share/glpi/plugins`` and included in
that list will be moved to a backup directory ``/root/glpi_plugins_backup``. All the other
plugins, except for ``icingaweb2sso`` and ``inventorymultitenancy``, will be moved to
the directory ``/neteye/shared/glpi/data/marketplace``.

From |ne| 4.50, you can add a plugin to GLPI by placing the plugin files in the
folder ``/neteye/shared/glpi/data/marketplace`` on the host system.


Elastic Stack upgrade to version 9.5.2
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In |ne| 4.50, Elastic Stack upgrades from version 9.4.5 to 9.5.2. To ensure compatibility, review the official breaking changes linked below:

* `Elasticsearch <https://www.elastic.co/docs/release-notes/elasticsearch/breaking-changes#elasticsearch-9.5.2-breaking-changes>`_
* `Kibana <https://www.elastic.co/docs/release-notes/kibana/breaking-changes#kibana-9.5.2-breaking-changes>`_
* `Elastic Agent <https://www.elastic.co/docs/release-notes/elastic-agent/breaking-changes#elastic-agent-9.5.2-breaking-changes>`_
* `Logstash <https://www.elastic.co/docs/release-notes/logstash/breaking-changes#logstash-950-breaking-changes>`_
* `Beats <https://www.elastic.co/docs/release-notes/beats/breaking-changes#beats-9.5.2-breaking-changes>`_
