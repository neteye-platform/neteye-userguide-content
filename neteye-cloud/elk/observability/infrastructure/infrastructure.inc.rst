.. _elk-observability-infrastructure:

Infrastructure
~~~~~~~~~~~~~~

The **Infrastructure** section helps you monitor the health and
performance of hosts, containers and other monitored resources. Use it
to compare resource usage, spot unusual behavior and investigate the
systems involved.

It has three entries:

* **Infrastructure inventory**: compare resources in a centralized view
  to find those that need attention.
* **Metrics Explorer**: chart infrastructure metrics over time to
  investigate trends and compare resources.
* **Hosts**: inspect host-level performance and resource usage to narrow
  down infrastructure problems.

The available resources and metrics depend on the data collected by your
configured integrations. In each view, use the time picker and filters
to focus on the period and resources relevant to your investigation.

.. rubric:: Roles and Permissions

Depending on the role defined by the contract with your organization, you
might have different permissions:

* **VIEWER**: Read
* **EDITOR**: Create, Read, Update and Delete
* **ADMIN**: Create, Read, Update and Delete

Infrastructure Inventory
````````````````````````

Open :menuselection:`Infrastructure --> Infrastructure inventory` for an
overview of your monitored resources. The :guilabel:`Show` selector
chooses the resource type, such as hosts or containers, while
:guilabel:`Metric` chooses the value to compare. Use :guilabel:`Group by`
to organize resources by a shared attribute and :guilabel:`Sort by` to
change their order.

In the tile view, each tile represents a resource and displays its name
and selected metric value. Its color follows the scale on the right:
darker tiles indicate higher values in the illustrated CPU usage view,
not necessarily an unhealthy resource or an active alert. Compare the
values to identify resources with unusually high usage relative to their
peers.

.. figure:: /neteye-cloud/elk/observability/infrastructure/img/infrastructure-inventory.png
  :alt: Infrastructure inventory with resource selectors, CPU usage tiles and a host metric tooltip
  :align: center

Hover over a tile for a summary of CPU, memory and network usage. Select
a resource to access its details and performance charts, then check
whether the unusual value is a short-lived spike or a sustained pattern.
The view controls also let you switch from tiles to a table, and
:guilabel:`Auto-refresh` keeps the inventory updated during an
investigation.

The host detail panel summarizes CPU usage, normalized load, memory and
disk usage for its selected time range. Check the aggregation labels:
an *average* describes usage across the period, while a *maximum* highlights
the peak. The charts below show when changes occurred; use the
:guilabel:`Metrics` or :guilabel:`Logs` tabs to investigate further where
the corresponding data is available.

.. figure:: /neteye-cloud/elk/observability/infrastructure/img/inventory-host-details.jpg
  :alt: Host detail panel with summary metrics and CPU, memory, network and disk charts
  :align: center

Metrics Explorer
````````````````

Open :menuselection:`Infrastructure --> Metrics Explorer` to explore how
infrastructure metrics change over time. Select one or more metrics,
such as CPU, memory, disk or network usage, and an aggregation such as
:guilabel:`Average`. The :guilabel:`graph per` selector splits the data
by an attribute, allowing you to compare individual hosts or Kubernetes
nodes rather than viewing all matching resources together.

The CPU screenshot below has 3 values selected: ``system.cpu.total.norm.pct``,
``kubernetes.node.cpu.usage.node.pct`` and ``docker.cpu.total.pct``, with
:guilabel:`Average` as the aggregation and :guilabel:`Everything` as the
grouping. The colored metric labels identify the corresponding chart
lines; the horizontal axis shows time and the vertical axis shows the
metric values. Compare their trends, keeping in mind that host, node and
container metrics describe different resource scopes.

.. figure:: /neteye-cloud/elk/observability/infrastructure/img/metrics-explorer.png
  :alt: Metrics Explorer averaging three CPU percentage metrics over time without splitting by resource
  :align: center

To investigate individual Kubernetes nodes, for example, select
``kubernetes.node.cpu.usage.nanocores`` and group by
``kubernetes.node.name``. This metric measures CPU usage in nanocores
(one billionth of a CPU core), rather than as a percentage.
Use filters to isolate the nodes of
interest and look for spikes, sustained increases or differences between
nodes during the same period.

Hosts
`````

When you need a host-level perspective, open
:menuselection:`Infrastructure --> Hosts`. The host list and key CPU,
memory, disk and network metrics
help you identify systems whose resource usage stands out within the
selected period.

The summary cards give an overview of the matching hosts, while the
table breaks the values down by host. Check the column labels to
distinguish averages from maximum values. The :guilabel:`Metrics` tab
below the table compares host trends using a separate line for each
host; :guilabel:`Logs` and :guilabel:`Alerts` provide related context.

During an incident, filter the host list to the affected systems to
exclude unrelated activity. Select a host to inspect its performance
charts and check whether resource usage changed when the problem began.
Compare the timing of changes across
metrics: for example, a rise in CPU usage alongside increased network
traffic can help narrow the investigation to a workload change. Use the
charts to distinguish a brief peak from persistent resource pressure.

.. figure:: /neteye-cloud/elk/observability/infrastructure/img/hosts.jpg
  :alt: Hosts page with filters, summary metrics, a per-host resource table and comparative metric charts
  :align: center

.. seealso::

   * `Elastic infrastructure metrics reference
     <https://www.elastic.co/docs/reference/observability/metrics-reference>`__
