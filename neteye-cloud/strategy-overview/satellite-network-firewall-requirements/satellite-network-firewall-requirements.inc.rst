
NetEye.Cloud Satellite Network and Firewall Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Pending before publication: the names of the subscriptions that trigger the subscription- or module-dependent flows.

Purpose and audience
^^^^^^^^^^^^^^^^^^^^

This page lists the network and firewall flows that must be allowed before a NetEye Satellite is connected
to NetEye.Cloud. It is intended for administrators and implementation teams who prepare network
and firewall rules before onboarding.

Read this page before the onboarding session so that the required rules can be requested in time.

.. note::

   This page describes only the generic requirements that apply to all NetEye.Cloud customers.
   Values specific to your environment, such as private IP addresses, custom routing, and custom ports,
   are provided separately during onboarding.

Architecture overview
^^^^^^^^^^^^^^^^^^^^^

The communication model involves four parties:

* **Customer hosts**: the monitored systems in your network. Icinga agents must be installed on each host.
  The Elastic Agent is also installed if the corresponding subscription is active.
* **NetEye Satellite**: deployed in your network. Hosts send their data to the Satellite, and the Satellite
  communicates with NetEye.Cloud. You can operate more than one Satellite.
* **Internet**: the path between your network and NetEye.Cloud. All connections from the Satellite to
  NetEye.Cloud are initiated from your network.
* **NetEye.Cloud Master**: the central NetEye.Cloud platform that the Satellites connect to.

In short, your hosts send data to the Satellite inside your network, and the Satellite
connects to the NetEye.Cloud Master over the Internet. The Master only initiates connections to the Satellite
for the SSH and HTTPS flows listed below.

.. figure:: /neteye-cloud/strategy-overview/img/satellite-architecture-overview.svg
   :alt: Customer hosts send monitoring and log data to the NetEye Satellites in the customer network, and the Satellites send it to the NetEye.Cloud Master over the Internet. Firewall rules are needed at two points: between hosts and Satellites, and between Satellites and the Master.

   Communication paths and the two points where firewall rules apply

Firewall requirements
^^^^^^^^^^^^^^^^^^^^^

The tables list the flows between your hosts, the NetEye Satellite, and the NetEye.Cloud Master.
The **Condition** column shows when each flow is required:

* *Baseline*: required for every NetEye.Cloud Satellite.
* *Subscription or module dependent*: required only when the matching subscription or module is enabled.
* *Only if required by the source*: required only for the syslog sources that send data to the Satellite.
* *Only after confirmation during onboarding*: do not open the flow until it is confirmed during onboarding.

.. figure:: /neteye-cloud/strategy-overview/img/satellite-firewall-flows.svg
   :alt: Firewall flows with service, protocol, and port between customer hosts and the NetEye Satellites, and between the NetEye Satellites and the NetEye.Cloud Master

   Firewall flows between customer hosts, NetEye Satellites, and the NetEye.Cloud Master

The hosts open all connections to the Satellite.

.. csv-table:: Flows between customer hosts and the NetEye Satellite
   :header: "Protocol/Port", "Service", "Purpose", "Condition"
   :widths: 18, 20, 38, 24

   "TCP 4222", "NATS", "Message transport between hosts and the Satellite", "Baseline"
   "TCP 5045", "Logstash", "Log data ingestion", "Subscription or module dependent"
   "TCP 5665", "Icinga 2", "Monitoring checks and agent communication, in both directions", "Baseline"
   "TCP/UDP 514", "Syslog (standard)", "Syslog forwarding to the Satellite", "Baseline"
   "TCP/UDP 5001-5199", "Syslog (various)", "Syslog forwarding from specific sources", "Only if required by the source"
   "TCP 8220", "Elastic Agent / Fleet", "Agent management and data ingestion", "Subscription or module dependent"
   "TCP 9200", "Elasticsearch data", "Data ingestion into Elasticsearch", "Subscription or module dependent"
   "TCP 443", "HTTPS", "Data sent by the GLPI Agent", "Baseline"

The NetEye.Cloud Master is reachable at the public IP addresses 82.193.25.103 and 5.161.38.146. Allow both addresses.
The Satellite opens the connections to the Master. The Master opens only the SSH and HTTPS connections
to the Satellite, from the same two addresses.

.. csv-table:: Flows between the NetEye Satellite and the NetEye.Cloud Master
   :header: "Direction", "Protocol/Port", "Service", "Purpose", "Condition"
   :widths: 16, 14, 14, 34, 22

   "Satellite to Master", "TCP 7422", "NATS Leaf Node", "Connection of the Satellite to the NetEye.Cloud Master", "Baseline"
   "Satellite to Master", "TCP 5665", "Icinga 2", "Monitoring data exchange with the Master", "Baseline"
   "Satellite to Master", "TCP 8220, 9200", "Elastic", "Elastic data exchange with the Master", "Subscription or module dependent"
   "Satellite to Master", "TCP 5000", "Logstash", "Log data delivery to the Master", "Subscription or module dependent"
   "Master to Satellite", "TCP 22", "SSH", "Transmission of the Satellite configuration. The port can be configured.", "Only after confirmation during onboarding"
   "Master to Satellite", "TCP 443", "HTTPS", "Access to HTTP and HTTPS services exposed on the Satellite, for example IMEdge and ntopng", "Only after confirmation during onboarding"

TLS and certificates
^^^^^^^^^^^^^^^^^^^^

Traffic between the NetEye Satellite and the NetEye.Cloud Master must reach the Master without modification.
TLS inspection, SSL offloading, certificate replacement, or proxy behavior on these flows can prevent the Satellite
from registering and can break secure communication with NetEye.Cloud. Exclude the NetEye.Cloud Master endpoints
from any such inspection or interception.
