
.. _neteye-ports-requirements:

TCP and UDP Ports Requirements
------------------------------

This section contains a list of TCP and UDP ports that should be
opened on the Corporate Network and/or Private (Heartbeat) Network
to ensure that |ne| operates correctly. These requirements apply to
both |ne| Single Node and Cluster installations, except for cluster-specific ports.

It is important to remember that Private (heartbeat) Network should not be directly
accessible from external networks. For security reasons, we suggest to open only
the ports used by the running services and close everything else.

.. note:: All ports are listed with their default values as assigned
   by IANA or by the respective software producers.

System Ports
~~~~~~~~~~~~

Make sure the following system ports are always opened, because they refer to basic
functionalities of |ne|. The ports listed in Table 4 are to be opened on
a Corporate Network in order to allow its communication with |ne|.

Additionally, the communication between |ne| and the Corporate Network
should be built with respect to the |ne| architecture, which means selected ports
are to be opened on the Master Node or its Satellite.

.. _table-cluster-tcp-communication-req:

.. csv-table:: TCP/UDP Ports Requirements for System and Management
   Communication between Corporate Network and the |ne| Cluster
   :header: "Protocol/Port", "Service", "Instance", "Description"

   "RMCP TCP 5900", "**iDRAC Access**, Inbound", "Master, Satellite", "Systems that need to manage a
   node via iDRAC should reach each Management IP Address on iDRAC
   dedicated ports. Please refer to `Dell's Support Documentation
   <https://www.dell.com/support/manuals/it-it/poweredge-fx2/idrac8_2.30.30.30_ug>`__
   to understand the required ports."
   "TCP 80, 443", "**NetEye Management Interface** and **System Updates**, Inbound", "Master, Satellite", "Systems
   used to manage |ne| should reach the Cluster Virtual IP via
   HTTP/S. Satellites use those ports to receive data from agents."
   "TCP 22","**Node SSH Console**, Inbound", "Master, Satellite", "Systems used to manage deep |ne|
   configuration and node configuration should reach every Physical
   Node IP via SSH."
   "TCP 25,465", "**SMTP**, Oubound", "Master", "To allow sending of notifications,
   the required ports for SMTP outbound should be allowed from each Physical Node IP to the selected SMTP Relay Server.
   If the Icinga2 notification feature is enabled on a Satellite as well, the same ports should be opened on the latter."
   "UDP 123", "**NTP**, Outbound", "Master, Satellite", "Each node should be able to reach the official
   internal time source server with NTP Protocol."
   "TCP 636, 3269", "**LDAP Authentication and Authorization**, Outbound", "Master", "To allow
   your Active Directory user accounts the ability to access |ne|,
   each node must be able to contact at least one DC on both ports 636
   (LDAP) and 3269 (Global Catalog) encrypted over SSL.
   To allow your LDAP user account the ability to access |ne|, each
   node must be able to contact your LDAP Source on port 636 (or the
   Port of your choice)."
   "TCP 7422", "**NATS Leaf Nodes**", "Master (Inbound), Satellite (Outbound)", "Satellites should be able to reach
   the |ne| Master NATS Leaf Node port in order to send data generated on the Satellite to the Master."
   "TCP 4222", "**NATS Server**", "Master (Inbound), Satellite", "In order to send data from a NATS Client (e.g. a Telegraf)
   directly to |ne|, port 4222 should be opened (NATS TCP port)."

The ports in :numref:`table-cluster-internal-req` should be opened on the Private (heartbeat) Network
and include `the cluster requirements specified by RedHat
<https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/s1-firewalls-haar>`_.

.. _table-cluster-internal-req:

