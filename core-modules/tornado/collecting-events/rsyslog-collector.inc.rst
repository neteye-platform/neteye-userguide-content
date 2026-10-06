.. _tornado-rsyslog-collector-exec:

Rsyslog
~~~~~~~

The rsyslog Collector binary is an executable that generates Tornado
Events from rsyslog inputs.

The collector is pre-configured and is not to be started manually.

The example of the rsyslog event is:

.. code:: JSON

   {
      "type": "syslog",
      "created_ms": 1713881098196,
      "payload": {
         "@timestamp": "2024-04-23T16:04:58.016685+02:00",
         "facility": "daemon",
         "host": "myhostname",
         "message": "my-service.service: Failed with result exit-code.",
         "severity": "WARNING",
         "source": "systemd",
         "syslog-tag": "systemd[1]:"
      },
      "type": "syslog"
   }

.. rubric:: Disabling Rsyslog events coming from |ne| nodes

|ne| forwards the system log of each host to the Tornado event engine, where it
is processed as events of type syslog. This behavior is controlled by the
environment variable `TORNADO_RSYSLOG_COLLECTOR_ENABLED`, which is set for the
rsyslog service by the configuration file
:file:`/usr/lib/systemd/system/rsyslog.service.d/10-neteye-tornado.conf` shipped
with the NetEye Tornado package. The forwarding is enabled by default, meaning
no action is required if you want to keep this behavior.

To disable the forwarding, create the file
:file:`/etc/systemd/system/rsyslog.service.d/tornado-rsyslog-collector.conf`
with the following content:

.. code:: text

    [Service]
    Environment=TORNADO_RSYSLOG_COLLECTOR_ENABLED=false

Then apply the change:

.. code:: bash

    sudo systemctl daemon-reload
    sudo systemctl restart rsyslog

Note that the setting takes effect only after `systemctl daemon-reload` followed
by a restart of the rsyslog service; changing configuration files without these
steps has no effect on the running service.

.. warning::

    Disabling the forwarding stops system log events from appearing in Tornado.
    Event rules and dashboards that rely on syslog events will no longer receive
    data. To disable the forwarding, do not edit the package file
    :file:`/usr/lib/systemd/system/rsyslog.service.d/10-neteye-tornado.conf`, as
    it belongs to the NetEye Tornado package and is replaced on every update.

To ennable the forwarding again, remove the override file you created:

.. code:: bash

    sudo rm /etc/systemd/system/rsyslog.service.d/tornado-rsyslog-collector.conf
    sudo systemctl daemon-reload
    sudo systemctl restart rsyslog

With no override present, the package default (enabled) applies again.
