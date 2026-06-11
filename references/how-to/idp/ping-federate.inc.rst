.. _idp-ping-federate-oidc:

How To setup PingFederate in Keycloak with OIDC
-----------------------------------------------

This guide explains how to configure PingFederate as an Identity Provider (IdP) in Keycloak.

.. note::
  This guide assumes that you have access to the :ref:`Authentication Admin Console <auth-admin-console>` and to your PingFederate instance.

.. note::
   You need the discovery info endpoint URL from your PingFederate instance. This URL is used to configure the IdP auto-discovery in Keycloak.
   Following this `guide <https://docs.pingidentity.com/pingfederate/latest/developers_reference_guide/pf_oauth_authorization_server_metadata_endpoint.html>`__ the URL should be ``https://<PINGFEDERATE_HOST>/.well-known/openid-configuration``.


.. _idp-ping-federate-keycloak-configuration:

**Client and Keycloak Configuration**

To configure PingFederate as an IdP in Keycloak, you need to go to the :ref:`Authentication Admin Console <auth-admin-console>` and follow these steps:

    * Go to :menuselection:`Identity Providers` > :menuselection:`Add provider` > :menuselection:`OpenID Connect v1.0`.
    * Select an alias for the IdP, for example, ``PingFederate`` and copy the redirect URL.
    * Open a new page and go to your PingFederate console.

Next, we need to create a new OIDC client in PingFederate, you can follow this `guide <https://docs.pingidentity.com/solution-guides/customer_use_cases/htg_oidc_app_setup_pf.html>`__ to create a new client.
Feel free to configure the settings as needed, however, it is recommended to use the following parameters:

.. csv-table::
    :header: "Field", "Value"

    "Client Authentication", "Client Secret",
    "Allowed Grant Types", "Select at least Authorization Code",
    "Scopes", "At least **openid** and **profile**",
    "Redirection URI", "The redirect URL from Keycloak"

.. note::
    Other parameters have to be configured depending on your own setup.

Now you can copy the client id and the secret, and put them in the Keycloak configuration. After that, simply click on :menuselection:`Save`.

Next step is adding a new mapper in :menuselection:`Mappers` with the following settings:

* **Name**: ``username``
* **Mapper Type**: ``Attribute Importer``
* **Attribute Name**: ``sub``
* **User Attribute Name**: ``username``

.. note::
    The ``Attribute Name`` **MAY** be different for you, refer to your claim names to make sure. In this configuration we used ``sub`` that is the default with **profile** scope

At this point, you can complete the rest of the configuration with common settings, please refer to :ref:`Common IdP configuration <idp-common-config>` and :ref:`IdP After Setting up configuration <idp-after-config>` sections to find all the information you need.
