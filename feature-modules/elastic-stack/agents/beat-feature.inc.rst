.. _elastic-beat:

The Elastic Beat feature
~~~~~~~~~~~~~~~~~~~~~~~~

NetEye can receive data from Beats installed on monitored hosts (i.e.,
on the *clients*).

NetEye currently supports Filebeat as a Beat agent and the `Filebeat
NetFlow Module
<https://www.elastic.co/guide/en/beats/filebeat/7.17/filebeat-module-netflow.html>`__
for internal use. Additional information about the Beat feature can be
found in the `official documentation
<https://www.elastic.co/guide/en/beats/libbeat/7.17/index.html>`__.

The remainder of this section shows first how NetEye is configured to
receive data from Beats, i.e., as a receiving point for data sent by
Beats, then explains how to install and configure Beats on clients,
using SSL certificates to protect the communication.

Overview of NetEye's Beat infrastructure setup
``````````````````````````````````````````````

Beats are part of the |ne| Elastic Stack module, which is an additional module, that
can be installed following the directions in the
:ref:`neteye-install-modules` section if you have the subscription.

.. warning:: Beats are intended as a replacement for Safed, even if they can
   coexist. However, since both Beat and Safed might process the same
   data, they would double the time and resources required, therefore
   it is suggested to activate only one of them.

The NetEye implementation allows Logstash to listen to incoming data on
a secured TCP port (**5044**). Logstash then sends data into two flows:

-  to a *file on disk*, in the **/neteye/shared/rsyslog/data** folder,
   with the following name:
   ``%{[agent][hostname]}/%{+YYYY}/%{+MM}/%{+dd}/[LS]%{[host][hostname]}.log``.
   The format of the file is the same used for ``safed`` files. This
   file is encrypted and its integrity validated, like it happens for
   Safed, and written to disk to preserve its inalterability.
-  to *Elastic*, to be displayed into preconfigured Kibana dashboards.

Communication is SSL protected, and certificates need to be installed
on clients together with the agents, see :ref:`next section
<elastic_beat_agent_installation>` for more information.

.. note:: When the module is installed there is no data flow until
   agents are installed on the clients to be monitored.  Indeed,
   deployment on NetEye consists only of the set up of the listening
   infrastructure.

The Beat feature is currently a CLI-only feature: no GUI is available
and the configuration should be done by editing configuration files.
