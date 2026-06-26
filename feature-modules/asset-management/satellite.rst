Communication through a Satellite
---------------------------------

In the case GLPI Agents cannot reach the :term:`Master` directly due to traffic or accessibility restrictions,
a |ne| :term:`Satellite` can be used as a Proxy to forward all the assets collected by the GLPI Agent.
As described in :numref:`figure-satellite-glpi-agent-proxy`, GLPI Agents perform inventories and
send them to the |ne| :term:`Satellite` which forwards the communication to the |ne| :term:`Master`. In a :term:`Multi-tenancy`
environment, the :term:`Satellite` should belong to the same tenant of the GLPI Agents.

.. _figure-satellite-glpi-agent-proxy:

.. figure:: /feature-modules/asset-management/img/satellite-glpi-agent-proxy.svg

   The GLPI Agents use a |ne| :term:`Satellite` as a proxy to send assets to |ne| :term:`Master`.
