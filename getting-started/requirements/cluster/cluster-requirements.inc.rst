
.. _cluster-configuration:

.. _cluster-guidelines:

Cluster Requirements and Best Practices
---------------------------------------

This section focuses mostly on best practices for a |ne| deployment in
a cluster environment, since system requirements for each Cluster Node
correspond to those for a :ref:`Single Node
<neteye-single-requirements>`.

These guidelines are subject to change and should not be considered as
hard requirements, because they may vary significantly depending on
the running services and logging level.

The infrastructure of a network in which |ne| is involved
should be carefully designed in order to take advantage all of its
functionalities, especially in the case of a particularly complex
setup, in which the experience of a |ne| specialist can prove
useful. To get in touch, please contact us at info.italy@wuerth-it.com.

.. _cluster-net-requirements:

Cluster Networking Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This section illustrates in detail the requirements and their
rationale for all networking involving a |ne| Cluster: inbound,
outbound (**"Corporate Network"**), and among the nodes composing the
cluster (called *"intra-cluster communication"* or **"Private
(Heartbeat) Network"** in the remainder).

The remainder of this section is therefore rather conversational, to
summarize the content, we point out a few good practices:

* setting up a (|ne|) Cluster requires a dedicated network for
  intra-cluster communication, separated from the Corporate Network

* intra-cluster communication should be allowed freely without
  limitations

* Each |ne| Cluster node should have its own IP Address in the Private
  Network

.. _corporate-net-requirements:

Corporate Network
`````````````````

Configuring the |ne| Cluster and allowing communication between
Cluster and Corporate Network impacts several parts of networking and
requires to open a number of :ref:`ports <neteye-ports-requirements>`. Key
concepts and points to focus on include:

**Network Layer: Monitoring and Management Network**
   This network will be used by |ne| to collect monitoring and
   performance data, system logs and allow access to:

   * |ne| Web interface
   * Each node SSH interface
   * Any other running services

   The bottom line for this network is that it must be able to
   access--and must be reached by--every system that needs to be
   monitored by |ne|.


**Network Link**
   Although a single NIC will suffice, to allow service continuity in
   case of hardware malfunction, we suggest that you plan for bonding
   of two network adapters in an active/standby (failover)
   configuration.

**IP Addresses: Physical Node**
   A dedicated IP address for each node. Each IP should be in the same
   network segment. This IP is used both for management tasks and
   active (from |ne| to devices) monitoring.

**IP Addresses: Management (iDRAC)**
   A dedicated IP address for the management interface of each node.

**Cluster Virtual IP Address**
   One IP address used by the clustered system to allow monitoring and
   management from the public network

Depending on the services enabled on the |ne| Cluster, a number of
ports must be used for the communication flow with the Corporate
Network.

While Satellite Nodes are |ne| instances themselves, they do not need
to respect all these requirements. Indeed, Satellite Nodes
already communicate securely with the |ne| Master node using
NATS/Tornado. Moreover, the purpose of Satellite Nodes is to monitor
the infrastructure and collect data, therefore they only need to allow
traffic for NATS (Master/Satellite communication), Icinga
(monitoring), and Elastic (EBP and related services).

.. _private-net-requirements:

Private (Heartbeat) Network
```````````````````````````

Intra-cluster communication should usually be freely allowed. Key
concepts and points to focus on include:

**Network Layer: Internal Communication Network**
   This network will be used for internal communication between each
   |ne| service. |ne| cluster nodes should be able to talk to each
   other without restriction. For security reasons, you should not
   share this network with other systems.

**Network Link**
   Although a single NIC will suffice, to allow service continuity in
   case of hardware malfunction, we suggest that you plan for bonding
   of two network adapters in an active/standby (failover)
   configuration.  Ensure inter-node, round-trip latency between each
   node is less than 300ms, with a target of 2ms as optimal, as stated
   in the `RHEL Corosync documentation <https://access.red
   hat.com/articles/2823721>`__."

**IP Addresses**
   Internal services running on a |ne| Cluster with all modules
   installed require at least **30 IP Addresses**. It is therefore
   **strongly recommended** to always configure a dedicated */24
   network* (e.g., 172.20.12.0/24) to avoid running out of available
   IPs and being forced to reconfigure the whole network if the
   cluster is expanded."

   .. note:: None of these IPs should be publicly exposed, because
      they are used only by services running on the |ne| cluster.
