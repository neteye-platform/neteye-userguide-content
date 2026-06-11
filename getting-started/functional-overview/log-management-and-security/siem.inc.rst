
SIEM
~~~~

**SIEM**, Security Information and Event Management, helps to perform management and analysis of Logs
by collecting them, processing them to detect anomalies and threats, and finally visualizing them.

Machine Learning and a multinode architecture were implemented for scalability, which help to complete these functionalities.

The SIEM solution is based on the Elastic stack and is intended to provide various means to manage---collect, process, and sign---log files
produced by |ne| and various services running on it, as well as the logs collected from external systems, with the help of
Beats and Elastic Agents.

Typical components of a SIEM solution include:

* a log collector, which can consist of multiple applications that work together to
  receive log files and convert them into a given format
* a storage facility, typically a (distributed) database
* a visualisation engine, to create dashboards and reports
* some kind of time-stamping solution to ensure data immutability,
  which is useful for log auditing and compliance to laws and regulations

.. rubric:: Beats and Elastic Agents

**Beats** and **Elastic Agents** are used to collect logs and send them to |ne|.

In order to enhance the monitoring of different hosts, using available integrations given by the Elastic environment,
*Elastic Agent* can be used to collect data from local and external sources with a single unified agent per host.

In a similar way, |ne| can receive data from *Beats* installed on monitored hosts.
The agent is to be installed on devices within an infrastructure (mostly servers and workstations)
and acts as a client to send data to a centralised server where they are processed in a suitable way.

.. rubric:: Logstash

`Logstash <https://www.elastic.co/logstash>`__ is an open source server-side data processing pipeline that collects data from
multiple sources, transforms it, and then sends it to a preferable storage.

In the context of SIEM solutions, Logstash is responsible for collecting logs,
(pre-)processing them, and forwarding them to Elasticsearch or El Proxy.

.. rubric:: El Proxy

The purpose of the :ref:`Elasticsearch Blockchain Proxy <ebp-architecture>` is to receive data from Logstash and process it.
After the data is signed and integrated into a secure blockchain, it is sent to Elasticsearch.

.. rubric:: Elasticsearch

Used mainly as a storage facility for all the log files that have been collected and processed in the context of SIEM solution,
`Elasticsearch <https://www.elastic.co/elasticsearch>`__ can also be used for multiple purposes, such as serving as search engine over the data stored,
or even for processing data.

`Kibana <https://www.elastic.co/kibana/features>`__ software enables you to give shape to your data and serves as the GUI for Elasticsearch.
With this you can visualise data stored in Elasticsearch, create dashboards for quick data access
and define queries against the underlying Elasticsearch.

You can find more information on how to configure, collect and centralize your log data in the :ref:`Elastic Stack feature module <elastic-feature-module>`.
