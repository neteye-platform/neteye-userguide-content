.. _advanced-configuration:

Advanced Configuration
~~~~~~~~~~~~~~~~~~~~~~

Authentication backends
```````````````````````

Authentication backends are sources that provide user credentials or user group membership.
They are configured in the configuration file :file:`/neteye/shared/icingaweb2/conf/authentication.ini`.
By default there are two authentication backends configured:

- `neteye-jwt` that is the backend used internally by NetEye to authenticate internal users via JWT
- `keycloak-master` that is the backend used to authenticate the users against Keycloak using the OpenID Connect Core 1.0 protocol

Different authentication backends can be configured and they are consulted in the order listed in the tables shown in the figure below.

.. note::
   The `keycloak-master` backend must always be the last one in the list.

.. warning::
   Do not remove or edit the keycloak-master backend under any circumstances to ensure the correct functioning of the SSO.


JWT Authentication Backend
``````````````````````````

In this section, we describe a custom :ref:`authentication backend
<advanced-configuration>` which enables authenticating users via `JSON Web
Tokens <https://www.jwt.io>`__ (JWT). JWT is an open industry standard
defined in :rfc:`7519` that defines a compact and self-contained way
for securely transmitting information between parties as a JSON
object.

This backend can be used to authenticate users via an external entity
(SSO) e.g. a centralized company login portal, outside of NetEye.

When the JWT authentication backend is enabled and the
``Authorization Header`` with the JWT token is included in the request,
NetEye will validate the JWT token using the previously configured
Public Key. If this is successful, the user defined in the token will be
logged in with the provided username.

To configure the JWT Authentication backend, you need to add the JWT
authentication backend in the
``/neteye/shared/icingaweb2/conf/authentication.ini``. For example:

.. container:: codeblock

   .. code::

      [jwt_example]
      backend = "jwt"
      hash_algorithm = "RS256"
      pub_key = "jwt-example-pubkey.pem"
      username_format = "${sub}"
      expiration_field = "exp"

- ``backend``: is always ``jwt`` for this backend
- ``hash_algorithm``: an algorithm for the verification of the token
  to be chosen between ``RS256``, ``RS384``, and ``RS512``
- ``pub_key``: the public key used to verify the JWT token

  - the public key must be located in
    ``/neteye/shared/icingaweb2/conf/modules/neteye/jwt-keys/``
  - if you have a pem certificate, you may need to extract the public
    key with the following command::

      openssl x509 -pubkey -noout -in jwt-example-cert.pem > jwt-example-pubkey.pem

- ``username_format``: this variable can be used to configure the
  key(s) of the JWT payload to be used as the username of the
  users to be authenticated.

  - this supports variable extraction from the JWT payload by using the
    following pattern: ``$key``.

- ``expiration_field``: define the variable if you want to use a custom
  expiration field for the JWT payload. The default value is ``$exp``.

Let’s suppose the following JWT token payload:

.. container:: codeblock

   .. code:: json

      {
        "sub": "user1",
        "domain": "example.com",
        "exp": "0123456789"
      }

I want to authenticate the user with the following username:
“user1.example.com”, using the verification algorithm RSA Signature with
SHA-256, and the public key
``/neteye/shared/icingaweb2/conf/modules/neteye/jwt-keys/my-jwt-pubkey.pem``
and the field expiration taken from ``exp`` the config looks like:

.. container:: codeblock

   .. code::bash
        [jwt_example]
        backend = "jwt"
        hash_algorithm = "RS256"
        pub_key = "my-jwt-pubkey.pem"
        username_format = "${sub}.${domain}"
        expiration_field = "exp"

The user authorization (e.g., mapping a user with one or more module’s
permissions or restrictions) is managed as already described in the
:ref:`user authorization <auth-roles>` section.


Request Header Authentication Backend
`````````````````````````````````````

In this section, we describe a custom :ref:`authentication backend
<advanced-configuration>` which enables authenticating users via a **Request Header**
that contains the username of the user to log in. The prerequisite
is that also a valid certificate is sent with the request for the header
to be considered.

This backend can be used to authenticate users via an external (SSO) entity,
like e.g., an F5 proxy.

When the Request Header authentication backend is enabled, NetEye searches for
the ``X-REMOTE-USER`` HTTP header only for the requests that are using
a client certificate signed by one of the :ref:`CAs trusted by httpd <neteye-https-conf>`.
If found, then NetEye checks if the certificate has an allowed ``Common Name``.
If everything looks fine, the user defined in the header will be logged in with
the provided username.

The client certificate that must be used for the requests has to be provided by
the manager of the CA used by httpd for the validation.

To configure the Request Header Authentication backend, you need to configure it
in the file :file:`/neteye/shared/icingaweb2/conf/authentication.ini`. For example:

.. container:: codeblock

   .. code::

      [request_header_example]
      backend = "request-header"
      authorized_common_names = [ "..." ]

- ``backend``: is always ``request-header`` for this backend
- ``authorized_common_names``: a list of authorized common names for this backend

Let’s suppose a request with the header *X-REMOTE-USER: user1.example.com* is sent,
using a certificate with the *neteye-sso.example.com* Common Name, the config
should look like:

.. container:: codeblock

   .. code::

      [request_header_example]
      backend = "request-header"
      authorized_common_names = [ "neteye-sso.example.com", "other-allowed-cn" ]

The user authorization (e.g., mapping a user with one or more module’s
permissions or restrictions) is managed as already described in the
:ref:`user authorization <auth-roles>` section.
