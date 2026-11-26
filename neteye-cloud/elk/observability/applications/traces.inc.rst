Traces
``````

A trace is the record of a single request as it travels through your
services: the transaction that received it, the spans it executed and the
transactions it triggered in other services, each with its duration and
outcome. **Traces** lists the transactions collected across all services
in the selected time range, so you can go from an aggregated indicator to
one concrete request and see precisely what happened.

Each row shows the transaction name, the service it originated in, its
average latency, how many traces per minute it produces and its
**impact**. Impact is latency multiplied by throughput: a transaction that
is both slow and frequently called has a high impact, while a slow one
that is rarely called, or a fast one that is called constantly, ranks
lower.

The list is sorted by impact by default, which puts the
transactions whose optimization would benefit users most at the top. This
is the list to start from when you have a performance budget and need to
decide what to work on first, rather than when you're chasing a specific
incident.

.. figure:: /neteye-cloud/elk/observability/applications/img/traces.png
   :alt: Traces list with transaction name, originating service, latency, traces per minute and impact
   :align: center

Click a transaction to open its trace: a timeline of every span in the
order it ran, nested under the span that called it, with the service each
span belongs to, its duration and any error it produced. Reading it from
top to bottom takes you to the step responsible for the latency or the
failure, even when that step ran in a different service from the one
where the symptom appeared.
