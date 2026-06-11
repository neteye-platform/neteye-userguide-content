.. _tornado-architecture:

Overview
~~~~~~~~

Tornado is a high performance, scalable application, and is intended
to handle millions of events each second on standard server
hardware. Its overall architecture is depicted in
:ref:`figure-tornado-architecture`.

.. _figure-tornado-architecture:

.. figure:: img/architecture.png
   :alt: Tornado architecture

   Tornado architecture


.. _tuning-infrastructure:

Tuning your infrastructure
``````````````````````````

As a part of the NetEye Core, Tornado module does not need additional
installation. However, to start running passive monitoring of your infrastructure
with Tornado software, some basic actions are to be taken.

Since passive monitoring process does not imply agent installation on
the systems and devices to be monitored, the latter should be configured
to send a particular type of data to NetEye. Tuning your own system to send
events should be done taking into account its properties, architecture and
setup.


Tornado Architecture
````````````````````

Tornado is structured as a library, and there are three main components
of its architecture:

* The *Tornado Collector(s)*, or just *Collector(s)*
* The *Tornado Engine*, or *Engine*
* The *Tornado Executor(s)*, or Executor(s)*

The term *Tornado* refers to the whole project or to a deployed system
that includes all three components.

Architecturally, Tornado is organized as a processing pipeline, where
input events move from Collectors to the Engine, to Executors, without
branching or returning.

When the system receives an *External Event*, it first arrives at a
*Collector* where it is converted into a *Tornado Event*. Then it is
forwarded to the *Tornado Engine* where it is matched against
user-defined, composable *Rules*. Finally, generated *Actions* are
dispatched to the *Executors*.


.. _tornado-collectors-overview:

Collectors
++++++++++

The purpose of a *Collector* is to receive and convert external events
into the internal Tornado Event structure, and forward them to the
Tornado Engine.

*Collectors* are *Datasource*-specific. For each datasource, there must
be at least one Collector that knows how to manipulate the datasource's
Events and generate Tornado Events.

Out of the box, Tornado provides a number of Collectors for handling
inputs from snmptrapd, rsyslog, JSON from Nats channels and generic
Webhooks. All the collectors are pre-configured on NetEye, however, there are
some that may eventually be configured manually in order to fit your purposes,
e.g. Tornado Webhook or Icinga 2 Collectors.

.. seealso:: More details on Tornado Collectors and how they can be configured
   can be found in :ref:`tornado-collect-events`.

Tornado Collectors run on the NetEye Master and on Satellites if there are
any. A Satellite converts an event into JSON and sends
it to the Master with :ref:`tornado-nats-json-collector-exec`.


Engine
++++++

The *Engine* is the second step of the pipeline. It receives and
processes the events produced by the *Collectors*. The outcome of this
step is fully defined by a Processing Tree composed of *Filters*, *Iterators*
and *Rulesets*.

A *Filter* is a processing node that defines an access condition on the
children nodes.

An *Iterator* can iterate over a field in the event, dispatching multiple
events to be handled by it's child nodes.

A *Ruleset* is a node that contains an ordered set of *Rules*, where
each *Rule* determines:

* The conditions a *Tornado Event* has to respect to match it
* The actions to be executed in case of a match

The Processing Tree is parsed at startup from a configuration folder
where the node definitions are stored in JSON format.

When an event matches one or more *Rules*, the Engine produces a set of
*Actions* and forwards them to one or more *Executors*.

Executors
+++++++++

The *Executors* are the last element in the Tornado pipeline. They
receive the *Actions* produced from the *Engine* and trigger the
associated executable instructions.

An *Action* can be any command, process or operation. For example it can
include:

* Forwarding the events to a monitoring system
* Logging events locally (e.g., as processed, discarded or matched) or
  remotely
* Archiving events using software such as the Elastic Stack
* Invoking a custom shell script

A single *Executor* usually takes care of a single *Action* type.

More information and example code snippets can be found in Section
:ref:`tornado-executors-conf`.

.. _tornado-multitenancy:

Multitenancy
~~~~~~~~~~~~

Tornado can be run on both single-tenant and multi-tenant environments.
In case of a multi-tenant environment, each tenant has only got access
to the part of a Processing Tree (starting from a filter) that contains
filtered events received from this particular tenant. This way all the event
streams are processed independently and securely.

Roles Configuration
```````````````````

If your NetEye installation is tenant aware, roles associated to each user must be
configured to limit their access only to the Tornado configuration they are allowed to work with.

In the NetEye roles (:menuselection:`Configuration / Access Control / Roles`), add or edit the role
related to the tenant limited users. In the detail of the role configuration you can
find the `tornado` module section. You can set the tenant ID in the
``tornado/tenant_id`` restriction.

.. hint:: You can **find the list of available Tenant IDs** by reading the directory
   names in :file:`/etc/neteye-satellites.d/`. You can use this command:

   .. code:: bash

      neteye# basename -a $(ls -d  /etc/neteye-satellite.d/*/)


.. _tenant-based-tornado-configuration:

Tenant-based Configuration
``````````````````````````

Tornado configuration is tenant-specifc.
For single-tenant environments the configuration you will create
will apply to the Master Tenant.

In case you would like to go with a multi-tenant environment, i.e. sending
data to the NetEye from multiple tenants, you should create a tenant with
:command:`neteye tenant config create` if this has not been done yet.
Consult :ref:`The NetEye Command <neteye-tenant-config-create>` for more information on how to
create a new tenant.

Please note that due to Implicit Lock mode only one user at a time from the pool of users
belonging to **all** tenants within your |ne| installation can modify the configuration.



Tornado Collectors are preconfigured out of the box. However, for some of them
additional steps are required in order to let a Collector receive events
sent by your system or device, e.g. in case of sending SMS events to the Tornado SMS Collector
the Modem and the ``smstools`` are to be additionally configured.

For more details on each particular type of Collectors please see :ref:`tornado-collect-events`.
