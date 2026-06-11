Introduction
------------

|ne| is a flexible stack of monitoring technologies and can easily be
adjusted to serve its purpose under conditions granted by the hosting
infrastructure.

|ne| is a scalable and extensible software. Indeed, on the one side,
|ne| can be run on a *Single Node* or *Cluster* (*multiple nodes*)
architecture, while on the other side you can add Tenants, Satellites
or even agents to gather data and send them to the Master for
processing, offloading tasks to other devices.

Based on the customer's monitoring and analytical needs, services can
be executed in complex environments across multiple locations,
e.g. tracking the state of routers in all the premises and offices of
your organization.

The set of services provided by |ne| may vary depending on whether
you choose to utilize the core functionality modules that are originally
shipped with |ne|, or install *additional components* in order to
perform even more specific tasks, e.g. measure visual end-user
experience with the help of the integrated Alyvix software.

Single Node and Cluster Concept
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Another decisive point while tuning the |ne| architecture to meet your
business requirements is the complexity of the processes to be
executed.

All the provided services may be scaled by means of adopting an
appropriate |ne| setup type. To choose the most appropriate setup type
you will need to estimate the expected level of the monitoring system
and the amount of resources you'd like to involve in the monitoring
process.

In case your target infrastructure is small and requires a minimum
amount of resources, running |ne| on a Single Node architecture would
be the best solution.

For a more complex environment that requires redundancy and high
availability when running monitoring processes, it is recommended that
you use a Cluster setup. Clustering allows to scale the system up to a
level where it is able to deal with an extensive amount of
resources. At the same time Clustering provides high availability and
helps avoid any downtime or service disruption whenever one of several
nodes in a Cluster goes offline, e.g. in case of a networking or
hardware/software issue.

.. _arch-multi-tenancy:

|ne| Master and Multi-tenancy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

|ne| Master instance serves either as a self-contained server
(Single Node), or a high availability Cluster composed of a
combination of nodes.

The Master allows to build individual and isolated communications from
multiple clients to a centralized server, i.e. the Master, that will then process
independently all data streams.

Communication between the client and the Master may involve *Satellite
nodes*. Each client is to be monitored independently, so a Satellite would be
assigned to each separate set of hosts to collect and communicate
their data to the Master for further processing.

Satellites can be set up irrespective of whether you are using |ne| as a Single Node
or a Cluster.

The client data may also be transmitted to the Master directly, without
involving a Satellite (or multiple Satellites where appropriate) as an actor.

Having received the data, the Master then processes it in order to
fulfill predefined tasks, e.g. make data entries to Influx DB, generate
a Tornado action based on the event, etc.

|ne| supports multi-tenancy architecture, where a single |ne| instance
allows you to serve multiple business units inside your
organization(s). Multi-tenancy concept allows you to monitor a number of
clients, whether you choose to run monitoring on a Cluster or
Single Node architecture.

Having multiple tenants you will be able to aggregate even larger
arrays of data, preserving data security: each tenant zone communicates
with the Master individually and is not visible for others.
Communication of a tenant with the Master can be protected by using multiple accounts.
