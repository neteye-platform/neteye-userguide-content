.. _tornado-monitoring-statistics:

Tornado Monitoring and Statistics
+++++++++++++++++++++++++++++++++

Tornado Engine performance metrics are exposed via Tornado APIs and periodically collected by a dedicated
telegraf instance `telegraf_tornado_monitoring.service`.
Metrics are stored into the database *master_tornado_monitoring* in InfluxDB.

Tornado Monitoring and Statistics gives an insight about what data Tornado is processing and how.
These information can be useful in several scenarios, including workload inspection, identification of bottlenecks,
and issue debugging.
A common use case is to identify performance-related issues: for example a difference between the amount of
events received and events processed by Tornado may identify a performance problem because Tornado does not have
enough resources to handle the current workload.

Examples of collected metrics are:

- **events_processed_counter:** total amount of event processed by Tornado Engine
- **events_received_counter:** total amount of events received by Tornado Engine through all Collectors
- **actions_processed_counter:** total amount of actions executed by Tornado Engine

Metrics will be automatically deleted according to the selected retention policy.


The user can configure Tornado Monitoring and Statistics via GUI under
:menuselection:`Configuration / Modules / Tornado / Configuration`. Two parameters are available:

- **Tornado Monitoring Retention Policy:** defines the number of days for which metrics are retained in InfluxDB and
  defaults to **7 days**, after which data will be no longer available.
- **Tornado Monitoring Polling Interval:** sets how often the Collector queries the Tornado APIs to gather metrics and
  defaults to **5 seconds**.

To apply changes you have to either run :command:`neteye install` for both options or execute
:command:`/usr/share/neteye/tornado/scripts/apply_tornado_monitoring_retention_policy.sh` or
:command:`/usr/share/neteye/tornado/scripts/apply_tornado_monitoring_polling_interval.sh`, according to
the parameter changed.

.. note:: On a NetEye Cluster, execute the command on the node where ``icingaweb2`` is active.
