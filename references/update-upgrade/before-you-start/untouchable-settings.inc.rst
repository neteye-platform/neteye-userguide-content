To avoid issues during software update or :ref:`upgrade <neteye-upgrade-single>` procedures, the following
configuration items must **not** be modified by customers. This applies specifically
to the **Elastic Stack**.

1. **Index Templates and Component Templates**
   - All index templates marked as *Managed*
   - All component templates marked as *Managed*

2. **Index Lifecycle Management (ILM) Policies**
   - ``1_year_retention``
   - ``2_years_retention``
   - ``3_months_retention``
   - ``6_months_retention``
   - ``default_retention``
   - ``infinite_retention``

3. **Fleet Policies**
   - NetEye Operative Nodes
   - NetEye Single-Purpose Nodes

   Integrations may be added to these policies, but existing settings must remain unchanged.*

.. warning::

   Modifying any of the configuration items listed above may cause failures or
   inconsistencies during update or upgrade procedures, potentially leading to
   service degradation or data loss. Customers must leave these settings unchanged.
