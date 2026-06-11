Safed Configuration: Overview
-----------------------------

This section introduces the Safed Syslog Agent, and shows you how to use
Log Manager to automatically configure and update a remotely installed
Safed Agent in your infrastructure.

As indicated in the schema in
:numref:`figure-syslog-host-configuration`, the EventID and LogFile
configuration is abstracted by a templates configuration that allows
you to more easily reuse multiple log objects and filters across
multiple types of hosts.

.. _figure-syslog-host-configuration:

.. figure:: /feature-modules/elastic-stack/deprecated-modules/img/syslog-view.png
   :alt: Syslog Host Configuration Schema

   The Syslog Host Configuration Schema

The centralized Safed configuration extends the NetEye server-based
Syslog server configuration and filter setup. This allows you to develop
filter and log templates that can be sent as configuration instructions
to Safed Agents. The first step is to register the host(s) you want to
log within Log Manager.

Configuring the Safed Agent consists of three tasks:

- Agent configuration both in general, and specifically for Safed
- :ref:`General Settings <logmanager-safed-configuration>`:
  Configuring the Safed Agent and associating LogFile and EventLog
  templates with a particular host.
- :ref:`LogFiles <logmanager-safed-logfiles>`: The configuration of
  log files (text files written by applications, DBMS, etc.) to be
  monitored and logged on the Syslog server
- :ref:`EventLogs <logmanager-safed-eventlog>`: The configuration of
  EventID Objects from Microsoft systems that should be logged
