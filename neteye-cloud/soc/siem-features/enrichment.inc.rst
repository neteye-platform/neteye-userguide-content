
Enrichment
~~~~~~~~~~

Another key feature of NetEye SIEM is the possibility to enrich the collected data
with contextual information, e.g. geolocation data, business criticality, details of assets
and infrastructure in general.

Geographic data in NetEye SIEM is used to visually display IP addresses within a map and
can be used in rules to create specific alarms based on the geographic location of hosts.
The geographic location information is obtained either from a geographic search database
provided by `MaxMind <https://www.maxmind.com/>`__.

.. figure:: /neteye-cloud/soc/img/geographic-location.png

   Geographic location of IP addresses.

The data collected is supplemented with information about the assets and the role they play
within the customer's business (impact).

The metrics established by `FIPS199 <https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-60v1r1.pdf>`__ (page 10)
are used to assess impact.
Each host sending events within the SIEM will then be given the impact property (Low, Moderate, High).

The NetEye SIEM platform is integrated with the SATAYO IoC feed. By integrating the IoCs,
which are updated daily and contain information on IPs, hostnames, suspicious or dangerous
domains, the Cyber Security Analysts team has sufficient contextual information to immediately
identify alerts that need to be promptly investigated or directly assigned
to the Incident Response (IR) team.

Finally, the SIEM uses the vulnerability information to determine the level of severity
and possible business impact of detected threats.

NetEye SIEM is integrated with the Continuous Vulnerability Assessment
platform Greenbone Security Manager. If the customer has a different platform,
its integration can be considered.
