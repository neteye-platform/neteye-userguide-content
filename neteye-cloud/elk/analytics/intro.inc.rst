
.. _elk-analytics-overview:

Analytics
=========

The Analytics area of Kibana is where you work with the data your
|nec| tenant collects in Elasticsearch. Logs, events and metrics are
stored as indexed documents, and Analytics gives you the tools to search
them, look at them in detail and turn them into visual information you can
share.

This section covers the Analytics applications available in your tenant,
starting with the simplest way of working with data, and moving on to more
structured views:

* **Discover**: search and explore raw documents over a chosen time range,
  filter and sort results, inspect individual fields and save searches to
  reuse later. It also explains **data views**, which decide which indices or
  data streams each Analytics application works on.
* **Visualize Library**: build charts, tables and metrics from your data and
  store them as reusable visualizations.
* **Dashboards**: put several visualizations, saved searches and controls
  together in one view to keep track of the information that matters most
  to you.
* **Canvas**: make free-form, presentation-style workpads that combine live
  data with text and images.
* **Maps**: show geospatial data on interactive maps.
* **Machine Learning**: analyze data to detect anomalies and identify
  patterns automatically.

The search bar, filters and time picker work the same way in every Analytics
application. They're explained in detail in the Discover chapter, and the
later chapters build on that.

.. note::

   What you can do in each application depends on your ELK role. Users with
   the VIEWER role can view data and existing objects. EDITOR and ADMIN users
   can also create and change saved objects such as searches, visualizations
   and dashboards. Each chapter has a **Roles and Permissions** note that
   lists the operations allowed for each role.
