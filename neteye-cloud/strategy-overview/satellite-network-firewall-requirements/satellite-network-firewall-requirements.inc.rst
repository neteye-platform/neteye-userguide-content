
|nec| Satellite Network and Firewall Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Purpose and Audience
^^^^^^^^^^^^^^^^^^^^

This page lists the network and firewall flows that must be allowed before a |ne| Satellite is connected
to |nec|. It is intended for administrators and implementation teams who prepare network
and firewall rules before onboarding.

Read this page before the onboarding session so that the required rules can be requested in time.

.. note::

   This page describes only the generic requirements that apply to all |nec| customers.
   Values specific to your environment, such as private IP addresses, custom routing, and custom ports,
   are provided separately during onboarding.

Architecture Overview
^^^^^^^^^^^^^^^^^^^^^

The communication model involves four parties:

* **Customer hosts**: the monitored systems in your network. The Icinga Agent and the GLPI Agent must be installed on each host.
  The Elastic Agent is also installed if the corresponding subscription is active.
* |neb| **Satellite**: deployed in your network. Hosts send their data to the Satellite, and the Satellite
  communicates with |nec|. You can connect more than one Satellite to the |nec| Master.
  This is recommended for multi-site environments.
* **Internet**: the path between your network and |nec|. All connections from the Satellite to
  |nec| are initiated from your network.
* |necb| **Master**: the central |nec| platform that the Satellites connect to.

In short, your hosts send data to the Satellite inside your network, and the Satellite
connects to the |nec| Master over the Internet. The Master only initiates connections to the Satellite
for the SSH and HTTPS flows listed below.

.. figure:: /neteye-cloud/strategy-overview/img/satellite-architecture-overview.svg
   :alt: Customer hosts send monitoring and log data to the NetEye Satellites in the customer network, and the Satellites send it to the NetEye.Cloud Master over the Internet. Firewall rules are needed at two points: between hosts and Satellites, and between Satellites and the Master.

   Communication paths and the two points where firewall rules apply

Firewall Requirements
^^^^^^^^^^^^^^^^^^^^^

The tables list the flows between your hosts, the |ne| Satellite, and the |nec| Master.
The **Condition** column shows when each flow is required:

* *Baseline*: required for every |nec| Satellite.
* *Subscription or module dependent*: required only if one of these subscriptions is active:
  SOC Attacker Centric, SOC System Administrator (SOC ADS), or ELK - Log Analytics.
* *Only if required by the source*: required only for specific syslog sources that send data to the Satellite.
* *Only after confirmation during onboarding*: do not open the flow until it is confirmed during onboarding.

.. figure:: /neteye-cloud/strategy-overview/img/satellite-firewall-flows.svg
   :alt: Firewall flows with service, protocol, and port between customer hosts and the NetEye Satellites, and between the NetEye Satellites and the NetEye.Cloud Master

   Firewall flows between customer hosts, |ne| Satellites, and the |nec| Master

The hosts open all connections to the Satellite.

.. csv-table:: Flows between customer hosts and the |ne| Satellite
   :header: "Protocol/Port", "Service", "Purpose", "Condition"
   :widths: 18, 20, 38, 24

   "TCP 5665", "Icinga 2", "Monitoring checks and agent communication, in both directions", "Baseline"
   "TCP 4222", "NATS", "Message transport between hosts and the Satellite", "Baseline"
   "TCP/UDP 514", "Syslog (standard)", "Syslog forwarding to the Satellite", "Baseline"
   "TCP/UDP 5001-5199", "Syslog (various)", "Syslog forwarding from specific sources", "Only if required by the source"
   "TCP 8220", "Elastic Agent / Fleet", "Agent management and data ingestion", "Subscription or module dependent"
   "TCP 9200", "Elasticsearch data", "Data ingestion into Elasticsearch", "Subscription or module dependent"
   "TCP 5045", "Logstash", "Log data ingestion", "Subscription or module dependent"
   "TCP 443", "HTTPS", "Data sent by the GLPI Agent, and download of the Icinga, GLPI, and Elastic Agents from the local repository", "Baseline"

The Satellite opens the connections to the |nec| Master. The Master opens only SSH and HTTPS connections
to the Satellite. The Master uses different public IP addresses for the two directions:

* **Satellite to Master**: allow connections to 82.193.25.103 and 18.102.30.177.
* **Master to Satellite**: allow connections from 82.193.25.103 and 15.161.38.146.

.. csv-table:: Flows between the |ne| Satellite and the |nec| Master
   :header: "Direction", "Protocol/Port", "Service", "Purpose", "Condition"
   :widths: 16, 14, 14, 34, 22

   "Satellite to Master", "TCP 7422", "NATS Leaf Node", "Connection of the Satellite to the |nec| Master", "Baseline"
   "Satellite to Master", "TCP 5665", "Icinga 2", "Monitoring data exchange with the Master", "Baseline"
   "Satellite to Master", "TCP 8220, 9200", "Elastic", "Elastic data exchange with the Master", "Subscription or module dependent"
   "Satellite to Master", "TCP 5000", "Logstash", "Log data delivery to the Master", "Subscription or module dependent"
   "Master to Satellite", "TCP 22", "SSH", "Transmission of the Satellite configuration (port can be configured).", "Only after confirmation during onboarding"
   "Master to Satellite", "TCP 443", "HTTPS", "Access to HTTP and HTTPS services exposed on the Satellite, for example IMEdge and ntopng", "Only after confirmation during onboarding"

TLS and Certificates
^^^^^^^^^^^^^^^^^^^^

Traffic between the |ne| Satellite and the |nec| Master must reach the Master without modification.
TLS inspection, SSL offloading, certificate replacement, or proxy behavior on these flows can prevent the Satellite
from registering and can break secure communication with |nec|. Exclude the |nec| Master endpoints
from any such inspection or interception.
