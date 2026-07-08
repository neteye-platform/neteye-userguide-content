.. _glpi-permissions:

Permissions
-----------

Access to GLPI from the NetEye GUI is granted by permissions of a particular
user role. In order to create a role with mentioned permissions, go to the
Assetmanagement module in **Configuration** > **Access control** > **Roles**,
where you can set suitable permissions and restrictions.
It is recommended to inherit role properties from the default role ``neteye_tenant_master``.
This existing role should never be modified since it has all the GLPI Entity configurations.

The **profile and entities** in GLPI of users must be mapped correctly in the
NetEye (**Configuration** > **Access Control** > **Roles**) to persist
across login/logout otherwise the **GLPI profile and entity** will be lost
as soon as the user logged out from NetEye.

Each NetEye role corresponds to a unique combination of GLPI
recursive profile/entity. For example, if a user belongs to more than
one entity, or has different profile inside GLPI, he should belong to
multiple NetEye roles.

Note that, if the GLPI user role will inherit the ``neteye_tenant_master`` role
properties, the already configured GLPI Entity ``Root entity > master`` will be
used without additional configuration steps.

All entities and profiles must be created before users login
for having a success permission synchronization. The only exceptions to
this are the **Root entity** and the default GLPI profiles. If the
**profile/entities** does not exist for the users in GLPI, then the mapping
between |ne| and GLPI will not be successful.

Note that if you need to investigate on what happens during the
permissions synchronization (e.g. for debugging purposes), you can have
a look at the following logfile, in which are logged all the actions
performed during the permissions synchronization:

.. code::

  /neteye/shared/glpi/data/_log/php-error.log


All the log messages printed during the SSO will be prefixed with
*GLPI-Plugin-Icingaweb2SSO*.

.. _glpi-special-permissions:

Special Cases
~~~~~~~~~~~~~

There exist two special cases, with pre-defined triple
recursive-profile-entity:

* NetEye users with **Administrative Access**
* NetEye users with **Full Module Access** for the **Assetmanagement**

Both cases correspond to users with **Super-Admin** recursive profile in
the **Root entity**.

Note that for any reason you must **not** rename the GLPI
**Super-Admin** profile and the **Root entity**.
