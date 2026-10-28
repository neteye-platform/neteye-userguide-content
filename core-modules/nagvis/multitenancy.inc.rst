.. _object_filtering_and_multi-tenancy:

Object filtering and multi-tenancy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Objects shown on a NagVis map are filtered through the Icinga DB restrictions that apply
to the logged-in user. Only objects the user is allowed to see in Icinga DB have their state retrieved.
This applies to all backend queries, including host, service, hostgroup and servicegroup objects.

As a consequence, NagVis can be used for a basic form of multi-tenancy. If the ``neteye_tenant`` variable
is set on objects, an Icinga DB role can restrict each user to the objects of their own tenant.
NagVis then only returns state information for those objects.

Moreover, tenant users can create and edit maps. When adding objects to a map, a user with the
``nagvis/edit`` permission can only select the objects they have access to, so objects belonging
to other tenants never appear.

.. warning:: The filter applies to the object's state, not to the map itself. A map is a static definition,
            so an object the user isn't allowed to see is not removed from the map. It is still drawn at its
            position, with its name, but instead of a status it shows an error saying the object does not exist.
            Users can therefore still see that these objects exist on the map, and see their names.
            For this reason, NagVis multi-tenancy should not be treated as strict isolation.
            If the existence or name of an object must stay hidden from a tenant, don't place that object on a map
            the tenant can open. Use separate maps per tenant and restrict access to each map with the NagVis
            map permissions.
