
.. _elastic-feature-module:

Overview
========

|ne| Elastic Stack Feature Module provides a full `Elastic Stack
<https://www.elastic.co/>`_ solution for fulfilling a vast range
of log management and data analysis tasks.

Elastic Stack, which comprises three components - Elasticsearch, Logstash, and Kibana - works as a powerful,
integrated solution for managing large volumes of data, offering real-time insights
and a comprehensive analytics suite.

.. rubric:: Module Installation

|ne| Elastic Stack is an additional Feature Module.
It can be installed following the procedure described in :ref:`Additional NetEye Components <neteye-components>`.

The module is available for installation only if the additional component is installed
in the NetEye system.

.. rubric:: Elastic Stack Subscription

The scope of |ne| Elastic Stack functionality set is based on the `Elastic Stack subscription levels <https://www.elastic.co/subscriptions>`__ -
**Platinum** or **Enterprise**. You can manage your subscription by running
a :ref:`dedicated command <neteye-feature-module-neteye-elastic-stack-subscription-set>`.

With the Elastic Stack module installed you can also spin up your Elastic monitoring e
experience by setting up additional solutions:


.. rubric:: SIEM

SIEM solution is available for a setup under the Elastic Stack module.
SIEM is based on the Elastic stack and is intended to
provide various means to manage--collect, process, and sign--log files
produced by NetEye and by the various services running on it.

:abbr:`SIEM (Security Information and Event Management)` in computer
security refers to a set of practices whose purpose is to collect log
files from different hosts and services, usually running on the
internal network infrastructure of a company or enterprise, and
process them for disparate purposes including security analysis, data
compliance, log auditing, reporting, alerting, performance analysis,
and much more.

.. rubric:: Elastic APM

Elastic APM is an application performance monitoring system built on the Elastic Stack.
It allows you to monitor software services and applications in real time, by collecting
detailed performance information on response time for incoming requests, database queries,
calls to caches, external HTTP requests, and more. This makes it easy to pinpoint
and fix performance problems quickly.

.. rubric:: Elastic RUM

Real User Monitoring captures user interaction with clients such as web browsers.
The JavaScript Agent is Elastic's RUM Agent.
Unlike Elastic APM backend agents which monitor requests and responses,
the RUM JavaScript agent monitors the real user experience and interaction
within your client-side application. The RUM JavaScript agent is also framework-agnostic,
which means it can be used with any front-end JavaScript application.
