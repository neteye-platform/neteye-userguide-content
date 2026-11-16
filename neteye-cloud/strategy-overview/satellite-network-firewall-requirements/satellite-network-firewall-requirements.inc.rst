
NetEye.Cloud Satellite Network and Firewall Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Pending validation before publication: the inbound SSH (TCP 22) and HTTPS (TCP 443) flows from the Master, and all subscription- or module-dependent flows.

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

The table lists the flows between your hosts, the NetEye Satellite, and the NetEye.Cloud Master.
The **Condition** column shows when each flow is required.

.. figure:: /neteye-cloud/strategy-overview/img/satellite-firewall-flows.svg
   :alt: Firewall flows with service, protocol, and port between customer hosts and the NetEye Satellites, and between the NetEye Satellites and the NetEye.Cloud Master

   Firewall flows between customer hosts, NetEye Satellites, and the NetEye.Cloud Master

.. csv-table::
   :header: "Source", "Destination", "Initiator", "Direction", "Protocol", "Port", "Service", "Purpose", "Condition"
   :widths: 13, 13, 11, 10, 8, 9, 11, 15, 10

   "Customer hosts", "NetEye Satellite", "Customer host", "Outbound from hosts", "TCP", "4222", "NATS", "Message transport between hosts and the Satellite", "Baseline"
   "Customer hosts", "NetEye Satellite", "Customer host", "Outbound from hosts", "TCP", "5045", "Logstash", "Log data ingestion", "Subscription or module dependent"
   "Customer hosts", "NetEye Satellite", "Customer host (Icinga agent)", "Bidirectional", "TCP", "5665", "Icinga 2", "Monitoring checks and agent communication", "Baseline"
   "Customer hosts", "NetEye Satellite", "Customer host", "Outbound from hosts", "TCP/UDP", "514", "Syslog (standard)", "Syslog forwarding to the Satellite", "Baseline"
   "Customer hosts", "NetEye Satellite", "Customer host", "Outbound from hosts", "TCP/UDP", "5001-5199", "Syslog (various)", "Syslog forwarding from specific sources", "Only if required by the source"
   "Customer hosts", "NetEye Satellite", "Customer host", "Outbound from hosts", "TCP", "8220", "Elastic Agent / Fleet", "Agent management and data ingestion", "Subscription or module dependent"
   "Customer hosts", "NetEye Satellite", "Customer host", "Outbound from hosts", "TCP", "9200", "Elasticsearch data", "Data ingestion into Elasticsearch", "Subscription or module dependent"
   "Customer hosts", "NetEye Satellite", "Customer host", "Outbound from hosts", "TCP", "443", "HTTPS", "Secure web traffic to the Satellite", "Baseline"
   "NetEye Satellite", "NetEye.Cloud Master (82.193.25.103, 5.161.38.146)", "NetEye Satellite", "Outbound", "TCP", "7422", "NATS Leaf Node", "Connection of the Satellite to the NetEye.Cloud Master", "Baseline"
   "NetEye Satellite", "NetEye.Cloud Master (82.193.25.103, 5.161.38.146)", "NetEye Satellite", "Outbound", "TCP", "5665", "Icinga 2", "Monitoring data exchange with the Master", "Baseline"
   "NetEye Satellite", "NetEye.Cloud Master (82.193.25.103, 5.161.38.146)", "NetEye Satellite", "Outbound", "TCP", "8220, 9200", "Elastic", "Elastic data exchange with the Master", "Subscription or module dependent"
   "NetEye Satellite", "NetEye.Cloud Master (82.193.25.103, 5.161.38.146)", "NetEye Satellite", "Outbound", "TCP", "5000", "Logstash", "Log data delivery to the Master", "Subscription or module dependent"
   "NetEye.Cloud Master (82.193.25.103, 5.161.38.146)", "NetEye Satellite", "NetEye.Cloud Master", "Inbound to Satellite", "TCP", "22", "SSH", "Remote access to the Satellite (port is configurable)", "Only after confirmation during onboarding"
   "NetEye.Cloud Master (82.193.25.103, 5.161.38.146)", "NetEye Satellite", "NetEye.Cloud Master", "Inbound to Satellite", "TCP", "443", "HTTPS", "Secure web traffic to the Satellite", "Only after confirmation during onboarding"

Allow the baseline flows (TCP 4222, 5665, and 443, and TCP/UDP 514) from all monitored hosts to the NetEye Satellite, and install
the Icinga agent on each host. Allow the Satellite to connect outbound to the NetEye.Cloud Master on TCP 7422 and 5665.
Allow the Elastic (TCP 8220, 9200) and Logstash (TCP 5000) flows only if the corresponding subscription or module is enabled.
Do not open the inbound SSH (TCP 22) and HTTPS (TCP 443) flows from the Master until they are confirmed during onboarding.

Baseline rules are required for every NetEye.Cloud Satellite. Rules marked *Subscription or module dependent* apply only
when the matching module or subscription is enabled. Rules marked *Only if required by the source* apply only to the
syslog sources that send data to the Satellite.

TLS and certificates
^^^^^^^^^^^^^^^^^^^^

Traffic between the NetEye Satellite and the NetEye.Cloud Master must reach the Master without modification.
TLS inspection, SSL offloading, certificate replacement, or proxy behavior on these flows can prevent the Satellite
from registering and can break secure communication with NetEye.Cloud. Exclude the NetEye.Cloud Master endpoints
from any such inspection or interception.
