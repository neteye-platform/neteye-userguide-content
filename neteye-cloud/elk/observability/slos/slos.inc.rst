.. _elk-observability-slos:

Service Level Objectives (SLOs)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The **SLOs** section is where you define reliability targets for your
services and check whether they're being met. Alerts tell you about
individual incidents; an SLO tells you how a service has performed over a
period against a target you agreed on, such as "99.5% of checkout requests
succeed" or "99% of page loads complete within 300 ms". This turns a
service level agreement or an internal goal into a single, continuously
evaluated number you can act on and report.

How an SLO is Built
```````````````````

Every SLO comes down to three decisions. Make them before you open the
wizard and the setup is straightforward.

**What to measure.** The *service level indicator* (SLI) is the proportion
of good events out of all events. Good can mean a successful HTTP
response, a transaction under a latency threshold, a synthetic check that
returned *up*, or any condition you can express as a query.

**How good is good enough.** The *target* is a percentage, for example
99.5%. What's left (``100% - target``) is the *error budget*: the share of
events allowed to be bad before the SLO is breached.

**Over which period.** A **rolling** time window (for example the last 30
days) always looks back from now and suits day-to-day operations. A
**calendar aligned** window (for example each month) resets on fixed dates
and suits contractual SLAs and periodic reporting.

.. rubric:: Roles and Permissions

Depending on the role defined by the contract with your organization, you
might have different permissions:

* **VIEWER**: Read
* **EDITOR**: Create, Read, Update and Delete
* **ADMIN**: Create, Read, Update and Delete

Create an SLO
`````````````

Open :menuselection:`Observability --> SLOs` and click
:guilabel:`Create SLO` (or :guilabel:`Create new SLO` if SLOs already
exist). The wizard has three steps.

#. **Define the SLI.** From :guilabel:`Choose the SLI type`, pick the type
   that matches your data:

   * :guilabel:`APM latency` / :guilabel:`APM availability`: for services
     monitored with APM. Select the service, environment and optionally a
     transaction name; for latency, also the threshold in milliseconds.
   * :guilabel:`Custom Query`: for log or event documents. Select the data
     view and timestamp field, then write a :guilabel:`Good query` (for
     example ``http.response.status_code < 500``) and a
     :guilabel:`Total query` (for example ``http.response.status_code : *``).
   * :guilabel:`Custom Metric`: as Custom Query, but good and total are
     aggregations of numeric fields rather than document counts.
   * :guilabel:`Histogram Metric`: for values stored in histogram fields,
     typically latency distributions.
   * :guilabel:`Timeslice Metric`: for an equation over several
     aggregations evaluated per time slice; requires the *Timeslices*
     budgeting method.
   * :guilabel:`Synthetics availability`: for the uptime of synthetic
     monitors, selected by name, project or tag.

   Use :guilabel:`Group by` (for example on ``service.name``) to get one
   SLO per value of a field instead of a single aggregated one.

   .. figure:: /neteye-cloud/elk/observability/slos/img/create-slo-define-sli.png
      :alt: Define SLI step with the Choose the SLI type dropdown open
      :align: center

#. **Set the objectives.** Choose the :guilabel:`Time window` (rolling or
   calendar aligned) and its :guilabel:`Duration`, the
   :guilabel:`Budgeting method` and the :guilabel:`Target / SLO (%)`. For
   the budgeting method, **Occurrences** counts good events against total
   events and is the usual choice; **Timeslices** splits the window into
   slices of fixed duration and counts good slices against total slices,
   so that a short outage weighs the same regardless of how much traffic
   it affected.

   .. figure:: /neteye-cloud/elk/observability/slos/img/create-slo-set-objectives.png
      :alt: Set objectives step with time window, duration, budgeting method and target
      :align: center

#. **Describe the SLO.** Give it an :guilabel:`SLO Name` that identifies
   the service and what is measured (for example ``[Production] Checkout
   availability``), a :guilabel:`Description`, and :guilabel:`Tags` such as
   environment or team for filtering later. Optionally pick
   :guilabel:`Linked dashboards` to reach related dashboards from the SLO.
   Click :guilabel:`Create SLO`.

   .. figure:: /neteye-cloud/elk/observability/slos/img/create-slo-describe.png
      :alt: Describe SLO step with name, description, tags and linked dashboards, and the Create SLO button
      :align: center

.. note::

   The SLO is computed from the moment it's created. With a rolling 30-day
   window, status and error budget reflect a shorter period until 30 days
   of data are available.

Monitor your SLOs
`````````````````

The SLOs overview shows one card per SLO (or per group value). Use the
:guilabel:`Status` and :guilabel:`Tags` filters and the search bar to
narrow the list, and :guilabel:`Sort by` or :guilabel:`Group by` to
organize it. Each card shows:

* **Status**: *Healthy* if the observed SLI is at or above the target,
  *Violated* if below, *Degrading* if the error budget is being consumed
  faster than planned. An alert badge appears if an alert is active for
  the SLO.
* **SLI type, tags and time window** of the SLO.
* **Observed value**: the current SLI percentage against the target, with
  its trend.
* **Budget remaining**: how much of the error budget is left in the
  current window. A negative value means the budget is exhausted and the
  SLO is violated.

.. figure:: /neteye-cloud/elk/observability/slos/img/slos-overview.png
   :alt: SLOs overview with one card per SLO showing status, observed value and budget remaining
   :align: center

Click a card to open the SLO details with the SLI and error budget charts
over time, the burn rate (how fast the budget is being spent: at 1 it runs
out exactly at the end of the window, at 10 in a tenth of it), the related
alerts, and actions to edit, clone, reset or delete the SLO. To show an SLO
next to other data, add an **SLO Overview** panel to a dashboard (see
:ref:`elk-dashboards`).

.. note::

   Instead of alerting only when an SLO target is breached, you can
   configure an **SLO burn rate** alert rule. It triggers when the error
   budget burns at an unsustainable rate, for example 10% of a 30-day
   budget in under an hour, so that you can address the degradation before
   the target is violated. See :ref:`elk-observability-alerts` for rules
   and connectors.

.. seealso::

   * `Elastic Service Level Objectives (SLOs) documentation
     <https://www.elastic.co/docs/solutions/observability/incident-management/service-level-objectives-slos>`__
   * `Elastic guide to creating an SLO
     <https://www.elastic.co/docs/solutions/observability/incident-management/create-an-slo>`__
