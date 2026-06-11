
Service Description
~~~~~~~~~~~~~~~~~~~

The elements that will be used within the project are described below, including the technical aspects.

Technological solutions adopted
```````````````````````````````

Würth IT Italy will configure and make use of the following technological solutions:

.. rubric:: NetEye SIEM

NetEye for years has been recognized in the market as a stable and highly flexible platform
in the IT Monitoring market. **NetEye SIEM** is the natural evolution of the platform toward
the System Information Event Management segment and integrates the ELK stack in the `Enterprise version <https://www.elastic.co/subscriptions>`__.

The Elastic Stack (aka ELK) is a robust solution for search, log management, and data analysis.
Elasticsearch, Logstash, Kibana and Elastic Agent are the macro components of the solution that combine to provide a single platform
for data storage, data retrieval, data sorting, and data analysis.

Würth IT Italy, through its partnership and close working relationship with the Elastic development team,
is able to offer the solution fully integrated within NetEye.

.. rubric:: SATAYO

SATAYO is an OSINT & Cyber Threat Intelligence platform developed by Würth IT Italy. Its capabilities
make it a key tool for all organizations that need to monitor their exposure within public
domain sources, found in the Surface, Deep & Dark Web.

The platform has an API (Application Programming Interface) and allows for the integration of collected
evidence with NetEye SIEM. SATAYO, for the monitored organization, is able to continuously calculate
the Exposure Assessment Index Value, a value ranging from 0 to 100 that indicates how exposed
that particular organization is.

Check out more information on SATAYO following the `link <https://www.wuerth-it.it/prodotto/cyber-security-threat-intelligence/>`__.

.. rubric:: SOC Prime

SOC Prime is the leading Threat Detection Marketplace, which makes detection rules developed by the best Threat Hunters internationally available
through an exclusive subscription.

Würth IT Italy, as a SOC Prime partner, has developed an integration that allows NetEye SIEM to have on board at all times detection rules
that are constantly updated and thus able to identify threats as they are discovered and analyzed over time.

.. rubric:: Greenbone Security Manager

GSM is a Continuous Vulnerability Assessment platform. The Greenbone Security Feed includes
more than 100,000 vulnerability tests to date and provides recognition and protection from
vulnerabilities such as SUPERNOVA, BlueKeep and PrintNightmare.

The API-equipped platform is integrated within NetEye SIEM.

.. rubric:: OpenCTI

OpenCTI (Open Cyber Threat Intelligence) is a platform designed for knowledge processing and
sharing for Cyber Threat Intelligence purposes.

It was developed by the French national cyber security agency (ANSSI) together with CERT-EU
(Computer Emergency Response Team of the European Union).

It was initially designed to develop and facilitate ANSSI's interactions with its partners.
Today the platform has been fully released in open source and made available to the entire
Cyber Threat Intelligence community to enable actors to structure, store, organize, visualize
and share their knowledge.

The platform comes with a connector that enables interaction with NetEye SIEM.

Monitoring Perimeter
````````````````````
The Monitoring perimeter is based on the list of host objects and services to be monitored.

It is provided by the customer during the onboarding and can be tweaked later in the process of monitoring
for existing contracts.

The NetEye SIEM platform will receive as input the data/logs/flows collected from the NetEye satellites
installed in the customer's network and from the SATAYO platform in the cloud of Würth IT Italy.

Architecture
````````````
The NetEye SIEM solution is configured as follows:

 - **NetEye Master**: is the main component in the architecture provided by Würth IT Italy, receives and processes logs
   from the various satellites installed in the client network. It transmits real-time data to the ELK console.
   The master machine is located within the Würth IT Italy cloud, thus enabling security and confidentiality of customer data.

 - **NetEye Satellite**: receives and processes logs from nodes connected to it, applying predefined
   correlation rules in order to detect possible cybersecurity threats.
   The satellite is installed at the client network, in the form of a VM (Virtual Machine).

All nodes in the client network send their logs to the NetEye Satellite installed at the client site,
and the NetEye Satellite subsequently sends them to the NetEye Master through IPSec VPN connections.

All logical components of the infrastructure are based on the use of encrypted protocols.

You can also learn more details at :ref:`master-satellite-arch`.

Data inalterability - El Proxy module
`````````````````````````````````````
To ensure the inalterability of authentication events, NetEye uses the El Proxy module.
El Proxy uses a series of signature keys to sign incoming logs and then sends them to Elasticsearch.

Each log is signed with a different Signature Key (seeded by the previous Signature Key);
the signature includes the hash of the previous log. Logs that for whatever reason cannot be indexed
in Elasticsearch are written in a Dead Letter Queue.

You can find more information of how El Proxy works in a dedicated :ref:`NetEye Guide section <ebp-architecture>`.

Health Checks
`````````````
In order to ensure that the blockchain is robust and that no errors have occurred during the collection of logs,
NetEye defines several Health Checks to alert users of any irregularities.

Segregation of Duties - customer spaces
```````````````````````````````````````
In Elastic, the mechanism used to implement access control is RBAC (Role Base Access Control).
This mechanism allows customized role authorizations to be granted and roles to be assigned to users
to implement access control.

To guarantee the correct segregation of client data, so that clients can only view objects
(events, detection rules, dashboards, reports) within their competence, strict subdivisions are applied
at the level of Elastic spaces, with the creation of a specific role for each individual client,
which inherits the permissions on the relative space.

The activity of creating the client space and defining the appropriate roles and permissions
is carried out through carefully checked batch phases that are periodically audited.
Each client is assigned a unique id.

.. figure:: /neteye-cloud/soc/img/rbac-access-control.png

   RBAC access control.
