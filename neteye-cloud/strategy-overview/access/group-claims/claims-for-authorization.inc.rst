
.. _group-claims-for-authorization:

Group Claims for Authorization Management
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When your Identity Provider authenticates you, it returns a security
token to NetEye.Cloud. This token can optionally include **Group
Claims** — information about which groups your account belongs to in
your organization's directory.

Group Claims allow NetEye.Cloud to determine what you are authorized
to access **automatically**, based on your group memberships.


**With Group Claims enabled (self-managed authorization)**

* Your IdP token includes group-membership information.
* NetEye.Cloud maps these groups to internal roles and permissions.
* Your organization manages access **autonomously** — granting or
  revoking access to NetEye.Cloud services by simply changing group
  memberships in your own IdP.
* No interaction with the NetEye.Cloud Team is required for routine
  access changes.


**Without Group Claims (delegated authorization)**

* Your IdP still handles authentication — your credentials remain
  under your organization's control.
* However, **authorization** is managed by the NetEye.Cloud Team on
  your behalf, since NetEye.Cloud has no way to read your group
  memberships.
* Any changes to user access must be requested through the
  `NetEye.Cloud service request process
  <https://siwuerthphoenix.atlassian.net/servicedesk/customer/portal/13/group/34/create/188>`_.

.. note::
   Including Group Claims is recommended for organizations that want
   full control over authorization management without operational
   dependency on the NetEye.Cloud Team.

.. note::
   For step-by-step instructions on configuring Group Claims in
   Microsoft Entra ID, see
   :ref:`authentication-via-microsoft-entra-id`.
   If you use a different Identity Provider, consult your IdP's
   documentation on including group claims in OIDC tokens.