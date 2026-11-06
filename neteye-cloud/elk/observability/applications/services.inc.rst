Services
````````

Open :menuselection:`Applications --> Service inventory`. The
:guilabel:`Inventory` tab lists every service that has sent transactions
or spans in the selected time range, with its environment, average
latency, throughput and failed transaction rate, each with a sparkline of
the last period. Click a column header to sort, for example by failed
transaction rate to bring the least reliable services to the top, and use
the :guilabel:`Environment` selector when you only want to see production.

The :guilabel:`Comparison` option adds a grey line to every sparkline
showing the same indicator for an earlier period, such as the day or week
before. This is what makes the inventory useful as a routine check: on a
Monday morning, set the time range to the last 24 hours, compare with
:guilabel:`Week before`, and the services whose coloured line sits
visibly above the grey one are the ones that got slower or less reliable
over the weekend, without you having to remember what their normal values
are.

.. figure:: /neteye-cloud/elk/observability/applications/img/services-inventory.png
   :alt: Service inventory listing services with environment, latency, throughput and failed transaction rate
   :align: center

Clicking a service name opens its :ref:`Service overview
<elk-observability-service-overview>`.

.. note::

   A service appears in the inventory only if it has sent transactions or
   spans within the selected time range. Services that send only logs are
   not listed. If you expect a service and don't see it, widen the time
   range first.

**Service groups**

When the inventory grows long, the :guilabel:`Service groups` tab lets you
work with a subset. Each group is shown as a card with the number of
services it contains and, if any, the number of active alerts on them, so
a team can see the state of its own services without scrolling through
everyone else's.

.. figure:: /neteye-cloud/elk/observability/applications/img/services-groups.png
   :alt: Service groups tab with one card per group showing service count and active alerts
   :align: center

To create a group, click :guilabel:`Create group`, give it a name, a
colour and optionally a description, then click
:guilabel:`Select services` and enter a query on the services' attributes,
for example ``service.environment : "production"`` for all production
services or ``service.name : *beat`` for all services whose name ends in
*beat*. Services matching the query are assigned to the group
automatically, including ones that appear later.

.. figure:: /neteye-cloud/elk/observability/applications/img/services-create-group.png
   :alt: Create group dialog with name, colour and description fields and the Select services button
   :align: center
