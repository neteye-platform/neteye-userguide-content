.. _idp-fortinet-oidc:

How To setup FortiAuthenticator in Keycloak with OIDC
-----------------------------------------------------

This guide explains how to configure FortiAuthenticator (6.6.2) as an Identity Provider (IdP) in Keycloak.

.. note::
  This guide assumes that you have access to the :ref:`Authentication Admin Console <auth-admin-console>` and to your FortiAuthenticator console.

.. note::
   You need the discovery info endpoint URL from your FortiAuthenticator instance. This URL is used to configure the IdP auto-discovery in Keycloak.
   Following this `guide <https://docs.fortinet.com/document/fortiauthenticator/6.6.2/rest-api-solution-guide/33269/oidc-connect-discovery-info>`__ the URL should be ``https://<FORTIAUTHENTICATOR_HOST>/api/v1/oauth/.well-known/openid-configuration/``.


.. _idp-forti-keycloak-configuration:

**Client and Keycloak Configuration**

To configure FortiAuthenticator as an IdP in Keycloak, you need to go to the :ref:`Authentication Admin Console <auth-admin-console>` and follow these steps:

    * Go to :menuselection:`Identity Providers` > :menuselection:`Add provider` > :menuselection:`OpenID Connect v1.0`.
    * Select an alias for the IdP, for example, ``Fortinet`` and copy the redirect URL.
    * Open a new page and go to your FortiAuthenticator console.

Next, we need to create a new OIDC client in FortiAuthenticator, you can follow this `guide <https://docs.fortinet.com/document/fortiauthenticator/6.6.2/administration-guide/796040/relying-party>`__ to create a new client.
Feel free to configure the settings as needed, however, it is recommended to use the following parameters:

.. csv-table::
    :header: "Field", "Value"

    "Client type", "confidential",
    "Authorization grant types", "Authorization Code",
    "Scopes", "openid. You can read this `guide <https://docs.fortinet.com/document/fortiauthenticator/6.6.2/administration-guide/154496/scopes>`__ to create scopes",
    "Claims", "With openid you need to add at least one claim: the username. You can read this `guide <https://docs.fortinet.com/document/fortiauthenticator/6.6.2/administration-guide/796040/relying-party#Claims>`__ to create claims",
    "Redirect URIs", "The redirect URL from Keycloak"

.. note::
    Other parameters have to be configured depending on your own setup.

Now you can copy the client id and the secret, and put them in the Keycloak configuration. After that, simply click on :menuselection:`Save`.

Next step is adding a new mapper in :menuselection:`Mappers` with the following settings:

* **Name**: ``username``
* **Mapper Type**: ``Attribute Importer``
* **Attribute Name**: ``**username**``
* **User Attribute Name**: ``username``

.. note::
    The ``Attribute Name`` **MAY** be different for you, refer to your claim names to make sure. In this configuration we used ``username``.

At this point, you can complete the rest of the configuration with common settings, please refer to :ref:`Common IdP configuration <idp-common-config>` and :ref:`IdP After Setting up configuration <idp-after-config>` sections to find all the information you need.
