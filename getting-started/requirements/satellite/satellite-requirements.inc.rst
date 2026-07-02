.. _neteye-satellite-requirements:

|ne| Satellite Requirements
---------------------------

A **Satellite** is a |ne| instance which depends on a main |ne|
installation (either Single Node or Cluster), called **Master**, and
carries out tasks such as:

* execute :ref:`Icinga 2 <active-monitoring>` checks and forward
  results to the Master
* collect logs and forward them to the Master
* collect assets with GLPI Agents
* forward data through :ref:`NATS <data-gathering-in-neteye-satellites>`
* collect data through :ref:`Tornado Collectors
  <tornado-collectors-overview>` and forward them to the Master to be
  processed by Tornado

Besides those mentioned in :ref:`neteye-single-requirements`, there
are a few addeitional requirements that a satellite must satisfy:

* It is required that both the Master and the Satellite be equipped
  with the **same NetEye version**

* The NATS connection between Master and Satellite is always initiated
  by the Satellite, so please ensure that the :ref:`Networking
  Requirements <table-cluster-tcp-communication-req>` for NATS Leaf
  Nodes are satisfied

* If you are in a |ne| Cluster environment, check that all resource
  are in started status before proceeding with the Satellite
  configuration procedure

.. note:: For successful registration of |ne| Satellites in |ne|, any
   network security device (firewall, proxy, IDS/IPS, HTTPS inspection appliance)
   MUST NOT intercept, replace or modify SSL/TLS certificates used by the registration
   traffic. SSL/TLS inspection, SSL offloading or certificate substitution will prevent
   successful registration. Ensure TLS connections are passed end-to-end without modification.
