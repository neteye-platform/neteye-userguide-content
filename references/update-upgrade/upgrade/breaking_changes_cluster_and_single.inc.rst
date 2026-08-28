Keycloak Breaking Changes
~~~~~~~~~~~~~~~~~~~~~~~~~

|ne| 4.49 updates Keycloak from version 26.6.2 to 26.7.2, which introduces some breaking
changes. Please review the
`Keycloak Upgrading Guide - Migration Changes <https://www.keycloak.org/docs/latest/upgrading/#migration-changes>`_
before upgrading.

GLPI 11 Upgrade
~~~~~~~~~~~~~~~~

NetEye is now shipping GLPI 11, upgrading from GLPI 10.0. This major version update may include breaking changes for your existing GLPI configuration and plugins.
If you have installed additional plugins in GLPI, please follow the official GLPI migration guide to ensure your plugins are compatible with GLPI 11:
`Specific migration of GLPI 10 plugins to GLPI 11 <https://help.glpi-project.org/tutorials/procedures/updating-glpi#specific-migration-of-glpi-10-plugins-to-glpi-11>`_.

.. note::

   Before starting the NetEye upgrade, as suggested by the GLPI documentation, it is recommended to update all GLPI plugins
   to the latest version compatible with GLPI 10 and PHP 7, so that they are in the best state for the subsequent migration to GLPI 11.

   Once the NetEye upgrade is completed, you should then update the plugins again to a version compatible with GLPI 11.

GLPI PHP configurations have moved from ``/neteye/local/php/conf/php.d/`` to ``/neteye/shared/glpi/conf/php.d/``. If you have custom configurations, you must manually move them to the new path, with the exception of the PHP timezone customization which is automatically migrated during the upgrade.

During the upgrade to GLPI 11, the database tables related to the GLPI plugin ocsinventoryng (all tables prefixed with ``glpi_plugin_ocsinventoryng``) will be removed from the ``glpi`` database, as they are left over from the old plugin.
If you need to preserve this data, ensure you have a backup before starting the upgrade process.

.. note::

   For security reasons, the Webhook feature introduced in GLPI 11 is disabled in NetEye until
   an appropriate mitigation for the NetEye environment is in place.
