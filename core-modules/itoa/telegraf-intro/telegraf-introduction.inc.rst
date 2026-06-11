.. _telegraf_intro:

Telegraf Metrics in NetEye
~~~~~~~~~~~~~~~~~~~~~~~~~~

**Telegraf** is an agent written in Go for collecting, processing,
aggregating, and writing metrics. To use it in NetEye you will need to
install the *telegraf* package using *yum*.

Telegraf is entirely plugin-driven and has 4 distinct plugin types:

-  **Input Plugins:** Collect metrics from the system, services, or 3rd
   party APIs
-  **Processor Plugins:** Transform, decorate, and/or filter metrics
-  **Aggregator Plugins:** Create aggregate metrics (e.g. mean, min,
   max, quantiles, etc.)
-  **Output Plugins:** Write metrics to various destinations

For more information about Telegraf please refer to the `official
documentation on
GitHub <https://github.com/influxdata/telegraf/tree/1.5.3/docs>`__.
