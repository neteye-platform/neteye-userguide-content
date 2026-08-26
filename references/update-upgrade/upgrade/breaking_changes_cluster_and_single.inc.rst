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

Icinga 2 Upgrade to v2.16
~~~~~~~~~~~~~~~~~~~~~~~~~

|ne| 4.50 upgrades Icinga 2 from version 2.15 to 2.16.

Please review the official upstream `upgrade guide <https://icinga.com/docs/icinga-2/latest/doc/16-upgrading-icinga-2/>`_.

To address the deprecations regarding ``ElasticsearchWriter``, ``Elasticsearch Datastream Writer``
and ``FilterExpression`` permission, please refer to the :ref:`upgrade-additional-steps-cluster` section.
