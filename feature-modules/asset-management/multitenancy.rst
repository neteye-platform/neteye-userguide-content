.. _glpi-configuration-multitenancy:

Multitenancy
------------

With the entities being configured properly, GLPI supports :term:`Multi-tenancy`.
The GLPI Server can be used by multiple tenants maintaining the confidentiality
and integrity of the information: this feature is implemented by associating every
|NE| Tenant to a dedicated GLPI Entity.

Enabling Asset Management
~~~~~~~~~~~~~~~~~~~~~~~~~

Asset Management features in a multitenant environment can only be used if enabled
for a specific Tenant.

For installing the Asset Management on a multitenant environment please make sure that
the Module is enabled for each particular tenant with the help of ``--enable-module neteye-asset``
option of the :ref:`neteye-tenant-config-create` command for a new tenant, or
:ref:`neteye-tenant-config-modify` command for an existing tenant, e.g. like shown below:

.. code::

    neteye tenant config modify <tenant_name> \
    --enable-module "neteye-asset"


If the Tenant still doesn't exist, follow :ref:`neteye-tenant` to configure it properly.


GLPI Entity
~~~~~~~~~~~

If Multitenancy is used in GLPI, when creating a new |NE| Tenant as described in
:ref:`tenants-configuration`, a dedicated GLPI Entity ``Root entity > 'New Tenant'``
will be created. All the users belonging to that Tenant should then be associated
to the automatically created role ``neteye_tenant_<tenant_name>`` in order to have
access to the Tenant's entity in GLPI.

For every new tenant created, there will be a connected user named ``neteye_glpi_agent_<tenant_name>``
that can be used for assets collection.

.. warning::

    |NE| Roles, Users and GLPI Entities automatically created with the ``neteye tenant config create``
    should never be modified to avoid permission issues or profile/entity mismatch between
    |NE| and GLPI.

The general :term:`Multi-tenancy` implementation, as described in the :numref:`figure-general-use-case`
is reached by having a GLPI Entity "Entity tenant A" associated with the |NE| "Tenant A".
Assets are sent by the GLPI Agent that belongs to the same Tenant authenticated through Basic
auth and, if no rules are applied in GLPI, Assets are directly sent in the correct Entity.

.. _figure-general-use-case:

.. figure:: /feature-modules/asset-management/img/general-use-case.svg

   General GLPI :term:`Multi-tenancy` implementation.

Once the Tenant is configured to receive assets, agent-based or agentless mode can be selected as
asset collection methods. All the configuration details can be found in the
:ref:`asset-collection-methods` section.

GLPI Rules
~~~~~~~~~~

The possibility of adding custom rules in GLPI for associating defined assets to a specific entity
remains valid. The following example displayed in :numref:`figure-sub-entity` shows how, applying an inventory tag
to a certain inventory and defining a GLPI rule that associates the tag to a specific sub-entity, GLPI rules are
still applied for custom entity association.

.. _figure-sub-entity:

.. figure:: /feature-modules/asset-management/img/sub-entity.svg

   Rules are applied to associate an inventory tag with a specific GLPI Entity.

In order to maintain the separation of multiple Tenants, GLPI Agents are not allowed to import inventories to
entities or sub-entities that don't belong to the same Tenant. The :numref:`figure-wrong-rule-applied` shows an
example where a GLPI Agent tries to import an inventory with a tag that associates the inventory to another
Tenant.

.. _figure-wrong-rule-applied:

.. figure:: /feature-modules/asset-management/img/wrong-rule-applied.svg

   The GLPI Agent is not allowed to upload inventories to entities which it is not associated with.
