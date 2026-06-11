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
