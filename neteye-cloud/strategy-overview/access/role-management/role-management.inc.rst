.. _neteye-cloud-role-management:

Roles and Permissions
---------------------

|nec| uses role-based authorization to determine which modules
users can access and which operations they can perform. Authentication
verifies a user's identity; authorization grants the permissions needed
for that user's tasks. Signing in through Single Sign-On (SSO) does not
automatically grant access to every module or to all data.

Roles, Permissions and Scope
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A user's role combines a **contract type** with an **access level**,
within the scope of the user's **company/tenant**:

* **Contract type** identifies a subscribed service or a defined subset
  of its functionality, such as monitoring (MON) or the Elastic Stack
  (ELK).
* **Access level** describes the user's responsibilities within that
  contract type: Viewer, Editor or Admin.
* **Permissions** are the module-specific operations granted by the
  platform for that contract type and access level.
* **Tenant restrictions** limit those operations to the company's own
  data. No access level, including Admin, overrides this boundary.

A user can have different access levels for different contract types,
but only one access level is allowed per contract type. For example,
a user may be a Viewer for MON and an Editor for ELK.

Access Levels
~~~~~~~~~~~~~

.. list-table::
   :header-rows: 1
   :widths: 20 80

   * - Access level
     - General capabilities
   * - Viewer
     - Views information, such as monitoring data and performance
       graphs, within the user's tenant and contract type.
   * - Editor
     - Includes Viewer capabilities and allows editing dashboards and
       other elements that are not configurations or settings.
   * - Admin
     - Includes Editor capabilities and allows managing data sources
       and editing configurations. This is a module administrator,
       not a full system administrator.

Not every contract type supports all three levels. The exact permissions
and available levels depend on the subscribed service. Unsupported
contract/access-level combinations default to Viewer, as described in
:ref:`authorization-procedure`. Consult that section for the current
contract-specific availability table before assigning a role.

Who Manages Roles?
~~~~~~~~~~~~~~~~~~

**With an Identity Provider and Group Claims enabled**, your
organization manages role assignments through group memberships in its
own Identity Provider (IdP). During login, the IdP supplies those groups
in the authentication token, and |nec| maps recognized groups to
the configured contract types, access levels and module permissions.
Routine access changes through existing mapped groups do not require
a service request.

Group names must match the mappings agreed with the |nec| Team.
Creating or renaming a group in the IdP alone does not create a new role
or change the platform's permissions. |nec| controls the mapping
and grants access only within the company's active contracts and tenant.
Unrecognized group claims are silently ignored and grant no permissions.

**Without Group Claims, or when using local** |nec| **accounts**,
the |nec| Team manages authorization on your behalf. Requests to
change roles or add or remove service access must be submitted through
the |nec| `service request process
<https://siwuerthphoenix.atlassian.net/servicedesk/customer/portal/13/group/34/create/188>`_.

For more information about these management options, see
:ref:`group-claims-for-authorization`.

Managing Role Assignments
~~~~~~~~~~~~~~~~~~~~~~~~~

In order to manage your role assignments:

#. Identify the subscribed contract types the user needs for their work.
#. Choose the lowest available access level that meets those needs,
   assigning only one level for each contract type.
#. For self-managed authorization, add or remove the user's membership
   in the corresponding mapped IdP groups. Coordinate any new group
   mapping with the |nec| Team. For delegated authorization,
   request the change through the service request process.
#. Have the user log out and log back in, then verify that the expected
   modules, data and operations are available.

.. note::

   Permissions are applied during login. Role or group-membership
   changes require logging out and logging back in to take effect;
   they should not be assumed to update an existing session immediately.

If expected permissions are missing, check that the user's group claims
exactly match the configured group names and that the requested access
level is available for the contract type. The detailed mapping process
and tenant restrictions are described in :ref:`authorization-procedure`.
