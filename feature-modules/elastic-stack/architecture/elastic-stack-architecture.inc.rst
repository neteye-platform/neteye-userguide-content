
NetEye Elastic Stack Feature Module is based on the `Elastic Stack
<https://www.elastic.co/>`_ key components, in particular:

**Elasticsearch and Elasticsearch cluster**
   Elasticsearch can be installed in different modalities, the
   simplest being as a service running on a NetEye single instance.

   When running a NetEye Cluster with NetEye Elastic Stack installed,
   Elasticsearch can be run as either a parallel Elasticsearch Cluster
   or as an Elastic node within the NetEye cluster.  Please refer to
   :ref:`NetEye's Cluster Architecture <neteye-cluster-architecture>`
   for details.

   Elasticsearch, regardless of how it is installed, is used in the
   context of SIEM for multiple purposes:

   #. as a database to store all the log files that have been
      collected and processed

   #. as a search engine over the data stored

   #. to process data (though this function is carried out also by other
      components, see below)

**BEAT**
   A Beat is a small, self-contained agent installed on devices within
   an infrastructure (mostly servers and workstations) that acts as a
   client to send data to a centralised server where they are
   processed in a suitable way.

   Beats are part of the Elastic Stack; they gather data and send them
   to Logstash.

   There are different types of Beat agents available, each tailored
   for a different type of data. BEATs supported by NetEye are
   described in section :ref:`elastic-beat`.

**Elastic Agent**
    Elastic Agent is a lightweight, modular agent that is part of
    Elastic Stack. It is designed to collect and ship data from various
    sources to Elasticsearch. The Elastic Agent is used to replace
    and consolidate multiple Beats and other agents previously used
    for data collection.

    Please refer to the dedicated :ref:`elastic-agent` section for more
    details of Elastic Agents in |ne| and to the `official Elastic Agent
    documentation <https://www.elastic.co/elastic-agent>`_ for a detailed
    description of the Elastic component.

**Logstash**
   Logstash is responsible for the collection of logs,
   (pre-)processing them, and forwarding them to the defined storage:
   an Elasticsearch cluster or to El Proxy. Logs are collected from
   disparate sources, including Beats, syslog, and REST endpoints.

**Rsyslog**
   Rsyslog is an Open Source software utility used on Unix and Unix-like
   computer systems for forwarding and collecting log messages.
   In the NetEye Elastic Stack, rsyslog acts as a data collector for systems
   and devices that support the syslog protocol, aggregating log messages and
   forwarding them to Logstash and then to Elasticsearch for centralized storage
   and analysis.

**El Proxy**
   The purpose of the **El**\ astic Blockchain **Proxy** is to receive
   data from Logstash and process it: first, the hash of the data is
   calculated, then data are signed and saved into a blockchain, which
   guarantees their unchangeability over time, and finally everything
   is sent to Elastic. Please refer to section :ref:`El Proxy
   Architecture <ebp-architecture>` for more information.

**Kibana**
   A :abbr:`GUI (Graphic User Interface)` for Elasticsearch, its
   functionalities include:

   #. visualise data stored in Elasticsearch
   #. create dashboards for quick data access
   #. define queries against the underlying Elasticsearch
   #. integration with Elastic's SIEM for log analysis and
      rule-based threats detection
   #. use of machine-learning to improve log analysis

More information about these components can be found in the remainder
of this section.
