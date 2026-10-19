.. _elk-getting-started:

Getting Started with ELK
~~~~~~~~~~~~~~~~~~~~~~~~

NetEye.Cloud includes **Log Analytics**, which is based on the Elastic Stack.
Elasticsearch stores and indexes the data collected from your environment, such
as logs, metrics and traces. Kibana is the interface you use to work with that
data. This chapter explains how to open Log Analytics and how its features are
organised, so you know where to go for each task. Each feature has its own
chapter with more detail.

.. _elk-opening-log-analytics:

Opening Log Analytics
`````````````````````

Log in to NetEye Cloud (``https://neteye.cloud``). On the start screen, click
**Log Analytics**, which opens Kibana on the Log Analytics home page.

.. figure:: /neteye-cloud/elk/img/log-analytics.png
   :alt: Log Analytics home page

   Log Analytics home page

The home page shows the two main areas of Log Analytics: **Analytics** and
**Observability**. Click either one to open it. To go straight to a specific
feature, open the menu (☰) in the top-left corner. It lists the features of
each area.

The two areas use the same data but for different purposes. In Analytics, you
explore the data and present it: you search, filter, aggregate and visualise
it. Observability is organised around the systems, applications and services
you monitor. It shows their health and performance and helps you troubleshoot
them. In daily work you'll often use both. For example, you might notice a
problem in Observability and then look at the underlying data in more detail
in Analytics.

.. tip::

   Most views include a time range selector. The data you see always depends
   on the selected period. If a view shows less data than you expect, check the
   time range first.

.. _elk-analytics:

Analytics
`````````

Analytics is where you turn the data collected in Elasticsearch into
information you can read and interpret. You use it to search events, logs,
metrics and other indexed data, and to filter them by time range, field or
value. You can also look at trends, distributions and comparisons, and build
the charts, tables and dashboards used for analysis and reporting.

**Discover** is usually your starting point. It gives you direct access to the
raw data stored in Elasticsearch. You can look at individual records, inspect
their fields and apply filters to narrow the results. You can also follow how
events are distributed over the selected time range. Use Discover for quick
investigations, for example to check whether a specific event was recorded. It
also helps you understand what an index contains before you build anything on
top of it.

**Visualize Library** is where you create and manage visualizations, such as
charts, tables and metrics. Visualizations are saved in the library, so you can
reuse the same one in several places instead of rebuilding it. This also keeps
the way data is presented consistent across your team.

**Dashboards** combine several visualizations into a single view, together
with filters that apply to the whole page. A dashboard shows the most important
information about a topic in one place, such as a service, a group of hosts or
a business process. This makes dashboards the usual tool for recurring
monitoring and for sharing a common view within a team.

**Canvas** is for building workpads, which are free-form pages that combine
live data, charts, text, images and other visual elements. Dashboards follow a
grid structure, but Canvas lets you control the layout and appearance in
detail. This makes it a better fit for presentation-oriented output, such as a
status screen or a report.

**Maps** displays data that has a geographic component on interactive maps.
You can place events, assets or metrics by location and analyse how they are
distributed across regions, sites or specific areas.

**Graph** helps you explore relationships between entities in your data. It
shows how values in different fields or records are connected. This is useful
when you're looking for links that aren't visible in a list of events, for
example between users, hosts and addresses involved in the same activity.

**Machine Learning** detects anomalies and unusual patterns in your data. It
highlights behaviour that differs from the norm, so you can notice issues or
unexpected trends that a fixed threshold wouldn't catch.

.. _elk-observability:

Observability
`````````````

Observability is meant for monitoring systems, applications and services. It brings
together logs, metrics and traces from applications and infrastructure, so you
can look at the health, performance and availability of a service in one
place. You can also correlate information from different sources. This makes
it easier to find errors, slowdowns and anomalies, and to work out their cause.

**Overview** summarises the whole observability environment. It shows the
health, performance and status of your systems and applications at a high
level. Use it to see quickly whether anything needs attention before you go
into a specific area.

**Logs** gives you central access to application and infrastructure logs. You
can search, filter and inspect log entries to find errors and see what happened
around a specific event. This is often where troubleshooting starts once you
know roughly when and where a problem happened.

**Infrastructure** is about hosts, containers and other infrastructure
resources. It shows metrics such as CPU, memory, disk and network usage. These
help you find bottlenecks and unusual system behaviour, and decide whether a
problem comes from the underlying resources.

**Applications** covers application performance monitoring. It shows response
times, failures and throughput for your services, and how the services depend
on each other. When an application is slow or returns errors, this view helps
you find the service where the problem starts.

**User Experience** looks at how real users interact with your applications.
It covers aspects such as page load times and frontend responsiveness. Use it
to find issues that affect end users, even when the backend looks healthy.

**Synthetics** checks the availability and behaviour of services with
simulated tests. It checks whether websites, APIs or endpoints are reachable
and respond as expected from the user's point of view. Because the tests run
independently of real traffic, you can detect a problem even when no user is
affected yet.

**Alerts** lists the alerts triggered by the rules defined in your environment.
You can follow the warnings and incidents these rules generate and respond to
them promptly.

**Cases** is for organising and tracking incidents and operational issues. In
a case, you document the problem, add comments, assign actions and follow the
resolution step by step. This is useful when several people work on the same
issue.

**SLOs** (Service Level Objectives) track whether your services meet their
reliability or performance targets, such as availability or response time
goals. Alerts and Cases are about individual incidents. SLOs show how a service
performs against its targets over time.

.. _elk-roles-and-permissions:

Roles and Permissions
`````````````````````

The ELK contract defines three roles: **VIEWER**, **EDITOR** and **ADMIN**.
Your role determines which operations you can perform in Kibana. You can
perform only the operations your role allows.

Roles are assigned to users or groups, depending on the authentication and
profiling method configured in NetEye. Each feature chapter lists the role it
requires in its *Roles and permissions* section.

.. note::

   If a feature or action described in this guide isn't available to you, ask
   your NetEye.Cloud administrator to check your role.
