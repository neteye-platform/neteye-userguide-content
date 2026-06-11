Identity and Access Management
------------------------------

.. _roles-users-permissions:

User authentication is managed by **Keycloak** that is the component responsible for the identity and access management.
It provides user federation, strong authentication, user management, fine-grained authorization, and many other features.

Here's a diagram of the authentication flow in |ne|:

.. _figure-keycloak-flow-architecture:

.. figure:: /getting-started/setup/img/keycloak-flow-architecture.svg

#. The login to |ne| goes through the login site of Keycloak. It will match your username to the
   right identity provider backend. If the username does not match any known IdP, it will lookup
   the user in the local users and on the federated systems.

#. If your username matches a known authentication provider, you will be redirected to their site
   for login.

#. Once the login succeeded, the external identity provider will redirect you to Keycloak with a
   authentication token, that will then in Keycloak be exchanged for a Keycloak Oauth token.

#. This Oauth token will then be used by the icingaweb2 Oauth backend to create the icingweb2
   session.

#. The user in icingaweb2 will then be mapped to the correct roles, assigning your user the
   permissions and restrictions.

#. Finally you are logged in and have access to the functionalities of |ne| configured for you.

If you need a reverse proxy in front of |ne|, please refer to this how-to:
:ref:`How to setup a reverse proxy <howto-networking-proxy>`

.. _auth-admin-console:

Authentication Admin Console
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

All the authentication settings can be managed from the Keycloak admin console
that can be accessed from the |ne| web interface by clicking on the :menuselection:`Configuration > Authentication` menu item.

.. note::
   By default only the root user has access to the Keycloak admin console.
   The password for the root user is saved in the file `/root/.pwd_icingaweb2_root` unless it has been previously changed.

.. warning::
   DO **NOT** DELETE OR EDIT the user `neteye-internal-keycloak-admin`. It is
   used by NetEye itself to manage the Keycloak configuration.

To give access to the authentication admin console to another user refer to the dedicated section: :ref:`realm roles <auth-realm-roles>`.


.. _auth-create-local-user:

Create a local user
```````````````````

Users can either be managed locally or be read from an external source like LDAP. Local users are
often used to manage |ne| service accounts, e.g. for agents to login over the SSO, clients for
automation that need restricted access to a small part of |ne| or any accounts that don't need to
be managed by external or federated identity providers.

.. note::
   The root user is a local user whose privileges are escalated to the admin role.

Use the following procedure to create a local user:

- Access the authentication admin console :menuselection:`Configuration > Authentication`
- Click on :menuselection:`Users > Add user`
- Fill at least the `Username` field
- Click on :menuselection:`Create`
- Set password for the user in :menuselection:`Credentials` tab

Now the user can be assigned to a group at :menuselection:`Groups` tab and be granted necessary permissions at :menuselection:`Role Mappings` tab.


.. _auth-create-local-group:

Create a local group
````````````````````

Similar to the users, the groups can be read from an external source like LDAP or be created
locally. As with the users, local groups can be used to manage privileges specific to users in
|ne| without accessing the external or federated identity providers.

To create a local group

- Access the authentication admin console :menuselection:`Configuration > Authentication`
- Click on :menuselection:`Groups > Add group`
- Fill the `Name` field
- Click on :menuselection:`Create`

Now one or multiple users can be assigned to the group at :menuselection:`Members` tab and obtain required permissions at :menuselection:`Role Mappings` tab.


.. _auth-realm-roles:

Realm roles
```````````

For any neteye user to be able to manage other users and other authentication related configuration,
they need to be assigned the right Realm roles inside of Keycloak. This also holds for all users
that have administrator level access in the rest of |ne|.

On a new installation, there are two users preconfigured with the necessary permissions: The `root`
user is the administrator of all of |ne| and therefore has all privileges, also inside of Keycloak.
The user `neteye-internal-keycloak-admin` instead is a user that has only permissions inside of
Keycloak, but no other access to other |ne| components. We do not recommend to use this account for
user management, but use a dedicated account for that.

While the before mentioned users both have administrative access, Keycloak also allows for more
finegrained access control to the user configuration over the Realm roles. Some of the most
commonly use Roles are:

- `admin` - This role has full access to the authentication admin console like the root user
- `view-users` - It allows to view the users and groups in the realm
- `manage-users` - It allows to manage the users and groups in the realm

For a complete list of the Realm roles refer to the `Keycloak documentation <https://www.keycloak.org/docs/25.0.6/server_admin/#master-realm-access-control>`_.
