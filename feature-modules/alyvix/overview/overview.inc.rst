

Alyvix is a synthetic monitoring system based on computer vision which synthesizes
real users without being hardwired to application engines.

NetEye integration with Alyvix allows currently the monitoring of Alyvix nodes,
with planned support for scheduling test cases, assigning test cases to sessions
on different Alyvix machines, and more.

More information about Alyvix is available on the `official website <https://www.alyvix.com/>`_
and in the `official documentation <https://www.alyvix.com/learn/index.html>`_.


.. _alyvix-nodes-architectures:

Architecture of Alyvix Nodes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section provides information about the supported Alyvix node types.
As explained below, currently |ne| supports four different types of node:

- :ref:`alyvix-multitenant-tenant-specific-node`
- :ref:`alyvix-multitenant-tenant-shared-node`
- :ref:`alyvix-single-tenant-via-satellite-node`
- :ref:`alyvix-single-tenant-direct-to-master-node`

The main differences between them reside in the adopted Tenancy configuration and
the way they communicate with the |ne| Master.
There are two types of communication involved: HTTPS and NATS.
|ne| communicates with the Alyvix Service via API calls through the HTTPS communication channels,
while NATS serves as a communication channel to send the performance metrics from the Alyvix nodes to the |ne| Master.
The choice of using one rather than the other boils down to your infrastructure
and purpose of the node.

.. _alyvix-multitenant-tenant-specific-node:

Multitenant - Tenant Specific
`````````````````````````````

This node is meant to be used in Multitenant environments where the Alyvix node is completely dedicated to and managed
by a specific tenant. In this case, the |ne| Master and the Alyvix node can be set up on different networks.

This Alyvix node is dedicated to only one Tenant and hence the communication
between the Alyvix node and the |ne| Master will flow through a Satellite of that Tenant.
This architecture forces the Alyvix node to run sessions belonging to one
specific Tenant, meaning that all the sessions running
on a node will be related to one Tenant.

Please note that, to associate a `Multitenant - Tenant Specific` Alyvix node to a tenant, this must be specified
in the Host configuration in the Icinga Director.

.. figure:: /feature-modules/alyvix/overview/img/alyvix-tenant-specific-director.png
   :alt: The configuration of an Alyvix Multitenant - Tenant specific in the Director

.. note:: This node is currently not fully supported, meaning that
   the API/HTTP communication is still direct between the |ne| Master and
   the Alyvix node

.. _figure-alyvix-tenant-specific-node-architecture-diagram:

.. figure:: /feature-modules/alyvix/overview/img/alyvix-tenant-specific-node-architecture-diagram.png
   :alt: Communication between the Alyvix node and the |ne| Master through Satellites

   Communication between the Alyvix node and the |ne| Master through Satellites

.. _alyvix-multitenant-tenant-shared-node:

Multitenant - Tenant Shared
```````````````````````````

This node is meant to be used in Multitenant environments the Alyvix node serves multiple Tenants.
This Alyvix node can run sessions belonging to different
Tenants and is managed by the admin of the |ne| Master.
For this reason, the |ne| Master and the Alyvix node must be set up on the same network.

In this case, the |ne| Master will communicate directly with the Alyvix node without the need of
Satellites. For this reason, all the sessions running on the node must be configured to be
related to one specific Tenant from the available ones.

.. _figure-alyvix-tenant-shared-node-architecture-diagram:

.. figure:: /feature-modules/alyvix/overview/img/alyvix-tenant-shared-node-architecture-diagram.png
   :alt: Direct communication between the Alyvix node and the |ne| Master

   Direct communication between the Alyvix node and the |ne| Master

.. _alyvix-single-tenant-via-satellite-node:

Single Tenant - Via Satellite
`````````````````````````````

This node is equivalent to the :ref:`alyvix-multitenant-tenant-specific-node`
node but for single Tenant environments. Note that in single Tenant environments
the only Tenant available is always the Master Tenant.

.. note:: This node is currently not fully supported, meaning that
   the API/HTTP communication is still direct between the |ne| Master and
   the Alyvix nodes

.. _alyvix-single-tenant-direct-to-master-node:

Single Tenant - Direct to Master
````````````````````````````````

This node is equivalent to the :ref:`alyvix-multitenant-tenant-shared-node`
node but for single Tenant environments. Note that in single Tenant environments
the only Tenant available is always the Master Tenant.

.. _alyvix-roles:

Roles
~~~~~

The following roles are currently supported in the NetEye Alyvix integration. For more information about how to
configure IcingaWeb2 roles to match the one described below, please consult
the :ref:`alyvix-permissions-roles` section.

.. _alyvix-super-admin-role:

Super Admin
```````````

A user having the `Super Admin` role is considered as an administrator of each configured Alyvix node and hence has
their full control, with all capabilities.

.. _alyvix-tenant-admin-role:

Tenant Admin
````````````

A user having the `Tenant Admin` role is considered as an administrator of one (or more) NetEye Tenants,
associated to his role.

As an administrator of a tenant, the user has administrative access on all the :ref:`alyvix-multitenant-tenant-specific-node`
nodes associated with that tenant.

On :ref:`alyvix-multitenant-tenant-shared-node` nodes, the `Tenant Admin` has control over objects specific to the
tenants under their administration. This includes editing their tenants' Sessions, creating Test Cases, adding and
editing their tags and monitoring their results. However, they do not have authority to manage global configurations
of the node, such as creating new Sessions or managing the license.

.. _alyvix-tenant-viewer-role:

Tenant Viewer
`````````````

A user with `Tenant Viewer` role is granted read-only access to one or more NetEye Tenants
associated with their role.

Within the :ref:`alyvix-multitenant-tenant-specific-node` nodes, the `Tenant Viewer` can observe
activities and data pertinent to their assigned tenants.

Optionally, the Tenant Viewer can be limited to viewing a specific subset of the Alyvix Test Cases of the Tenant.
This restriction is based on the tags assigned to their role, acting as filters. Consequently, the Tenant Viewer
gains visibility solely into the Test Cases linked with the designated tags. This filtering functionality is exclusive
to the `Tenant Viewer` role and can be set up by following the steps outlined in the
:ref:`Tenant Viewer Configuration <alyvix-tenant-viewer-configuration>` section.

On :ref:`alyvix-multitenant-tenant-shared-node` nodes, similar to a `Tenant Admin` user, the `Tenant Viewer`
can view tenant-specific objects, including Sessions and Test Cases, and monitor their outcomes. However,
they do not have permissions to observe global characteristics such as configurations, available sessions
or licenses.
