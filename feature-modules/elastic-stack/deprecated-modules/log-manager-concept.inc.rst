.. _logmanager-concept:

How Log Manager Works
---------------------

NetEye uses **rsyslog** as the underlying log auditing data collector.
Rsyslog is an Open Source software utility used on Unix and Unix-like
computer systems for forwarding and collecting log messages. It
implements the basic syslog protocol :rfc:`3164`, extends it with
content-based filtering, rich filtering capabilities and flexible
configuration options, and adds important features such as using TCP for
transport.

.. figure:: /feature-modules/elastic-stack/deprecated-modules/img/syslog_architecturalconcept.png
   :alt: Log Manager system architecture

   The Log Manager system architecture

.. _logmanager-concept-rsyslog:

Rsyslog as a Data Collector
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Most embedded devices (routers, switches, firewalls, etc.) support the
transmission of syslog data via the :rfc:`3164` protocol, allowing the data
to be collected by rsyslog. Systems based on Windows, Linux, AIX, HP-UX,
and Solaris must instead use other agents to fulfill these needs.
NetEye's Safed Agent combines the functionality of Snare and Epilog to
guarantee interoperability with rsyslogd and syslog-ng, and uses open
protocols to allow integration with other centralized Syslog Servers.

The Safed Agent extends the functionalities of other open source agents:
 * Better reliability for data transmission
 * Efficient CPU usage
 * Improved security through TLS, HTTPS, and access control
 * Optimized and centralized agent configuration
 * A larger number of supported platforms
 * Improved filtering efficiency
 * Auto discovery of administrators
 * A single agent for all systems: Windows XP, Windows
   Vista, Windows 7, Windows Server 2000, Windows Server 2003 (32 – 64
   Bit), Windows Server 2008, Windows Server 2008 R2, IBM-AIX, HP-UX,
   Solaris, and Linux (Debian, RedHat, SUSE, Ubuntu, etc.)

Log Manager Concepts
~~~~~~~~~~~~~~~~~~~~

**Role Distinction: System Administrator vs. Logging Supervisor**

One of the principle concepts of the legally clean implementation of the
central NetEye logging facility is the distinction between an internal
system administrator and the person in charge of system logging (the
logging supervisor). The security and protection of the log server
configuration can only be proven to be unaltered (a legal requirement)
if the log server configuration cannot be accessed by the system
administrator. This is why the NetEye Log Manager web frontend comes
with a dedicated, separate role determining access to the configuration
section.

**Syslog Server Access Restriction**

Following the above reasoning, the syslog server itself must not only be
protected physically from being accessed by system administrators, but
also by password. If the NetEye monitoring service is also running on
the same server, then access can be granted through dedicated
permissions. (This avoids the need of providing the universal and
unlimited access that a root user has).

**Security and Integrity Validation**

Log Manager provides for the following set of legally required
characteristics:

* **Verification that log date and date of last modification on the
  system are equal**.
* **The signature of the log file** exists and that the signature
  checksum recalculation matches the current content of the log file.
* **The timestamp of the signature** corresponds at most to only one
  day after the date when the log files were collected.
* **Folder integrity check:** A SHA512 checksum is calculated over the
  whole folder for a single day, and stored in the archive of the next
  day. This initiates a chain of trusted integrity.  Together with the
  checksum of the previous day, the hash of the next day's checksum is
  then calculated. ANY modification of the SHA512 file would lead to
  corruption on the hash result for the next day, and the same for ANY
  modification of any log file and their signatures.
* **Monitoring checks** are performed to determine whether the Safed
  Agent is running and is reachable on the remote system.
* For Windows-based hosts, checks can be run for **automatic detection
  and registration of users making up the AD administrators group**.

The process of daily log archiving can be described in the flowchart
in :numref:`figure-log-archival`:

.. _figure-log-archival:

.. figure:: /feature-modules/elastic-stack/deprecated-modules/img/log-archival-process.png
   :alt: Daily log archiving process

   Daily log archiving process

.. _logmanager-concept-technical-overview:

Technical Overview
~~~~~~~~~~~~~~~~~~

The Syslog server runs as a service and collects the incoming log
messages, based on custom filter and host rules, into a defined folder
architecture on the local file system. The folders where the log files
are stored have the following structure:

- Year

  - Month

    - Day

      - hostname-hostip.log

Each individual log file is filled during the day with incoming log
messages. Then at the end of the day, this log file is compacted to
optimize the space used on disk and signed with an installation-specific
private key in order to freeze the log file content and the current
timestamp in a signature file. This ensures that any modification to the
file can be detected because the signature will be invalidated, and that
any signature re-created after the day the log file was made can be
discovered.

By default, in the Monitoring module there are two services that check
respectively the retention policy and the blockchain integrity of the
Log Manager files. Each service check reads the verification output from
a cache that is periodically updated (:ref:`log-checks`); in
case of errors, the plugin output reports the timestamp of the last
verification execution.

A user can run the dedicated ``icingacli`` **verify** command and force
it to read from cache (both for retention policy and for integrity
verification) just adding the ``--cache`` option to it. For further
details use::

  icingacli logmanager consistency verifyBlockChain --help
  icingacli logmanager retentionpolicy verify --help

To carry out recurrent tasks, two systemd timers
are made available within the *Log Manager*
module and are activated during the installation:

1. **icingaweb2-module-logmanager-retention.timer**. This timer launches
   the command ``icingacli    logmanager retentionpolicy apply`` exactly
   at midnight; its purpose is to verify if the retention time for log
   files has been exceeded and applies the defined policy.
2. **icingaweb2-module-logmanager-extend-blockchain.timer**. The purpose
   of this timer is to build a blockchain for the log files saved on
   NetEye.

The NetEye Log Manager module also allows you to define server-based
filter and exclusion rules to optimize and restrict the content of
logged strings. Normally it would be better to already filter
unnecessary log content on the remote systems where the log stream is
generated in order to reduce network traffic and server processing time.
However, this is not always possible due to system and application
restrictions.

While it can be done for application-specific log content, this might
get more tricky if, e.g., the EventLog of a Microsoft system is read
out. Then there could be events with the same Event ID where some of
those events are of interest and others are not. In those cases where
you can't precisely filter all the required log strings on the remote
system, the NetEye Log Manager lets you define custom filters based on
white and/or black lists.
