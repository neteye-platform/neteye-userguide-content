
Base Concepts
-------------

Authentication
~~~~~~~~~~~~~~

**Authentication** is the process of verifying that a user is who they
claim to be. When you log in to |nec|, the platform checks your
credentials — such as your email and password or a corporate single
sign-on (SSO) session — against a trusted identity system before
granting access.

In practice, this means:

* **You** provide your credentials (e.g. email address and password).
* **A trusted identity system** confirms that those credentials are
  valid and belong to a real, authorized account.
* |necb| accepts the confirmation and lets you in — or
  denies access if verification fails.

In most cases, |nec| delegates the `verification step <cloud-group-claims>`
to an external **Identity Provider (IdP)** — a dedicated service that your
organization uses to manage user accounts and credentials (for example,
Microsoft Entra ID). In this scenario, |nec| never sees or stores
your password.

If your organization does not operate a supported Identity Provider,
authentication is handled through a local |nec| account managed
by the |nec| team. In this case, your credentials are stored
within the platform's built-in identity service.

.. note::

   If your organization does not operate a supported Identity Provider,
   authentication can be handled through a local |nec| account
   managed by the |nec| Team. In this case, any changes to user
   access must be requested through the
   `NetEye.Cloud service request process
   <https://siwuerthphoenix.atlassian.net/servicedesk/customer/portal/13/group/34/create/188>`_.

The sections that follow describe how the authentication flow works in
detail and how to configure your Identity Provider for use with |nec|.


How Authentication Works
~~~~~~~~~~~~~~~~~~~~~~~~

When you log in to |nec|, the platform processes your request
through several steps before granting access. The diagram below
summarizes the flow; the paragraphs that follow describe each step in
detail.

.. note::

   You do not need to install or configure anything for this flow to
   work. It is handled entirely by the |nec| platform once the
   initial Identity Provider setup has been completed during onboarding.

#. **You open the** |necb| **Login Page** and enter your email
   address.

#. |necb| **routes the request through its authentication
   broker.** Internally, the platform uses `Keycloak
   <https://www.keycloak.org/>`_ — an open-source identity and access
   management service — as a central broker for all login requests.
   Keycloak inspects the email domain you provided and determines
   which Identity Provider is responsible for your organization.

#. **The request is forwarded to your Identity Provider.** Keycloak
   redirects you to your organization's IdP (for example, Microsoft
   Entra ID). You authenticate there using your corporate credentials
   — |nec| never sees or stores your password.

#. **Your Identity Provider responds.** After successful
   authentication, the IdP issues a security token back to Keycloak.
   This token confirms your identity and, if configured, includes
   group-membership claims that |nec| can use for
   authorization.

#. |necb| **validates the token.** Keycloak verifies that the
   token is authentic, has not expired, and that your account is
   authorized to access |nec| services. If any of these checks
   fail — for example, the token is invalid, your account is not
   recognized by the IdP, or no valid authorization information is
   present — access is denied.

#. **Access is granted.** Once validation succeeds, you are logged in
   and can use the |nec| services included in your subscription.

When Access Is Denied
~~~~~~~~~~~~~~~~~~~~~

|nec| denies access when any of the following conditions occur during the
authentication process:

* **Authentication failure** — your Identity Provider could not
  verify your credentials (e.g. wrong password, expired account).
* **Unverifiable token** — the security token was issued by a
  known Identity Provider, but |nec| cannot validate it.
  This typically means the token's signature does not match the
  expected certificate, the signing algorithm is unsupported, or
  the token has been tampered with in transit.
* **Unrecognized Identity Provider** — the token's issuer does
  not match any Identity Provider configured in |nec|.
  This can occur if the IdP was never set up, was removed, or if
  its issuer identifier changed after the initial configuration.
* **No valid** |necb| **profile** — your account is not
  recognized as an authorized |nec| user.
* **Missing or insufficient group membership** — when Group Claims
  are enabled, your account does not belong to any group that is
  mapped to a |nec| role.

In all cases, you will be redirected to an error page. If you
believe your access was denied in error, contact your organization's
IdP administrator or open a request through the
`NetEye.Cloud service request process
<https://siwuerthphoenix.atlassian.net/servicedesk/customer/portal/13/group/34/create/188>`_.
