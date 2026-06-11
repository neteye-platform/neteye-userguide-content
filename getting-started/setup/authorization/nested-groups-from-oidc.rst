.. _import-OIDC-groups-inside-nested-group:

Importing OIDC IdP Groups inside another Group
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section describes how to import an OIDC IdP group inside another group in
**Keycloak**. This is especially helpful in a multi-tenant scenario where
multiple user federations or IdPs are configured in **Keycloak** and you want to
avoid group names collision when importing them.

After successfully configuring an OIDC identity provider in **Keycloak** (see
:ref:`Configure External Identity Providers <idp-configuring>`), you can add a
custom mapper as described in the following steps:

#. Go to the **Mappers** tab of your configured identity provider and click on
   the **Add mapper** button.

#. Insert an arbitrary **Name** for the mapper, select **Mapper Type** to
    **NetEye OIDC Group Path Mapper**.

#. Set the **Claim for group array** to the attribute of your identity provider
   containing the groups information.

#. Optionally, you can set **Override base groups path** to import the groups
   inside another group. By default, the alias of the identity provider is used
   as the parent group path.

.. _figure-full-group-path-from-oidc:

.. figure:: /getting-started/setup/img/keycloak-group-mapper-groups-path-oidc.png
   :alt: keycloak group mapper full groups path ldap

   Setting to import a OIDC identity provider groups inside another group.
