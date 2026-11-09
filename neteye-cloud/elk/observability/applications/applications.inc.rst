The **Applications** section is where you monitor the performance of your
instrumented services with APM (application performance monitoring). Each
one of your services reports its transactions, the operations it performs for its
callers, and the spans inside them, such as database queries or calls to
other services. From this data |nec| derives the three indicators you'll
meet on almost every page of this section: **latency** (how long a service
takes to respond), **throughput** (how many transactions it handles per
minute) and **failed transaction rate** (the share of transactions that
end in an error).

The section is organized the way an investigation usually goes:

* **Services** lists all monitored services with their indicators. This is
  where you notice that something is wrong and pick the service to look
  at.
* The **Service overview** is the page of a single service. It brings
  together its transactions, errors, dependencies, metrics and logs, so
  most of the troubleshooting happens here.
* **Traces** lists the transactions collected across all services, ranked
  by how much they affect your users, and lets you follow a single request
  end to end.
* **Dependencies** turns the view around and shows the databases, APIs
  and other external systems your services call, so you can tell whether
  a slowdown comes from your code, or from something it depends on.
* **Service Map** draws all services and their connections, which helps
  you understand the architecture and see which services an issue could
  spread to.

All pages share the global time range and the environment selector, so
when you move from one to another you will continue viewing the same data.

.. rubric:: Roles and Permissions

The Applications views are read-only for all roles defined by the contract
with your organization:

* **VIEWER**: Read
* **EDITOR**: Read
* **ADMIN**: Read
