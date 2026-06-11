
.. _glpi-configuration-singletenancy:

.. _asset-management-conf:

Single Tenancy
--------------

The single tenancy is the most common configuration. It consists of a |NE| system, installed without
Tenants or :term:`Satellite`. In this case the aim of an efficient GLPI configuration is the
collection of all the assets of the IT infrastructure and inventorying them inside the main GLPI Entity.

.. _figure-basic-configuration-use-case:

.. figure:: /feature-modules/asset-management/img/basic-configuration.svg

   General GLPI Single Tenancy implementation.


Thus, as described above, for correctly performing an inventory
the system should have the following configuration:

*  the Master Entity configured in the GLPI server
*  a |NE| user and role for the GLPI Agent that ensure that the inventory is sent to the correct Entity
*  GLPI Agents installed on the desired device and configured as described in :ref:`asset-collection-methods`

NetEye is preconfigured with a default user and role named ``neteye_glpi_agent_master``
for the Agent related to the Master Entity. By default the Agent can act on the ``Root entity > Master`` entity,
that is automatically created during Assetmanagement Module configuration.

In order to send assets directly to the GLPI ``Root entity``, you can modify the
GLPI entity of the parent role ``neteye_tenant_master`` with the following command:

.. code::

    neteye tenant config modify master \
    --custom-override-glpi-entity "Root entity"

It is also possible to specify other GLPI Entities as main entity for the tenant master role.
If you're planning to utilize multiple tenants in future, it is not recommended to override the
default GLPI Entity. In any case the role ``neteye_tenant_master`` should never be modified by
hand. More information can be found in :ref:`neteye-tenant-config-create`.

.. note:: The ``Root entity > Master`` entity can be deleted in case you want to directly
   use the ``Root entity`` or another custom entity for inventory.

To start collecting assets, you can choose to run in agent-based or agentless configuration.
All the configuration details can be found in the :ref:`asset-collection-methods` section.
