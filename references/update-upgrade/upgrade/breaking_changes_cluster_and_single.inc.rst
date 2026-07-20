GLPI 11 Upgrade
~~~~~~~~~~~~~~~~

NetEye is now shipping GLPI 11, upgrading from GLPI 10.0. This major version update may include breaking changes for your existing GLPI configuration and plugins.
If you have installed additional plugins in GLPI, you must follow the official GLPI migration guide to ensure they are compatible with GLPI 11:
`Specific migration of GLPI 10 plugins to GLPI 11 <https://help.glpi-project.org/tutorials/procedures/updating-glpi#specific-migration-of-glpi-10-plugins-to-glpi-11>`_.

GLPI PHP configurations have moved from ``/neteye/local/php/conf/php.d/`` to ``/neteye/shared/glpi/conf/php.d/``. If you have custom configurations, you must manually move them to the new path.

During the upgrade to GLPI 11, the tables of the OCS Inventory module will be removed as they are a leftover of the old plugin. If you need to preserve this data, ensure you have a backup before proceeding.
