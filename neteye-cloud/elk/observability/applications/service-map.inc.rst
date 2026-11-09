.. _elk-observability-service-map:

Service Map
```````````

The **Service Map**, reached from the :guilabel:`Service map` tab of the
service inventory, draws a graph of all instrumented services in the selected
environment and the connections between them as observed in the traces:
if service A called service B, there is a line from A to B. Databases,
caches and external hosts appear as well, so the map shows the whole path
a request can take.

Use it to get the picture of an architecture you're not familiar with, or
to answer questions that are hard to answer from lists: which services are
affected if this database is slow, which service sits in the middle of
most request paths, and whether a connection exists that shouldn't. Click
a node to open the Service overview of that service, and use the
environment selector and the search bar to reduce the map to the part you
care about.

.. figure:: /neteye-cloud/elk/observability/applications/img/service-map.png
   :alt: Service map showing instrumented services, databases and external hosts with the connections between them
   :align: center

The map is built from distributed traces. A service that isn't
instrumented, or doesn't propagate the trace context, appears without
connections or not at all.

.. seealso::

   * `Elastic Services inventory documentation
     <https://www.elastic.co/docs/solutions/observability/apm/services>`__
   * `Elastic Service overview documentation
     <https://www.elastic.co/docs/solutions/observability/apm/service-overview>`__
   * `Elastic Traces documentation
     <https://www.elastic.co/docs/solutions/observability/apm/traces-ui>`__
   * `Elastic Dependencies documentation
     <https://www.elastic.co/docs/solutions/observability/apm/dependencies>`__
   * `Elastic Service map documentation
     <https://www.elastic.co/docs/solutions/observability/apm/service-map>`__
