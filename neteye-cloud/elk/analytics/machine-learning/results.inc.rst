.. _elk-ml-job-results:

.. _elk-ml-results-analysis:

Investigating Anomalies
-----------------------

Once an anomaly detection job is running, you can review and investigate its results directly
in Kibana through two complementary views. The **Anomaly Explorer** gives an overview of
the anomalies detected by one or more jobs and shows the entities most affected. The
**Single Metric Viewer** focuses on a single metric series and compares its actual behavior
with the expected baseline calculated by the model. A typical analysis starts in the Anomaly
Explorer, where you find out when anomalies occurred and which services or transaction types
were involved. It then continues in the Single Metric Viewer to examine one specific metric in
detail. Both views use the same controls for job selection, time range, and data refresh, so
you can switch between them and keep the same context.

Anomaly Explorer
````````````````

The Anomaly Explorer is the main entry point for reviewing the anomalies detected by a job, for
example a job monitoring APM data. It helps you understand when anomalies occurred, which
services or transaction types were involved, and how severe the detected behavior was. To
start, select the job whose results you want to review and set the time range of interest.
You can change the job selection at any time, and a KQL filter lets you narrow the results
further, for example to a specific service or environment.

.. figure:: /neteye-cloud/elk/analytics/img/anomaly-explorer-web-results.png
   :alt: Anomaly Explorer showing top influencers, the anomaly timeline, and the anomalies table
   :align: center

.. rubric:: Reading the results

The view combines three elements, which are best read from the general to the specific:

**Top influencers**
   Lists the field values that contributed the most to the detected anomalies, such as
   specific ``service.name`` or ``transaction.type`` values. Use it to quickly identify
   where the investigation should start.

**Anomaly timeline**
   Shows the detected anomalies as a color-coded heatmap. The overall row summarizes the
   results of the whole job, while the rows below show individual influencer values, such
   as single services. The color intensity reflects the anomaly severity, so the time
   periods and entities with the most significant deviations stand out at a glance.

**Anomalies table**
   Lists each detected anomaly with its time, severity, detector, entity, influencers, and
   a short description. Comparing the **actual** value with the **typical** value
   calculated by the model shows how far the observed behavior differs from what was
   expected. From each record, you can take further actions, such as opening the related
   data for a more detailed analysis.

By combining a visual overview with detailed records, the Anomaly Explorer helps you identify
abnormal service behavior and supports troubleshooting activities.

Single Metric Viewer
````````````````````

The Single Metric Viewer analyzes one metric in detail and shows how it behaves over time
compared with the expected baseline. In addition to the job and time range, you select the
exact metric series to analyze by choosing the detector and the relevant entity values, such
as ``service.name`` and ``transaction.type``. This lets you focus on one specific service
context, for example the latency of a single transaction type of a given service.

.. figure:: /neteye-cloud/elk/analytics/img/single-metric-viewer.jpg
   :alt: Single Metric Viewer showing observed values, model bounds, anomaly markers, and anomaly records
   :align: center

.. rubric:: Actual versus expected behavior

The main chart shows the observed values of the selected metric together with the
**model bounds**, which represent the normal range expected by the model. Anomalies are
highlighted directly on the chart, so spikes or unusual values outside the expected range
are easy to spot. When available, the **Forecast** option projects the future behavior of
the metric based on the model. A compact timeline below the chart shows anomaly markers
across the full period, placing individual anomalies in a broader time context, while
**annotations** can mark relevant events, such as deployments or maintenance windows, that
may explain certain anomalies.

.. rubric:: Anomaly records

As in the Anomaly Explorer, the **Anomalies** table lists the records for the selected metric,
including time, severity, detector, entity, influencers, and description. Use the **actual**
and **typical** values to evaluate how much the observed behavior deviates from the expected
one. Together, the chart and the records give you a focused view of one metric series and
help you understand abnormal behavior in a specific service context.

For more details on viewing and interpreting anomaly detection results, refer to the
`official Elastic documentation <https://www.elastic.co/guide/en/machine-learning/current/ml-ad-view-results.html>`_.
