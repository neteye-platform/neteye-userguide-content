.. _tornado-interaction-with-icinga2:

Tornado Interaction with :phone:`Icinga 2`
++++++++++++++++++++++++++++++++++++++++++

The interaction between Tornado and Icinga 2 is explained in details
in sections :ref:`tornado-icinga-executor` and
:ref:`tornado-smartmon-check-executor`.  In particular the Smart
Monitoring Executor interacts with Icinga 2 to create objects and set
their statuses. To ensure that the status of the Icinga 2 objects does
not get lost, NetEye provides an automatism that stops the execution
of Smart Monitoring Actions during any Icinga 2 restart or Icinga
Director deployment.

The automatism keeps track of all Icinga 2 restarts (we consider also
Icinga Director deployments as Icinga 2 restarts) in the
``icinga2_restarts`` table of the ``director`` database.  As soon as
an Icinga 2 restart takes place, a new entry with **PENDING** status
is added in that table and at the same time the Tornado Smart
Monitoring Executor is deactivated via :ref:`API
<tornado-runtime-smart-monitoring-status>`.

The ``icinga-director.service`` unit monitors the status of the Icinga
2 restarts that are in **PENDING** status and sets them to
**FINISHED** as soon as the service recognizes that Icinga 2 completed
the restart, then the Tornado Smart Monitoring Executor is activated.
In case of Icinga2 errors, see the troubleshooting page ::ref::`icinga2-not-starting`.
