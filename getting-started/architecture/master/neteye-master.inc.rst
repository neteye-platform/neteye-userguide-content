.. _arch-master:

|ne| Master
-----------

Active or passive monitoring processes are concentrated within the main |ne|
instance - the |ne| **Master**. Based on the role distribution within the
environment, the Master instance is responsible for:

* holding configuration files

* sending monitoring configurations to monitored objects previously
  created in the Director

* receiving reports of events from various data sources, and
  matching those events with the predefined rules

* executing actions based on the outcome of the performed analysis

* configuring and initiating availability checks on a monitored
  infrastructure

The |ne| Master functionality for processing received data is granted
by Icinga 2 monitoring server, Tornado Engine and Telegraf Consumers.
The process of configuring monitoring tasks is controlled via the Director
module.

.. _arch-collect-data:

Agents and Data Collecting
~~~~~~~~~~~~~~~~~~~~~~~~~~

Retrieving data from the data sources is done by the local agents.

Collecting data for monitoring purposes is performed by means of Agents.
*Agent* is a software that is to be installed on a node to monitor hosts and services.
Apart from collecting data, agents are set to perform different tasks, e.g. process data
or pass it to the Master, or perform health checks.

In the |ne| hierarchy, agents can only have a parent node, i.e. either Master or Satellite.
In case Satellites are not involved in the process of data communication, an Agent communicates
directly with the Master instance.

While a Satellite can handle a set of hosts, different Agents can be installed on them.
In turn, Agents can be grouped into :ref:`tenants <nats_leaf_multi_tenancy>`.

In order for an agent to operate successfully, make sure that versions of all involved instances
are compatible. Namely, the agent version should be the same as the version of the Satellite or Master
it is connected to.

Depending on the type of monitoring you would like to perform, i.e. |ne| module you would like
to utilize, different agents can be installed on hosts: Icinga agent for Active monitoring
purposes, Beats agent (Elastic Stack), Telegraf agent, APM agent, AX Monitor agent, etc.


.. rubric:: Icinga Agents

Icinga Agents communicate with the Master within their own Icinga
infrastructure. Agents receive configuration information from the Master or
a Satellite, and send collected data back to the Icinga instance
on the Master.
Configurations are generated and distributed to Icinga agents
based on `zones <https://icinga.com/docs/icinga-2/latest/doc/06-distributed-monitoring/#zones>`_.

.. rubric:: Telegraf Agents

Local Telegraf Agents are to be configured manually as per which
metrics shall be collected. Collected data is then passed to
a Telegraf consumer on the Master for further processing. You can learn more
on the Telegraf configuration at :ref:`this page <telegraf-configuration>`.

.. rubric:: Elastic Agents

Elastic Agent is designed to collect and ship data from various sources to Elasticsearch.
It is used to replace and consolidate multiple :ref:`Beats <elastic_beat_agent_installation>` and other agents previously used for data collection.
Elastic Agents are pre-configured in the |ne| Master to serve as Fleet Servers: this allows you to easily install and manage Elastic Agents
on your monitored hosts via the `Fleet UI in Kibana <https://www.elastic.co/guide/en/kibana/current/fleet.html>`_.

.. rubric:: GLPI Agents

GLPI Agents are to be installed and configured manually to collect assets
and send them to the GLPI Server on the Master. They can be installed
on any Windows/Linux machine and configured to send data directly to
the Master or through a |ne| Satellite as a Proxy.

To learn more about how to configure GLPI for Single- or Multitenant environments, please
consult GLPI :ref:`asset-management-conf`.

.. rubric:: Alyvix Nodes

The Alyvix Node must be :ref:`installed <install-alyvix-node>` and :ref:`configured <alyvix-create-an-alyvix-node>` manually.
|ne| communicates with the Alyvix Service via API calls through HTTPS communication channels,
while NATS serves as a communication channel to send the performance metrics from the Alyvix Nodes to the |ne| Master
for service monitoring purposes. Alyvix Test Case execution reports are stored on the Alyvix Node instead.

.. figure:: /getting-started/architecture/img/multitenancy-neteye-agents.jpg

   Data communication from Agents to the Master.


.. rubric:: Tornado Collectors

Data gathering for passive monitoring is performed by *Tornado
Collectors*. Collected data flows down the **Tornado pipeline**,
which, apart from the Collectors, contains Tornado *Engine* and
Tornado *Executors*. The Engine and the Executors are deployed within
the Master instance.

A Collector receives information from one or more unstructured sources
(e.g. emails or log files), treated as External Events. Based on this,
a Collector converts External Events into structured Tornado-specific
Events, and sends them to the Tornado Engine for further
processing. Its main function is to match received events against
preconfigured rules, and execute actions in case of a match.

.. figure:: /getting-started/architecture/img/architecture_tornado_pipeline.svg
   :scale: 90%

   Outline of the Tornado Pipeline

See :ref:`passive_monitoring` for more information on Tornado.
