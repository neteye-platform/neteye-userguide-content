.. _satayo_items:

************
SATAYO Items
************

SATAYO items are evidence entities collected from OSINT scans and correlated to represent your external attack surface.

All items are shown in the dedicated **table** page, which includes **filters** to narrow down the displayed items.

Clicking an entry opens a **preview panel** on the right with its details and, if present, related items.
From the preview panel, clicking the icon near the **Related** title opens another tab with the list **filtered** to show its related items.
Clicking the link icon on a related finding opens the same view, with the corresponding item’s preview panel automatically displayed.


.. figure:: satayo-items/img/ipv4-address-details-example.png
   :alt: List of items with the preview panel open of a selected item

   List of items with the preview panel open of a selected item

.. figure:: satayo-items/img/ipv4-address-related-detail-example.png
   :alt: List of related items with the preview panel open of a selected item

   List of related items with the preview panel open of a selected item

|

.. _hostname_item:

Hostname
========

Hostname, like IP addresses, is one of the items that SATAYO finds during its analysis. It is a human-readable name that corresponds to an IP address, making it easier to identify and remember. They are one of the starting points for SATAYO’s exposure assessment analysis, and they can provide valuable information about the services and applications running on a network.

They also work as **aggregators** for other items, such as **Services, Mail servers, Mail, SSL/TLS certificates**, and more.

They do not contribute directly to the :abbr:`EAIV (Exposure Assessment Index Value)` score, as their mere existence doesn't pose a risk.

|

.. _ipv4_address_item:

IPv4 Address
============

The **IPv4 address item** represents a publicly reachable IP address discovered by SATAYO. It can be correlated to other items, such as **Ports** and **Hostnames**.

|

.. _port_item:

Port
========

Each **port item** complements an IPv4 address item and represents an exposed port on which a service or application at the IP address is listening.

|
|

.. _registry_item:

Registry
========

The **registry item** consists of the subnet blocks where the retrieved found IP address resides.
The addresses inside are scanned to see if there are other resolvable hostnames that, if found, are added to the list of hostnames and constitute an additional base for future scans and analysis.

|
|

.. _cve_item:

CVE
===

The **CVE item** represents a publicly known vulnerability identified on a specific host during SATAYO analysis.
Each item corresponds to a single :abbr:`CVE (Common Vulnerabilities and Exposures)` identifier detected on a particular host.
If multiple vulnerabilities are discovered on the same host, a separate CVE item is created for each vulnerability.

For each vulnerability, SATAYO provides a link to the `U.S. National Vulnerability Database (NVD) <https://nvd.nist.gov/>`_,
maintained by the National Institute of Standards and Technology (NIST), where detailed technical information about the CVE can be found.

When available, SATAYO also provides links to publicly known exploits or Proof-of-Concept (PoC) implementations associated with the vulnerability.

Additional contextual indicators are included to help prioritize remediation:

- **KEV (Known Exploited Vulnerabilities)** — Indicates whether the vulnerability
  is listed in the catalog maintained by the `Cybersecurity and Infrastructure Security Agency (CISA) <https://www.cisa.gov/known-exploited-vulnerabilities-catalog>`_,
  which tracks vulnerabilities that are known to be actively exploited in the wild.

- **EPSS (Exploit Prediction Scoring System)** — A data-driven `model <https://www.first.org/epss/model>`_
  that estimates the probability that a vulnerability will be exploited in real-world attacks.
  The EPSS model produces a probability score expressed as a percentage (0–100%).
  The higher the score, the greater the likelihood that the vulnerability will be exploited.

EPSS is managed by `Forum of Incident Response and Security Teams (FIRST) <https://www.first.org/epss/>`_,
and SATAYO is included in the official list of EPSS-supported vendors.

|
|

.. _service_item:

Service
=======

A **service item** represents a specific service or application exposed over HTTP on a particular port of an IP address and identified by a hostname.

It is always associated with a hostname, because the hostname is a determining factor in request routing. The HTTP server may use the hostname to route
requests to the appropriate virtual host or service.

It provides information about the exposed HTTP service and web server, including:

- Open ports **with** SSL/TLS enabled
- Open ports **without** SSL/TLS enabled

Also **service metadata**, such as:

- HTML title
- meta-generator
- maintainer email addresses
- exposed server headers
- unencrypted protocols in use
- detected technologies
