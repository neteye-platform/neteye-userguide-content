.. _slm-event-adjustment-gui:

The Event Adjustment Web Interface
++++++++++++++++++++++++++++++++++

Whether event adjustments will be considered or not during report
generation can be set with the flag “Consider Event Adjustments” on the
associated SLA Contract. This will be taken as the default behavior for
any reports produced for that contract. The value selected for this flag
may be overridden in the Reporting module by users who are granted the
permissions described below.

..  <!-- Add a link to the Reporting doc when available -->

.. _slm-event-adjustment-gui-permissions:

.. rubric:: Viewing Permissions


By default, only users with the **admins** role can access the SLM
module. Non-admin users who need to view an SLM report will need
special permissions to access or modify this flag or the other items
(i.e., Contracts, customers, and SLA Types) in the SLM Module. To
grant these permissions to users, you need to create a role (go under
:menuselection:`Configuration / Authentication / Roles`) with a
suitable permission (like e.g., **report-adjustment-override** or
**admin**) over the SLM module. Enabling only **General Module Access**
permission will give only *View Access* to slm Event Adjustment.


To set these permission for a non-admin user requires first to enable
the *General Module Access* permission for the SLM module.

.. <!-- When the Reporting task is done, change "relevant flag" to the
   actual name -->

.. _slm-create-event-adjustment:

Creating an Event Adjustment
++++++++++++++++++++++++++++++++

To add a new event adjustment, go to :menuselection:`SLM / Event Adjustments` and enter
values for the following options:

 * **Object Type:** Type of the monitoring object, can be host or service.
 * **Host Name:** Name of the host to which to attach the event.
 * **Service Description:** Name of the service, running on host passed in the *Host Name* field,
   to which to attach the event
 * **Description:** Title for the event
 * **Start:** Timestamp for the starting point of the event (YYYY-MM-DD hh:mm:ss)
 * **End:** Timestamp for the ending point of the event (YYYY-MM-DD hh:mm:ss)
 * **Event Type:** State for the event; The event type must be one of the values in :numref:`table-event-types` that are
   available for the monitored object passed in the *Host Name* or *Service Description* field.

Alternatively, Event Adjustments can also be created starting from any
existing Icinga 2 Event in the Monitoring, from the Event Details page.

.. rubric:: Advance Search Filter

It is possible for a user to search event adjustments, according to
their requirements using the search filter that supports search on the
basis of hostname, service, description, start/end time or event type.
