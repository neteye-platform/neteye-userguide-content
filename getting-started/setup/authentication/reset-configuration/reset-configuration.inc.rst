.. _reset-keycloak-configuration:

Emergency Reset of Keycloak Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you are reading this section, it means that you are in a situation where you cannot login to the NetEye web interface anymore.

We advise to resort to this specific procedure only in case of emergency, as it will reset all the Keycloak configuration to the default settings.

.. warning::
   You should always try to debug and fix the issue before resorting to this procedure.
   This should be a last resort kind of operation, meaning that nothing else worked before this.

.. warning::
   Executing this procedure will remove all your LDAP/AD configurations, IdPs, imported users, and groups from Keycloak, and you will have to again import again the users from LDAP/AD and recreate the local users/groups/idp configurations you had set up before.

In order to reset the login credentials, you can follow these steps:

* Backup the Keycloak database :command:`mysqldump keycloak > "/root/keycloak_db_backup-$(date -Iseconds).sql"`
* Delete the Keycloak database :command:`mysql -e 'DROP DATABASE keycloak;'`
* Delete the file locate in :file:`/root/.pwd_keycloak_neteye-internal-keycloak-admin` (on all nodes, if NetEye is in a cluster)
* Launch the following command :command:`neteye install`

After the procedure, the `neteye-internal-keycloak-admin` and `root` users will be recreated, and therefore you will be able to login again.

.. warning::
   You will have to reconfigure all the authentication settings, IdPs, LDAP/AD configurations, and local users in Keycloak.
