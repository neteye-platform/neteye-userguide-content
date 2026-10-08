.. _satayo-getting-started-registration:

Getting Started
~~~~~~~~~~~~~~~

Registration
============

User registration for |sat| is managed through the service provider (**Würth IT**).

Initial user accounts are typically provisioned together during the onboarding of the organization.

.. note::
   User accounts are provisioned **globally at the** |neb| **platform level** rather than
   specifically for |sat|. Because |sat| operates as an integrated feature module
   within |nec|, your |nec| user account provides access across relevant
   modules according to your assigned permissions.

Registration is one of the preparatory steps in the onboarding procedure. To register new
or additional users after onboarding, contact Würth IT `support <https://servicedesk.wuerth-it.it>`__.

When requesting new user accounts, provide the following required information for each user:

* **First Name**
* **Last Name**
* **Email Address**
* **Organization Name**

.. note::
   Additional user profile attributes may be requested depending on system and organizational requirements.

For an organization that is already registered, the customer-side contact
person can use the |ne| `user registration page
<https://satayo.cloud/admin_users.php>`__ to provide the user details.

Additional users must always be requested through Würth IT. The |sat|
`access page <https://satayo.cloud/index.php>`__ is used to access an
already registered organization; it does not replace the registration request
to the service provider.

Once the registration request is processed by the service provider, each user receives
a confirmation email containing instructions on how to complete their account setup and to access |ne|.

IP Whitelisting
===============

The whitelist of the network :command:`82.193.25.0/24` is requested. Whitelisting this network,
which is used to manage active scanning activities and that is
`managed directly <https://apps.db.ripe.net/db-web-ui/lookup?source=ripe&key=82.193.25.0%20-%2082.193.25.255&type=inetnum>`_
by Wurth IT Italy, is strongly recommended to allow SATAYO to obtain more consistent information about the services exposed
on the infrastructure being analyzed.
