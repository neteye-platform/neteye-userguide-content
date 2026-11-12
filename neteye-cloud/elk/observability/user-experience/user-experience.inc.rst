.. _elk-observability-user-experience:

User Experience (RUM)
~~~~~~~~~~~~~~~~~~~~~

The **User Experience** section uses **Real User Monitoring (RUM)** to
show how real users experience your web applications, including
responsiveness (such as page load times) and frontend errors. Unlike Synthetics, which
runs simulated checks, RUM analyzes data collected from the browser activity of actual users.

Use it to identify slow page loads, rendering delays and interaction
problems that backend or infrastructure metrics alone may not reveal.
The available views depend on the frontend telemetry your application
sends for monitoring.

.. rubric:: Roles and Permissions

The User Experience views are read-only for all roles defined by the
contract with your organization:

* **VIEWER**: Read
* **EDITOR**: Read
* **ADMIN**: Read

Filters
```````

Use the time picker and filters to focus the charts and tables on the
users and pages you want to investigate. You can narrow the view by
application or frontend service, environment, page or frontend
transaction, operating system, browser, device type and location, where
these attributes are available.

For example, if users report slow pages on mobile devices, filter by
device type and the affected page during the reported period. Compare
that view with other devices or browsers to see whether the slowdown is
specific to one group, or affects the application more broadly.

Page Performance
````````````````

The performance charts help you understand both the overall trend and
the differences between individual page loads:

* :guilabel:`Page load` summarizes total load time and its backend and
  frontend components for the selected percentile and period. Use the
  breakdown to see which part contributes most to load time.
* :guilabel:`Metrics` summarizes key frontend performance values, giving you a
  quick view of loading speed and responsiveness in the selected context.
* :guilabel:`Core Web Vitals` evaluates page loading, interactivity and visual
  stability. Use it to identify aspects of the experience that need
  attention, even when pages load successfully.
* :guilabel:`Page load distribution` shows how page load times are spread out.
  A concentration of fast load times with a smaller group of very slow ones
  can reveal problems that an overall average would hide.

.. figure:: /neteye-cloud/elk/observability/user-experience/img/observability-user-experience-tab.png
   :alt: User Experience overview with filters, page load metrics, Core Web Vitals and page load distribution
   :align: center

Traffic and User Context
````````````````````````

Use the traffic and visitor charts to understand who is affected and
whether performance changes coincide with changes in usage:

* :guilabel:`Total page views` shows traffic volume over the selected period.
  Compare it with loading performance to see whether slowdowns coincide
  with increased activity, although this alone does not establish the cause.
* :guilabel:`Page load duration by region` compares loading performance across
  geographic regions, helping you identify locations with slower loads.
* :guilabel:`Visitor breakdown` shows the browser and operating system mix.
  Use it to understand your audience, then filter the performance views
  to investigate a particular client environment.

.. figure:: /neteye-cloud/elk/observability/user-experience/img/observability-visitor-breakdown.png
   :alt: Page load distribution, page views, regional loading performance and visitor breakdown by browser and operating system
   :align: center

JavaScript Errors
`````````````````

The :guilabel:`JavaScript errors` chart highlights client-side errors and how
their frequency changes. Check whether an increase coincides with a
performance degradation or an application change, then use filters to
identify the affected pages, browsers or user groups. Errors can disrupt
an interaction even when the page itself loads quickly.

.. seealso::

   * `Real user monitoring (User Experience)
     <https://www.elastic.co/docs/solutions/observability/applications/user-experience>`__
