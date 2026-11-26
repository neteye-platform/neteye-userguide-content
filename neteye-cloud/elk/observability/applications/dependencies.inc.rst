Dependencies
````````````

**Dependencies** is where you analyze the external systems and downstream
services your applications depend on. Whereas the Services inventory shows
the services you run, this view shows what they call: databases, caches,
message queues, external APIs and other backend components, all in one list
regardless of which service is calling them.

Open :menuselection:`Applications --> Dependencies`. The table lists each
dependency with the average latency, throughput and failed transaction
rate of the calls made to it, along with its impact. Next to each value a
sparkline, a small inline chart, shows how the metric evolved over the
selected time range; the :guilabel:`Comparison` option adds a grey line to
each sparkline showing the previous period, as in the Services inventory.
Click a column header to sort, and a dependency name to see which services
call it.

.. figure:: /neteye-cloud/elk/observability/applications/img/dependencies.png
   :alt: Dependencies view listing dependencies with latency, throughput, failed transaction rate and impact
   :align: center

For each dependency you can:

* **See who calls it and how it performs.** Latency, throughput and
  failed transaction rate of the calls made to it, and the services that
  make them.
* **Measure its impact.** How much of your services' total time is spent
  waiting for this dependency. The table is sorted by impact by default,
  so the systems that cost you the most response time are at the top.
* **Find out whose fault it is.** If a database or API is slow for all its
  callers, the problem is likely on the external side; if only one service suffers, it's
  more likely how that service is using it.
* **Discover what you're calling.** The list shows every external system
  your applications actually talk to, including ones you may not expect,
  such as a third-party API still used by an old module.

**Example:** three unrelated services all show higher latency one
morning. Checked one by one they have nothing in common; sorted by impact
on this page, a single Postgres instance sits at the top with three times its
usual latency, and all three services turn out to be among its callers.

.. note::

   If a dependency isn't an instrumented service of yours, this is the
   only place where its behavior becomes visible at all.
