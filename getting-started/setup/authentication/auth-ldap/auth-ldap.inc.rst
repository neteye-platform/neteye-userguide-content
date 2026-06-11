.. _ldap-configuration:

Configure federated LDAP/AD
~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to use an external LDAP/AD server to authenticate the users, you can configure the LDAP federation in Keycloak following this guide `Keycloak LDAP documentation <https://www.keycloak.org/docs/25.0.6/server_admin/index.html#_user-storage-federation>`_.

**On your first configuration**, make sure to set the following parameters as indicated below:

Section `LDAP searching and updating`:

.. figure:: /getting-started/setup/img/ldap-configuration-1.png


Section `Synchronization settings`:

.. figure:: /getting-started/setup/img/ldap-configuration-2.png


Section `Advanced settings`:

.. figure:: /getting-started/setup/img/ldap-configuration-3.png

After filling out all the required fields, click on the `Save` button to save the configuration.

.. _test-ldap-configuration:

Test your LDAP/AD configuration
```````````````````````````````

To make sure that your LDAP/AD configuration is correct, test it by taking these steps:

1. clicking on both the `Test connection` and `Test authentication` buttons in the `Connection and authentication settings` section of the LDAP configuration page.

.. figure:: /getting-started/setup/img/ldap-testing-1.png

2. navigating to the `Users` tab and perform a search for a user with `*` in the search field.

.. figure:: /getting-started/setup/img/ldap-testing-2.png

If users are correctly found, the LDAP/AD configuration is correct. In case no users are found, you should check the LDAP/AD configuration on Keycloak and try again.

.. warning::
   In the event of the LDAP/AD configuration not working, local users can still log in with their credentials.
   Until the LDAP/AD configuration is fixed however, user search on keycloak will not work.

Finalize the LDAP/AD configuration
``````````````````````````````````
After you have verified that the LDAP/AD configuration is correct, you should finalize the configuration by manually triggering a full-sync of the users in the `Action` dropdown, inside your LDAP/AD configuration page. You can achieve this with the **Sync all users** button in the dropdown.

.. figure:: /getting-started/setup/img/ldap-configuration-4.png

This will synchronize all the users from the LDAP/AD server to Keycloak, and you can now use the LDAP/AD users to log in to NetEye.

.. warning::
   It is possible that imported groups are not fully visible in the :menuselection:`Groups` tab until a full-sync has been triggered.
