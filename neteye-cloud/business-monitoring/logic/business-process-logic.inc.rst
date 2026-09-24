Business Process Logic
~~~~~~~~~~~~~~~~~~~~~~

A **Business Process** is a high-level logical service that groups together multiple **monitored objects**
(and, in some cases, smaller business processes) that are interrelated through logical operations.
In the cloud service, these monitored objects are **Icinga objects**, that stay inside the monitoring perimeter
defined during the onboarding phase.

The overall state of a business process is determined by applying the status of each monitored object
to the process's logical expression. By treating a business process itself as a monitored object, it becomes possible to calculate its availability,
design more complex monitoring logic, and build Grafana dashboards that visualize service health in a business-oriented way.
