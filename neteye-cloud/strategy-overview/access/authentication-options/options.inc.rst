
Authentication Options
~~~~~~~~~~~~~~~~~~~~~~

Depending on your organization's setup, authentication to
NetEye.Cloud works in one of two ways:

.. list-table::
   :header-rows: 1
   :widths: 30 35 35

   * - Scenario
     - Who verifies your identity?
     - Who manages your access?
   * - With a customer Identity Provider
     - Your organization's IdP
     - Your organization (if Group Claims are enabled)
       or NetEye.Cloud Team (if not)
   * - Without an Identity Provider (local account)
     - NetEye.Cloud platform
     - NetEye.Cloud Team

**With a customer Identity Provider** — Your organization operates
its own IdP (for example, Microsoft Entra ID or any other provider
that supports the OpenID Connect protocol). Authentication is
delegated entirely to your IdP as described in
:ref:`authentication-via-idp`. NetEye.Cloud never sees or stores
your password.

.. note::

   Microsoft Entra ID is the most common Identity Provider used with
   NetEye.Cloud, but it is not the only supported option. Any IdP
   compatible with the OIDC protocol can be integrated.

**Without a customer Identity Provider (local account)** — If your
organization does not operate a supported Identity Provider,
authentication is handled through a **local NetEye.Cloud account**.

In this scenario:

* The NetEye.Cloud Team creates and manages your account credentials
  directly within the platform.
* You log in on the NetEye.Cloud Login Page using the credentials
  provided to you by the NetEye.Cloud Team.
* Both **authentication and authorization** are managed entirely by
  the NetEye.Cloud Team.

.. tip::

   If your organization plans to adopt an Identity Provider in the
   future, the NetEye.Cloud Team can assist with the migration from
   local accounts to IdP-based authentication at any time.


Requesting Access Changes
~~~~~~~~~~~~~~~~~~~~~~~~~

In certain configurations, user access to NetEye.Cloud is managed by
the NetEye.Cloud Team rather than by your organization directly.
This applies when:

* You use a **local NetEye.Cloud account** (no Identity Provider).
* You authenticate via an IdP but **Group Claims are not enabled**,
  so authorization is delegated to the NetEye.Cloud Team.

In both cases, any of the following changes must be requested through
the NetEye.Cloud service request process:

* Enabling or disabling a user account
* Changing a user's access level or role
* Adding or removing access to specific NetEye.Cloud services

`Open a NetEye.Cloud service request
<https://siwuerthphoenix.atlassian.net/servicedesk/customer/portal/13/group/34/create/188>`_

.. note::

   If your organization authenticates via an IdP **with Group Claims
   enabled**, you can manage all of the above autonomously by
   changing group memberships in your own Identity Provider. No
   service request is needed.
