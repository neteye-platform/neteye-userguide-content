
The :term:`Event Adjustment` feature allows :term:`Service Owners <Service Owner>`
and :term:`Service Level Managers <Service Level Manager>`
to retroactively add events. Indeed, an *Event Adjustment* is the
action of adding to a monitored host or service an event over a given
period of time, that actually did not take place. This proves useful
in situations where, for instance, the Operational team has forgotten
to schedule :term:`Downtime` in advance:
this oversight would add to a wrong calculation of the availability,
therefore adding an Event Adjustment that covers the unscheduled
downtime would fix the problem. It is worth noting that Event
Adjustments will affect the availability calculations for hosts and
services, and potentially affect whether the service provider is or is
not satisfying the :term:`target SLA <Target Availability>`
specified in the customer contract.

How It Works
++++++++++++

The Event Adjustment feature is an extension of the SLM module, and can
only be performed by a NetEye user with a certain privilege level. All
inserted event adjustments are stored in a dedicated database table in
order to ensure that the data (i.e., the existing timeline) cannot be
manipulated. Event adjustments are taken into account during
:term:`Availability` calculations without any additional intervention.

Each adjustment must be applied to a monitored object. The following
table indicates which types of event adjustments can be applied to each
type of monitored object.

.. _table-event-types:

.. table:: Available event types for host and services

   +-----------------------+--------------------------------------------+
   | Monitored Object Type | Event Type                                 |
   +=======================+============================================+
   | *host*                | *up, down, downtime*                       |
   +-----------------------+--------------------------------------------+
   | *service*             | *ok, warning, critical, unknown, downtime* |
   +-----------------------+--------------------------------------------+

Multiple event adjustments on the same host or service cannot overlap,
except if the event type of one of the adjustments is *downtime*. If a
new or modified event does overlap the time bounds of an existing
adjustment, NetEye will report an error and the new event adjustment
will not be processed.
