.. _import-identity-provider-groups-inside-nested-group:

Importing User Federation Groups inside another Group
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section describes how to import a user federation group inside another
group in **Keycloak**. This is especially helpful in a multi-tenant scenario
where multiple user federations or IdPs are configured in **Keycloak** and you
want to avoid group names collision when importing them.

After successfully configuring a user federation in **Keycloak** (see
:ref:`Configure federated LDAP/AD <ldap-configuration>`) and adding a group
mapper, you can import the user federation group inside another group in
**NetEye** by setting the ``Groups Path`` field in the group mapper
configuration, as shown in the following image:

.. _figure-full-group-path-from-ldap:

.. figure:: /getting-started/setup/img/keycloak-group-mapper-groups-path-ldap.png
   :alt: keycloak group mapper full groups path ldap

   Setting to import a user federation group inside another group.
