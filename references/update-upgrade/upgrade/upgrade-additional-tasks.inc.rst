The IDO database is not removed automatically during the upgrade, so if you want to delete it you have to run the following commands:

   .. code:: bash

      mysql -e "DROP DATABASE icinga;"
      mysql -e "DROP USER 'icinga'@'localhost';"

If you have the Elastic Stack installed, the ``retention-policy-neteyelocal``
service will be removed during the upgrade procedure. After the upgrade is
complete, you will need to Director deploy to make the changes effective.

Keycloak Hostname Strict Mode
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To avoid disrupting existing installations, the upgrade keeps ``KC_HOSTNAME_STRICT`` disabled
(``false``), so the public hostname continues to be resolved dynamically from incoming requests.

.. important:: Configuring a fixed hostname is a **prerequisite for upgrading to NetEye 4.50**.
   This requirement is enforced for security reasons.
   Follow the :ref:`Keycloak Hostname Configuration <hostname-configuration-upgrade-migration>`
   section to complete this configuration.
