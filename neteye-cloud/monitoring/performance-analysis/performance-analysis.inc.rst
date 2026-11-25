.. _nec-performance-analysis-section:

Performance Analysis and ITOA
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

IT Operations Analytics (ITOA) is the analysis of operational data from
systems, applications, and networks. In |nec|, the ITOA module presents this
data in interactive graphs and dashboards, helping you spot trends and
bottlenecks, investigate performance issues, and plan capacity improvements.
For more on the analytics components and how they fit together, see the
:ref:`ITOA module overview <itoa-module-description>`.

|nec| collects performance measurements, such as response times and CPU usage,
from monitoring checks and, where configured, from agents and other sources.
See :ref:`Active Monitoring <active_monitoring>` for an introduction to how
NetEye monitors hosts and services.
These measurements are stored over time, allowing you to examine historical
behavior rather than only the current status. You can select a time range and
drill down into the metrics for an individual host or service to understand
what changed and when.

Accessing Historical Performance Data
`````````````````````````````````````

To understand how a monitored host or service changed over time, open its
details and review the **Performance Graph** for a quick view of recent
measurements. For a longer period or a closer comparison, open an interactive
analytics dashboard from the main navigation. See the ITOA
:ref:`dashboard overview <itoa-dashboards-item>` for more about dashboards
and their panels.

Choose a dashboard for the monitored resource, select the host, service, or
metric if the dashboard provides these filters, and set the time range to
cover the period you want to investigate. Use the chart's time controls to
narrow the interval and compare measurements before and after an event, such
as a rise in response time or resource usage.

Historical charts show only performance data that was collected and is still
available in storage. If the expected measurements are missing, check that the
relevant monitoring check is producing performance data and that your account
has access to the dashboard.


Building a Grafana Dashboard
````````````````````````````

A Grafana dashboard brings related measurements together in *panels*, each
with its own query and visualization. For example, a **Host Performance**
dashboard can show CPU usage and service response times side by side, helping
you investigate whether increased resource usage coincided with slower
responses.

Before you begin, choose a host whose checks already collect the measurements
you want to display. Your account needs dashboard-editing access in the
appropriate Grafana organization, normally the **Editor** or **Admin** role,
and permission to save dashboards in the chosen folder. If the **ITOA** menu
or editing controls are unavailable, ask your administrator to check your
:ref:`access permissions <itoa-user-permissions-conf>`.

#. In the |nec| sidebar, select **ITOA** to open the integrated Grafana
   interface using your NetEye login. If you can access multiple
   organizations, switch to the organization associated with the tenant whose
   data you want to analyze. Dashboards and data sources belong to an
   organization; selecting the wrong one can hide the data you need.

   .. list-table::

      * - [Screenshot: NetEye ITOA entry and the selected Grafana organization.]

          |
          |
          |

#. In Grafana, open **Dashboards**, then select **New > New Dashboard**.
   Click **+ Add visualization** and choose an existing data source containing
   the host's performance measurements. Use the source configured for your
   environment rather than assuming that the default source contains your
   metrics. For details about available sources and their query editors, see
   :ref:`ITOA data sources <itoa-datasources>`. If the required source is
   missing, ask your administrator to configure it.

   .. list-table::

      * - [Screenshot: New dashboard and the available performance data sources.]

          |
          |
          |

#. In the panel editor's **Queries** tab, build a query for CPU usage on the
   selected host. The editor and query language depend on the data source.
   For an InfluxDB source, for example, select the measurement and field that
   contain CPU usage, and filter by the host and service tags where present.
   Use the actual names in your data source, not the dashboard's display
   labels, and ensure that the query uses the dashboard's selected time range.

   Select a period when the check was collecting data and click **Refresh**
   to preview the results. If no data appears, check the selected source,
   metric, filters, and time range before continuing.

   .. list-table::

      * - [Screenshot: Panel query filtered to one host, with returned CPU data.]

          | 
          |
          |

#. Choose the **Time series** visualization. Under **Panel options**, give
   the panel a descriptive title such as **CPU Usage**. Under **Standard
   options**, select a unit matching the stored values: for CPU percentages,
   distinguish between values on a 0-100 scale and fractions on a 0-1 scale.
   Use the legend to identify the displayed series. See the
   :ref:`panel overview <itoa-panels>` for more about panel queries and
   visualizations.

   You can add visual thresholds to highlight unusual values, but these do
   not change NetEye's monitoring checks or notification rules.

   .. list-table::

      * - [Screenshot: CPU time-series preview with panel title, unit, and
          legend.]

          |
          |
          |

#. Click **Back to dashboard**. To add another panel, select
   **Add > Visualization** in the dashboard header. Repeat the query and
   visualization steps for a service response-time metric on the same host,
   selecting the correct time unit for its stored values. Return to the
   dashboard and arrange or resize the panels so that you can compare them.

#. Click **Save dashboard**, enter a title such as **Host Performance**, and
   select a folder your intended audience can access. Add a description
   identifying the host and purpose of the dashboard, then click **Save**.
   Click **Exit edit** when you have finished.

   .. list-table::

      * - [Screenshot: Save dialog with dashboard title, description, and
          folder.]

          |
          |
          |

#. Reopen the saved dashboard from **Dashboards**. Use the time picker to
   select a relative interval, such as the last 24 hours, or an absolute
   interval around an incident. Check that both panels show measurements for
   the intended host and period. As you narrow the interval, compare the CPU
   trend with response times to investigate the change.

   .. list-table::

      * - [Screenshot: Saved host-performance dashboard with two panels and a
          selected historical time range.]

          |
          |
          |

Saving a dashboard in Grafana does not automatically add it to a monitored
object's **Performance Graph**. To make it available there, an administrator
can configure a :ref:`mapping to a service or checkcommand
<grafana-performance-mapping>`.


Drilling Into Metrics with Queries
``````````````````````````````````

Open **ITOA** in the NetEye sidebar, then find **Host Performance** under
Grafana's **Dashboards**. Each panel displays the results of its saved query.
You can inspect these results and change the displayed period without editing
the dashboard or its queries.

Start with the **CPU Usage** graph. Use the dashboard time picker to select
a broad interval, such as **Last 24 hours**, and look for peaks or sustained
increases. For an older incident, enter its date and times in the **From**
and **To** fields and click **Apply time range**. Check the displayed time
zone before comparing the graph with an incident timestamp. Hover over the
graph to read a measurement's time and value, and use the legend to focus on
the relevant series.

.. list-table::

   * - [Screenshot: CPU Usage graph with a selected time range and a value
       tooltip.]

       |
       |
       |

To investigate a peak more closely, click and drag horizontally across the
graph to select the interval around it. Grafana narrows the dashboard time
range and refreshes panels that follow that range. Include some time before
and after the peak so that you can see the baseline and recovery. Use the
**Zoom out** control or the time picker to return to a broader view; zooming
does not change the saved query definitions.

To drill down into two graphs simultaneously, keep **CPU Usage** and the
service response-time panel visible on the same dashboard. Confirm that both
queries select the intended host and follow the dashboard's time range.
Use the same displayed time zone and avoid panel-specific time shifts or
query filters that select a different period. If necessary, ask a dashboard
editor to correct these settings and arrange the panels so their time axes
are easy to compare.

For synchronized inspection, an editor can select **Edit > Settings**, open
the **General** tab, and set graph tooltip behavior under **Panel options**
to **Shared tooltip**. This displays a crosshair and tooltips across graph
panels when you hover over one of them. **Shared crosshair** instead shares
only the time marker. Save the dashboard and exit edit mode before continuing;
if you have view-only access, ask the dashboard owner to enable this setting.

.. list-table::

   * - [Screenshot: Dashboard settings with Shared tooltip selected.]

       |
       |
       |

For example, suppose a service became slow at 14:05. Select an absolute
interval from 14:00 to 14:15 on the incident date. You might see CPU usage rise
from 25% to 95% while response time increases from 100 ms to 800 ms. Drag over
the interval around 14:05 in either graph to zoom both together, then move
the pointer along one graph: the shared marker lets you inspect the other
panel at the corresponding time. Compare when each increase begins and
whether both return to normal together.

.. list-table::

   * - [Screenshot: Time-aligned CPU and response-time graphs with a shared
       marker and tooltips around the example incident.]

       |
       |
       |

Treat simultaneous changes as a clue, not proof that one caused the other.
Check whether response times rose before or after CPU usage, and compare the
same period with NetEye's monitoring history or other available evidence.
Measurements may have different collection intervals, and query aggregation
can smooth short spikes; inspect tooltip timestamps rather than assuming
every displayed point represents an identical sampling instant. Zooming
cannot reveal measurements that were never collected.
