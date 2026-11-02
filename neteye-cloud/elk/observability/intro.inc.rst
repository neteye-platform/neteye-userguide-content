
.. _elk-observability-overview:

Observability
=============

In |nec|, Observability extends traditional monitoring by providing
real-time visibility into the health, performance and behavior of complex,
distributed environments. Powered by the Elastic Stack, it unifies separate
telemetry streams into a single operational view, allowing you to move from
detecting symptoms to identifying the root cause of an issue.

While **Analytics** focuses on exploratory search, data visualization and
reporting, **Observability** provides purpose-built workflows centered
around system reliability and service health.

|nec| Observability correlates three fundamental data types across
infrastructure and applications:

* **Logs**: timestamped records of discrete events emitted by systems,
  containers and applications, used for forensic analysis and real-time
  troubleshooting.
* **Metrics**: numerical time-series data measuring resource utilization and
  performance indicators (such as CPU, memory, latency and throughput) to
  detect trends and capacity issues.
* **Traces (APM)**: end-to-end transaction journeys through distributed
  services that pinpoint latency bottlenecks, errors and failure points in
  application code.

The Observability module integrates these telemetry sources into unified
workflows:

Infrastructure Monitoring
  Track the operational status and resource consumption of hosts,
  containers and cloud services.

Application Performance Monitoring (APM)
  Instrument services to trace user requests, map service dependencies and
  profile performance.

Uptime & Synthetic Monitoring
  Proactively probe network endpoints and critical user journeys to verify
  availability from external viewpoints.

Alerting & Rule Management
  Configure dynamic rules across logs, metrics and thresholds to detect
  anomalies and trigger automated notifications before incidents impact
  users.

The primary value of the Observability interface is context-preserving
correlation. When investigating an anomaly, such as a sudden spike in
service latency, you can pivot seamlessly from an infrastructure metric
alert directly into the relevant application traces and container logs from
the exact same timeframe without losing search context.

The following chapters describe the dedicated interfaces, configuration
steps and common operational workflows for each Observability application.
