
El Proxy performance metrics can be useful for inspecting the workload, identifying bottlenecks, and
debugging issues. For more info about the individual metrics collected by El Proxy, please refer to the
:ref:`ebp-rest-endpoints` section.

You can configure the parameters for retrieving and storing metrics via GUI under
:menuselection:`Configuration / Modules / kibana / Configuration`. There are two parameters available:

- **El Proxy Monitoring Retention Policy:** defines the number of days for which metrics are retained in InfluxDB and
  defaults to **7 days**, after which data will no longer be available.
- **El Proxy Monitoring Polling Interval:** sets how often the Telegraf agent queries El Proxy APIs to gather
  metrics and defaults to **2 seconds**.

To apply changes, you can either run :command:`neteye install` for both options or execute
:command:`/usr/share/neteye/kibana/scripts/apply_elproxy_monitoring_retention_policy.sh` or
:command:`/usr/share/neteye/kibana/scripts/apply_elproxy_monitoring_polling_interval.sh`, depending on the parameter
to be changed.

.. note:: On a NetEye Cluster, execute the command on any PCS node.

NetEye also provides an out-of-the-box solution for visualizing the metrics, which consists of a set of dashboards
that allow you to analyze the work of El Proxy.
To check out the dashboards please click on the ITOA module and navigate to the **El Proxy** folder of the Grafana
Organization **Main Org.**. Here you will see dashboards regarding troubleshooting and performance of El Proxy.

El Proxy dashboards rely on the **ElProxyMonitoring** Grafana Data Source,
which is also automatically created by NetEye and you should never modify it manually.

.. note:: The dashboards present in El Proxy folder and the **ElProxyMonitoring** Data Source are managed by NetEye
   and you should never modify them manually because they will be overwritten by NetEye itself. If you would like
   to modify one of the dashboards, first you should copy it and then modify a copied version.