.. csv-table:: Cluster-internal Port Requirements
   :header: "Protocol/Port", "Required for", "Description"
   :widths: 20, 20, 60

   "UDP 623", "iDRAC fencing"
   "TCP 2224", "Node-to-node communication", "It is required to open
   port 2224 on each node to allow :command:`pcs` to talk from any
   node to all nodes in the cluster, including itself. [[#f1]_]"
   "TCP 2347", "`neteye-agent` service."
   "TCP 3000", "Grafana"
   "TCP 3121", "Pacemaker Remote nodes", "Required on all nodes if the
   cluster has any Pacemaker Remote nodes. [[#f2]_]"
   "TCP 3306", "MariaDB"
   "TCP 4748", "Tornado API", "Communication with Tornado API from the
   GUI and for testing."
   "TCP 5403", "Quorum device host", "Required on the quorum device
   host when using a quorum device with :command:`corosync-qnetd`. [[#f3]_]"
   "TCP 5404", "Corosync multicast UDP", "Required on corosync nodes if
   corosync is configured for multicast UDP."
   "TCP 5405, 5406", "Required on all corosync nodes"
   "TCP 5664", "Icinga 2", "Required by Icinga 2 for intra-cluster
   communication [[#f4]_]"
   "TCP 7788-7799", "DRBD", "Port range may be extended as new
   resources or services are added."
   "TCP 8086", "InfluxDB"
   "TCP 8000", "Lampo"

**Table Notes:**

.. [#f1] When using the Booth cluster ticket manager or a quorum
   device you must open port 2224 on all related hosts, such as Booth
   arbiters or the quorum device host.

.. [#f2] Indeed, Pacemaker's CRMd daemon on the full cluster nodes
   will contact the `pacemaker_remoted` daemon on Pacemaker Remote
   nodes at port 3121. If a separate interface is used for cluster
   communication, the port only needs to be open on that interface. At
   a minimum, the port should open on Pacemaker Remote nodes to full
   cluster nodes. Because users may convert a host between a full node
   and a remote node, or run a remote node inside a container using
   the host's network, it can be useful to open the port to all
   nodes. It is not necessary to open the port to any hosts other than
   nodes.

.. [#f3] The default value can be changed with the -p option of the
   :command:`corosync-qnetd` command.

.. [#f4] This port should be open also on the Corporate Network if
   Satellite Nodes need to send monitoring data to the Master.

*****

.. _cluster-monitoring-requirements:

Monitoring Requirements
~~~~~~~~~~~~~~~~~~~~~~~

Monitoring **should never** be carried out on the private (heartbeat)
cluster network.

At present, the *NetEye Cluster's Virtual IP* is used for **passive
monitoring** (i.e., by devices autonomously sending information to
|ne|) and agent deployment, while the *Physical Node's IP* is used
for **active monitoring** (i.e., requests from |ne| to devices).

We distinguish the following types of monitoring:

* **Active monitoring** through ``ICMP`` consisting of direct ``ICMP``
  requests from |ne| to monitored devices

* **Active monitoring** through ``SNMP`` is similar to previous, but using
  the ``SNMP`` protocol in spite of ``ICMP``

* **Passive monitoring** through ``SNMP`` uses SNMP trap events sent from
  monitored devices to |ne|

* **Mail-based monitoring** is based on emails sent by devices or users to
  |ne| that trigger specific events

The following monitoring requirements apply to the server that is to be monitored.
Active and passive monitoring have different requirements in terms of
ports. Moreover, the operating system installed on the devices to
be monitored also influences the ports to be opened; all are reported in
:numref:`table-monitoring-communication-req`. Depending on the
monitoring tasks activated, additional considerations are described in
section :ref:`monitoring-additional-requirements`.

.. _table-monitoring-communication-req:

.. csv-table:: Monitoring Requirements
   :header: "Protocol/Port", "Description", "Monitoring"
   :widths: 20, 60, 20

   "ICMP", "Test via ping to check if a host is
   alive","Active/Passive"
   "TCP 4222, 4244", "(APM)", "Active/Passive"
   "TCP 5001", "plugin check_iperf", "Passive"
   "TCP 5665", "server monitoring (ICINGA2 protocol)", "Active"
   "UDP 161", "Device/server monitoring (SNMP protocol)", "Active"
   "UDP 162", "TRAP SNMP", "Passive"
   "TCP 135", "Windows server monitoring (WMI protocol) and Windows
   admin user (more ports are required)", "Active, Windows devices
   only"
   "TCP 22", "Linux Server monitoring (SSH protocol with
   check_by_ssh)", "Active, Linux devices only"

.. _monitoring-additional-requirements:

Additional Remarks for Monitoring
`````````````````````````````````

Depending on the services enabled on the cluster, take the following
into consideration:

* For *Sahi* and/or *check_webpage*, create a dedicated user account
  if required.
* Enable the SNMP v2c protocol and community on all servers and
  devices.
* Enable all TCP and UDP ports needed for specific monitoring
  requirements, such as *check_tcp* and/or *check_udp* for network
  service ports like: **53** (DNS), **123** (NTP), **3306** (MySQL),
  etc. For a full list of reserved ports, you can consult `this
  website
  <https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers>`_.
* You may need to contact your |ne| 4 consultant for the following
  requirements:

  * Create a database monitoring user, where the rights granted will
    depend on the database’s vendor
  * Create a user on HyperV systems
  * Allow connections between |ne| 4 and all VLANs/Subnets involved
    in monitoring


.. _individual-module-reqs:

Individual Module Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Individual |ne| modules may have their own specific requirements that
need to be taken into consideration if a particular Module is to be enabled.
When configuring cluster nodes, you should also make sure that the following
requirements are included for each node.

.. note::
   Please pay attention to the type of Network - Corporate or Private - each port
   requirement applies to.


.. rubric:: ntopng

The following ports must be opened on the |ne| Master side in order to allow
the communication between ntopng, nProbe, and Redis. The ports are inbound.

.. _table-ntopng-port-req:

.. csv-table:: ntopng Corporate Network Port Requirements
   :header: "Port", "Service/Description"
   :widths: 20, 80

   "TCP 5556", "zmq (nProbe client)"
   "TCP 6363", "nProbe (Netflow collector)"

.. csv-table:: ntopng Private Network Port Requirements
   :header: "Port", "Service/Description"
   :widths: 20, 80

   "TCP 6379", "Redis"

.. rubric:: Elastic Stack

The following ports need to be opened either on the part of Corporate or
Private (heartbeat) Network to be able to receive, process and store log data.
Please note that all the ports are inbound and refer to the **Master** instance only.

.. _table-siem-corp-port-req:

.. csv-table:: Elastic Stack Corporate Network Port Requirements
   :header: "Port", "Description"

   "TCP/UDP 514", "syslog/rsyslog"
   "TCP 6161", "syslog/splunk"
   "UDP 2055", "Netflow listening port (Netflow protocol)"
   "TCP 5044", "Logstash input for Beats"
   "TCP 5045", "Logstash input for Elastic Agent"
   "TCP 9200", "Elasticsearch"
   "TCP 8220", "Fleet Server"
   "TCP 8200", "APM Server"

.. note:: Port **9200** should be opened if there are Satellite Nodes
   that send data for the Elasticsearch service

.. _table-siem-instracluster-port-req:

.. csv-table:: SIEM Private (heartbeat) Network Port Requirements
   :header: "Port", "Description"

   "TCP 4950", "El Proxy"
   "TCP 5061", "Kibana"


Moreover, the following domains should be reachable for ensuring correct
functioning of your Elastic Stack installation:


.. csv-table::
   :header: "Domain", "Port", "Intended Use"

   "epr.elastic.co", "443 TCP", "Elastic Package Registry (mandatory in all SIEM installations)"
   "geoip.elastic.co", "443 TCP", "Elastic GeoIP endpoint"
   "storage.googleapis.com", "443 TCP", "GeoLite2 City, GeoLite2 Country, and GeoLite2 ASN GeoIP2 databases used by Elastic GeoIP processor"


.. rubric:: SLM

The SLM Daemon needs a dedicated inbound port to be opened on the Master instance
to operate correctly. The requirement refers to the Corporate Network.

.. _table-slm-port-req:

.. csv-table:: SLM Port Requirements
   :header: "Port", "Description"
   :widths: 20, 80

   "TCP 4949", "SLM daemon"

.. rubric:: Alyvix

In order for Alyvix Service to successfully communicate with |ne| the following port
should be opened on a Corporate Network, relative to both Master and Satellite instances.

.. csv-table:: Alyvix Port Requirements
   :header: "Port", "Description"
   :widths: 20, 80

   "TCP 4222", "While TCP 4222 grants Inbound connection from Alyvix Service to |ne|,
   TCP 443 should also be opened to allow Outbound connection from |ne| to Alyvix Service"

.. _cluster-sp-nodes-req:

Single Purpose Nodes
~~~~~~~~~~~~~~~~~~~~

**Elastic-only nodes** work only as part of Elasticsearch
cluster and communicate on the private (heartbeat) network,
therefore they do not expose any ports required by other
services.

**Voting-only nodes** only provide quorum to several components of
|ne| cluster: DRBD, PCS, and Elasticsearch. Like Elastic-only nodes,
they do not expose any service and communicate with other cluster
nodes on the private (heartbeat) cluster network; therefore no port
should be explicitly opened.
