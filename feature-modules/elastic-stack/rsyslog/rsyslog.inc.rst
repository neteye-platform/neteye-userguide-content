.. _rsyslog:

NetEye uses **rsyslog** as the underlying log auditing data collector.
Rsyslog is an Open Source software utility used on Unix and Unix-like
computer systems for forwarding and collecting log messages. It
implements the basic syslog protocol :rfc:`3164`, extends it with
content-based filtering, rich filtering capabilities and flexible
configuration options, and adds important features such as using TCP for
transport.

Rsyslog as a Data Collector
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Most embedded devices (routers, switches, firewalls, etc.) support the
transmission of syslog data via the :rfc:`3164` protocol, allowing the data
to be collected by rsyslog. Systems based on Windows, Linux, AIX, HP-UX,
and Solaris must instead use other agents to fulfill these needs.

Technical Overview
~~~~~~~~~~~~~~~~~~

The Syslog server runs as a service and collects the incoming log
messages into a defined folder architecture on the local file system.
The folders where the log files are stored have the following structure:

- Year

  - Month

    - Day

      - hostname-hostip.log

Each individual log file is filled during the day with incoming log
messages.
It is possible to associate a hostname with a specific IP address by modifying the
:file:`/neteye/shared/rsyslog/conf/rsyslog.d/logmanager-hosts.conf` file and adding
statements similar to the following: ::

  if ( $fromhost-ip == '10.1.2.3' ) then {
    set $.hostid = "my-host";
    set $.hostgroups = "[\"my-hostgroup-01\",\"my-hostgroup-02\"]";
    call writeToLogFile
    stop
  }

The **Logscleaner** service applies a retention policy to these files on a daily basis as follows:

- Files are compressed and stored in the same folder structure as the original log
  files, using the ``.gz`` extension
- In case the size of the logs exceeds the configured maximum, the oldest files are deleted (by default, this value is set to *5GB*).
- Files older than *30 days* are deleted

You can configure the retention policy in :file:`/neteye/local/os/conf/logscleaner.d/rsyslog_logs.toml`.
The Logscleaner service is scheduled by a ``systemd timer``, which by default runs every day at 4:00 AM.
Thanks to this policy, if errors occur and some log files are not indexed correctly in Elasticsearch, they can be reindexed by running the
``elasticsearch-reindex-logs`` script, as described in :ref:`Troubleshooting <elasticsearch-log-not-indexed>`.
