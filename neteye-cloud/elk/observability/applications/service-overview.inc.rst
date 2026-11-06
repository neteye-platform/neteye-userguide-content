.. _elk-observability-service-overview:

Service Overview
````````````````

The **Service overview** is the page of a single service and the place
where most troubleshooting happens. Its tabs look at the same service from
different angles and share the time range, environment and
:guilabel:`Comparison` setting you select, so you can move between them
without losing context.

**Overview** summarizes the health of the service: latency, throughput and
failed transaction rate over time, the most frequent transactions, the
most common errors and the dependencies it relies on. It also lists the
service's **instances**, that is, the individual processes or containers
running it, each with its own indicators and CPU and memory usage. When
the service as a whole looks fine but a few users complain, this table is
where you find out that one instance out of five is at 99% memory and
answering slowly.

.. figure:: /neteye-cloud/elk/observability/applications/img/service-overview-instances.png
   :alt: Instances table of a service with latency, throughput, failed transaction rate, CPU and memory usage per instance
   :align: center

**Transactions** lists the operations the service performs, such as the
endpoints of an API or the jobs of a worker, each with its own indicators,
and shows four charts for the selected transaction type: latency,
throughput, failed transaction rate and **time spent by span type**. The
last one splits the latency into what the service did itself and what it
spent waiting on HTTP calls, database queries and so on. Say the checkout
service has become slow. Here you see that the latency chart has a single
sharp spike at 22:20 rather than a gradual rise, that throughput didn't
change at that moment, and from the span type breakdown whether the extra
time went into the application code or into external calls. That already
tells you whether to look at a deployment, at a dependency or at the
service's own resources.

.. figure:: /neteye-cloud/elk/observability/applications/img/service-overview-transactions.png
   :alt: Transactions tab with latency, throughput, failed transaction rate and time spent by span type charts
   :align: center

**Dependencies** shows the external systems and downstream services this
service calls, with a chart of the time spent in each dependency and a
table ranking them by impact. Continuing the example: if the span type
breakdown pointed to external calls, this chart shows which dependency
produced the spike. A single dependency peaking while the others stay flat
tells you the problem is in that database or downstream service, not in
the service you started from.

.. figure:: /neteye-cloud/elk/observability/applications/img/service-overview-dependencies.png
   :alt: Dependencies tab with the time spent by dependency chart and the dependencies table ranked by impact
   :align: center

**Errors** groups the errors captured for the service by type and
message, with when each was last seen and how often it occurred, next to
the failed transaction rate over time. Open a group to see a sample
occurrence with its stack trace and the transaction it happened in. The
occurrence counts are the quickest way to tell a new error that appeared
with the last release from a known one that has been there for months.

.. figure:: /neteye-cloud/elk/observability/applications/img/service-overview-errors.png
   :alt: Errors tab with the failed transaction rate chart and the list of error groups with occurrences
   :align: center

**Metrics** (named **JVMs** for Java services) shows the resource
consumption of the service's runtime, such as CPU, memory and garbage
collection, as reported by the APM agent. Use it when you suspect that the
service itself, rather than a dependency, is running out of resources.

**Service Map** shows this service in the middle, with the services that
call it on the left and the services and systems it calls on the right.
Use :guilabel:`Hops` to follow the connections further and
:guilabel:`Max visible` to limit the number of nodes; click
:guilabel:`Explore in Service map` to open the full map centred on this
service, as described in :ref:`elk-observability-service-map`.

.. figure:: /neteye-cloud/elk/observability/applications/img/service-overview-service-map.jpg
   :alt: Service Map tab showing the service with its upstream and downstream connections
   :align: center

**Logs** shows the log entries written by the service in the selected time
range, so you can read what the application reported at the moment a
transaction was slow or failed, without leaving the page.
