
The actions that a |ne| user can perform on the Alyvix module are based
on the user :ref:`role <alyvix-roles>`, defined by the |ne| user permissions. The roles are assigned based on the
user's IcingaWeb2 permissions as follows:

.. _alyvix-super-admin-configuration:

Super Admin
```````````

The privilege of a Super Admin role is granted to users with the **Unrestricted Access**, **Administrative Access** or
**Full Module Access** permission on the Alyvix module. Please note that, to be able to see the configured Alyvix nodes,
the user needs to have permissions to see them in the `Icinga DB` module.

.. image:: /feature-modules/alyvix/img/alyvix-super-admin-permissions.png
  :width: 49 %
.. image:: /feature-modules/alyvix/img/alyvix-icingadb-permissions.png
  :width: 49 %

.. _alyvix-tenant-admin-configuration:

Tenant Admin
````````````

The privilege of a Tenant Admin role is granted to users with the **General Module Access** and **alyvix/tenant-admin**
permissions, together with an **alyvix/tenant** restriction. For example, consider the case of a NetEye environment with
a `tenantA` tenant. If you would like to grant a set of users the `tenantA` Tenant Admin role for Alyvix, you should
:ref:`enable <neteye-tenant-config-modify>` the Alyvix module for tenant `tenantA` and configure a role
in the `Access Control` as follows:

#. Inherit the `neteye_tenant_tenantA` role
#. Enable the **General Module Access** for Alyvix
#. Enable the **alyvix/tenant-admin** permission for Alyvix
#. Enable the **General Module Access** for the Icinga DB (possibly restricting the hosts visible by the user)

.. image:: /feature-modules/alyvix/img/alyvix-tenant-admin-inherit-permissions.png
  :width: 49 %

.. image:: /feature-modules/alyvix/img/alyvix-tenant-admin-permissions.png
  :width: 49 %

.. _alyvix-tenant-viewer-configuration:

Tenant Viewer
`````````````

The privilege of a Tenant Viewer role is granted to users with the **General Module Access** and **alyvix/tenant-viewer**
permissions, together with an **alyvix/tenant** and **alyvix/viewer-tags** restrictions. For example, consider the case
of a NetEye environment with a `tenantA` tenant. If you would like to grant a set of users the `tenantA`
Tenant Viewer role for Alyvix, you should :ref:`enable <neteye-tenant-config-modify>` the Alyvix module for tenant
`tenantA` and configure a role in the `Access Control` as follows:

#. Inherit the `neteye_tenant_tenantA` role
#. Enable the **General Module Access** for Alyvix
#. Enable the **alyvix/tenant-viewer** permission for Alyvix
#. If needed, define the **alyvix/viewer-tags** restriction for Alyvix as a comma-separated list of tags.
   This restriction allows the user to see only the Test Cases associated with the tags specified in their
   role configuration. If no tags are defined, the user can see all the Test Cases of the tenant.
   To link Test Cases with the relevant tags, multiple tags can be assigned through the dedicated Test Case
   settings in the :ref:`alyvix-test-case-general-tab`
#. Enable the **General Module Access** for the Icinga DB (possibly restricting the hosts visible by the user)

.. image:: /feature-modules/alyvix/img/alyvix-tenant-viewer-inherit-permissions.png
  :width: 49 %
.. image:: /feature-modules/alyvix/img/alyvix-tenant-viewer-permissions.png
  :width: 49 %

.. note::

   In case you would like to grant `Tenant Admin/Viewer` access on more than one tenant to a set of users, please repeat the
   procedure above for all the implied tenants.
