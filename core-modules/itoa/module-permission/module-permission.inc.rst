
.. _itoa-user-permissions-conf:

Configuring User Permissions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

User permissions in the ITOA module can be managed by configuring and
assigning :ref:`Roles <auth-roles>` in NetEye.

The ITOA Module can be accessed directly from the NetEye GUI (within
the ITOA menu) using :ref:`Single Sign On <neteye-sso>`, if the logged
user has permissions to access (see below). Upon the first access to
ITOA from a user, that user will be created inside ITOA with ITOA
permissions initialized.

.. note:: The **ITOA** menu entry will not be visible to the user, if he
   doesn't have any of the listed **Grafana Organization Role** (i.e.,
   **Admin**, **Editor** or **Viewer**) in NetEye.

.. _itoa-user-management:

User Management
```````````````

In the ITOA Module, each **Role** can have one assigned **Organization**
and a respective **Organization Role**, one of *Admin*, *Editor*, and
*Viewer*). Optionally, a list of **Teams** belonging to the Organization
can also be specified.

You can refer to the official Grafana docs to learn more about the user
management model of Grafana with
`Organizations <https://grafana.com/docs/grafana/v12.4/administration/organization-management/>`__
and related
`Permissions <https://grafana.com/docs/grafana/v12.4/administration/roles-and-permissions/>`__.

If a user belongs to more than one **Role** within *different*
**Organizations**, they will be able to access *each* **Organization**.
If a user belongs to more than one **Role** within *the same*
**Organization** but *different* **Organization Roles**, they will be
assigned the *most permissive* **Organization Role** ( Admin >> Editor
>> Viewer ).

Example: For a **Role** in NetEye with the ability to edit, delete or
create dashboards in the Grafana "*Main Org.*", the **Organization**
"*Main Org.*" must be configured with either the "*Editor*" or the
"*Admin*" **Organization Role**.

.. _itoa-performance-graph:

Performance Graph
`````````````````

To show the **Performance Graph** in the status page for each
monitored object, a separate permission is required, but it is *not
necessary* to set it to a specific **Organization**.

.. _itoa-conf-form:

Configuration Form
``````````````````

The Analytics module adds the following fields for each **role**:

* **Organization:** The name of *one* Grafana organization. This
  setting also requires a **role** to be set. If the organization does
  not exist in Grafana, then nothing will happen.
* **Role:** Either the ``Viewer``, ``Editor`` or ``Admin`` role that
  will be granted on the specified **Organization**.
* **Teams:** A comma-separated list of **teams** which must exist in
  the specified **organization**.
* **analytics/view-performance-graph:** Enabling this option will allow
  each user to see the **Performance Graph** for every monitoring
  object.  However, this will have no effect on a user's access rights
  inside Grafana, they will merely be able to navigate the
  **Performance Dashboard** from the monitoring view.
  In order to correctly see the Graph, a user should have at least general access also to
  module **Grafana** with **grafana/graph**. For examples on how to correctly configure
  hosts/services graphs, please refer to `Icingaweb2 Module Grafana doc <https://github.com/Mikesch-mp/icingaweb2-module-grafana/blob/v1.4.2/doc/04-graph-configuration.md>`__
