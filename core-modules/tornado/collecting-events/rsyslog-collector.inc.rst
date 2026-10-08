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

|ne| forwards the logs handled by rsyslog to the Tornado event engine, where
they are processed as events of type `syslog`. These include the system logs of
the |ne| node itself and, if rsyslog is configured to receive them, the logs
sent by other hosts.

The forwarding of the node's own logs is controlled by the environment variable
`TORNADO_RSYSLOG_FORWARD_LOCAL_LOGS`, which is set for the rsyslog service by
the configuration file
:file:`/usr/lib/systemd/system/rsyslog.service.d/10-neteye-tornado.conf`. A log
is considered local when it reaches rsyslog from the loopback address
(`127.0.0.1` or `::1`), which is the case for all logs generated on the node.
Forwarding is enabled by default, meaning no action is required if you want to
keep this behavior. Logs received from other hosts are always forwarded,
regardless of this setting.

To disable forwarding of the node's own logs, first create the directory
:file:`/etc/systemd/system/rsyslog.service.d` if it does not exist yet:

.. code:: bash

    sudo mkdir -p /etc/systemd/system/rsyslog.service.d

Then create the file
:file:`/etc/systemd/system/rsyslog.service.d/tornado-rsyslog-collector.conf`
with the following content:

.. code:: text

    [Service]
    Environment=TORNADO_RSYSLOG_FORWARD_LOCAL_LOGS=false

Finally, apply the change:

.. code:: bash

    sudo systemctl daemon-reload
    sudo systemctl restart rsyslog

Note that the setting takes effect only after `systemctl daemon-reload` followed
by a restart of the rsyslog service; changing configuration files without these
steps has no effect on the running service.

.. warning::

    Disabling forwarding stops the system log events of the |ne| node from
    appearing in Tornado. Event rules and dashboards that rely on these events
    will no longer receive data; events from other hosts are not affected. When
    disabling forwarding, do not edit the package file
    :file:`/usr/lib/systemd/system/rsyslog.service.d/10-neteye-tornado.conf`, as
    it belongs to the |ne| Tornado package and is replaced on every update.

To re-enable forwarding of the node's own logs, remove the override file you
created:

.. code:: bash

    sudo rm /etc/systemd/system/rsyslog.service.d/tornado-rsyslog-collector.conf
    sudo systemctl daemon-reload
    sudo systemctl restart rsyslog

With no override present, the package default (enabled) continues to apply.
