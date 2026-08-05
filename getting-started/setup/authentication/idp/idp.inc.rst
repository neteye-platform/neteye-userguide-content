.. _idp-configuring:

External Identity Providers
~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section describes how to configure an external identity provider (IdP) in |ne|.

Identity Providers are services that create, maintain, and manage identity information for users
while providing authentication services to relying applications within a federation or distributed
network. If you have an existing IdP, you can configure it in |ne| to authenticate users with it
(via SAML), defined in :rfc:`7522` or OIDC defined in the `OpenID Connect Core 1.0 specification.
<https://openid.net/specs/openid-connect-core-1_0.html>`_ Some popular options for that are: Ping
Identity, Fortinet or MS ADFS. This page also shows examples of how to configure some of the most
common IdPs.

When an IdP is enabled, Keycloak redirects the user to the IdP login page based on the email address' domain.


.. note::
 If LDAP is enabled and provides the user's email address, the user can insert only the username at the login.


.. _idp-idp-in-neteye:

Add an IdP in |ne|
``````````````````

To add an IdP in |ne|, access the :ref:`Authentication Admin Console <auth-admin-console>` and
click on the :menuselection:`Identity Providers > Add provider` menu item. Many already configured
IdPs can be easily set up by clicking a button under the `Social` section, or a custom IdP can be
configured by selecting a protocol under the `User defined` tab.


.. note::
 Keycloak supports SAML v2.0, OIDC v1.0, and OAuth 2.0.

After clicking a button, a `wizard` will guide you through the configuration of the IdP.
You will also need access to the IdP admin console to obtain the necessary information to configure the IdP in |ne|, and accept |ne| as a client.


Specific IdP How-to Guides
``````````````````````````

* How to configure :ref:`MSADFS <idp-msadfs-saml>`
* How to configure :ref:`FortiAuthenticator <idp-fortinet-oidc>`
* How to configure :ref:`PingFederate <idp-ping-federate-oidc>`

.. _idp-oidc:

OpenID Connect (OIDC)
`````````````````````
Keycloak supports the auto-discovery of OIDC configuration from the IdP. The URL:
``https://{YOUR-IDP-HOST}/.well-known/openid-configuration`` can be used to configure the IdPs
`discovery endpoint` inside the Keycloak configuration form. If you want to configure the IdP
manually, you can follow the official
`OIDC Configuration documentation. <https://www.keycloak.org/docs/25.0.6/server_admin/#_identity_broker_oidc>`_


.. _idp-saml:

Security Assertion Markup Language (SAML)
`````````````````````````````````````````
Keycloak supports the SAML identity descriptor file (metadata) from the IdP. If you want to
configure the IdP manually, you can follow the official `SAML Configuration <https://www.keycloak.org/docs/25.0.6/server_admin/#saml-v2-0-identity-providers>`_
documentation.

.. _idp-common-config:

Common IdP Configuration
````````````````````````
.. csv-table:: Configuration fields

 "Alias", "The alias is a unique identifier for an identity provider and references an internal identity provider. Keycloak uses the alias to build redirect URIs for OpenID Connect protocols that require a redirect URI or callback URL to communicate with an identity provider. All identity providers must have an alias. Examples of aliases include Facebook, Google, and idp.acme.com."
 "Hide on Login Page", "**MUST** be set to `true` because we have the IdP auto-selection based on email address"
 "Account Linking Only", "**SHOULD** be set to `true`"
 "Store tokens", "**MUST** be set to `false`"
 "Store Token Readable", "**MUST** be set to `false`"
 "Trust Email", "**MUST** be set to `true`"
 "Sync Mode", "**SHOULD** be set to `force` because you have to trust the IdP."
 "Case-sensitive username", "**SHOULD** be set to `true`"

See the `official guide of Keycloak <https://www.keycloak.org/docs/latest/server_admin/#_general-idp-config>`__ if you need particular configurations.

.. _idp-domains:

Configuring IdP Domains
```````````````````````

For the **Home IdP Discovery** to correctly map the email domain to the proper login endpoint, the IdP
domains need to be configured. This step ensures that Keycloak properly redirects to the IdP for authentication, requiring only the username as the initial step of the login process. Additionally, if multiple IdPs are configured in Keycloak, setting this parameter allows Keycloak to determine the correct IdP to redirect to based on the email domain associated with the entered username.

Since that configuration option is currently only available over a REST-API, we
provide a :command:`neteye` subcommand to make these configuration as easy as possible. With the
:ref:`neteye-config-auth-idp` command, you can view and edit the domain configuration with the
subcommands :command:`list` and :command:`set` respectively.

To activate the flow, you then also need to follow the configuration steps below.


.. _idp-after-config:

Enabling the Home IdP Discovery Flow
````````````````````````````````````

After setting up the IdP, you must change the default ``Authentication Flow`` on Keycloak.
To do this, you need to access the `Authentication` section in the :ref:`Authentication Admin Console <auth-admin-console>` and:

* Click on the `neteye-idp-discovery-flow` flow
   * Click on the `Dots menu` button and select `Bind flow`
   * Select `Browser` and click on `Save`

* Click on the `neteye-first-broker-login-flow` flow
   * Click on the `Dots menu` button and select `Bind flow`
   * Select `First Login Flow` and click on `Save`


.. _idp-auto-redirector:

Configuring Identity Provider Redirector step
`````````````````````````````````````````````

If you have only one Identity Provider (IdP), you can configure an automatic redirect to it, bypassing the login screen for users. This will allow users to be redirected directly to your custom IdP.

To enable this:

* Go to the `Authentication`` section in the :ref:`Authentication Admin Console <auth-admin-console>`.
* Select `neteye-idp-discovery-flow`.
* Enable the "Identity Provider Redirector" step by setting its requirement to `Alternative`.
* Click the gear icon next to the step, choose an alias, and enter the alias of your IdP in the `Default identity provider` field.

Once this is configured, users will no longer see the login screen and will be automatically redirected to the specified IdP.

If you need to log in with a local user, visit: `https://{YOUR-HOST}/local_auth`

.. note::
   This step is optional and not required for all configurations.
