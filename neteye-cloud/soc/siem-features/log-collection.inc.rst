
Log Collection
~~~~~~~~~~~~~~

NetEye SIEM becomes the primary tool used to detect, analyze, respond to and prevent
cyber security incidents. It also meets compliance requirements by centralizing logs
and enabling forensic analysis.

NetEye SIEM aggregates event and flow data produced by connected resources (network devices
servers, applications, etc.), normalizes them into a consistent format and correlates them
by applying certain rules, allowing the analysis of events from different systems.

The platform natively supports the collection of events and flows from the network systems
of the main vendors, but no limit is placed on the type of resources that can be integrated
as |witit| Cyber Security Analysts can create customized filters capable of integrating
any type of data.

The communication between the sources and NetEye SIEM can take place, at high level, in 2 modes:

 - pushing: the source sends data to NetEye SIEM, mainly through Elastic agent and the syslog protocol;
 - polling: NetEye SIEM connects to the source and collects its data; different protocols are used depending on the log source.

The log collection process supports the flexible use of TCP or UDP ports.

The solution ensures the collection and retention of logs and events generated
in the customer's IT environment, according to specific retention policies that can be configured
on the platform, depending on the type of log and the organization’s need for compliance
with certifications and accreditations.

The default log retention policy configured on the platform is 30 days for network devices and 180 days for servers.
Elastic Stack (ELK), in its Enterprise version is the tool we use within NetEye SIEM to detect, analyze, respond to and prevent computer security incidents. ELK is valued for its speed, scalability, and ability to index different types of data. The data ingestion phase is the process by which raw data is analyzed, normalized and enriched before it is indexed. Once indexed, our Cyber Security Analysts can produce quite complex queries and initiate in-depth investigations.

An index in ELK is a set of data that is related to each other.
ELK stores data in JSON format. During the indexing process, ELK stores data and builds an inverse index
so that the data can be investigated in real time.

Logstash is one of ELK's main applications and is used to aggregate, process, and transmit data
to Elasticsearch.

Logstash allows us to receive data from different sources simultaneously, enrich and transform
them before indexing them in Elasticsearch.

The way logs are collected and stored is fully compliant with GDPR and PCI-DSS regulations
and ensures that the data is usable for forensic purposes, as it ensures that:

 - Logs are collected in real time. The solution performs time zone normalization and when components are located in countries with different time zones it is possible to configure them to use the same time zone as NetEye SIEM, or have all components adopt GMT (Greenwich Mean Time).
 - Logs cannot be altered. This is guaranteed by NetEye's El Proxy module, which is based on blockchain technology (Log Management - Real Time Log Signing). Archived events and flows cannot be modified even by system administrators, but possibly deleted (any deletion operation is saved in the platform audit logs).
 - The logs are timestamped.
