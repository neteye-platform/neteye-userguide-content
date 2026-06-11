.. warning::
   The availability calculation described in this section assumes the "Icinga DB Historical Data"
   feature flag is enabled. If you are using the legacy Icinga 2 IDO backend for historical data,
   please refer to the `User Guide version 4.46 <https://neteye.guide/4.46/feature-modules/slm/advanced-topics.html#availability-calculation>`_.

The availability calculation process is a complex multi-step algorithm
that involves two major phases. The first phase consists of identifying
the time periods over which the availability calculations must be
performed. The second phase calculates the availability for each of
those periods identified in the previous phase.

Phase One - Identify the Availability Periods
`````````````````````````````````````````````

In order to explain the details of the availability calculations, we
will use here a simplified request model that differs from the
implementations in the following respects:

* Represents dates in a human readable format instead of in epoch
  milliseconds
* Ignores the timezone
* Uses a simplified *event* format

Consider the following *pseudo request*.

.. note:: The format of this request is not valid for the SLM module
   as the real dates should be in Unix time format. We also explicitly
   omit the array of events for the moment.

.. code:: json

   {
       "output_format": "json"
       "time_period": {
           "ranges": {
               "monday": "08:00-18:00",
               "tuesday": "08:00-12:00"
           }
       },
       "time_zone": "Europe/Rome",
       "calculation_period": {
           "type": "weekly",
           "start": "monday"
       },
       "time_range": {
           "from": "Friday 1st January 2010 at 00:00 am",
           "to": "Monday 25th January 2010 at 00:00 am"
       },
       "initial_state": "0",
       "consider_downtime" : false,
       "consider_event_adjustments" : false,
       "events": [],
       "last_hard_states": [],
       "downtimes": [],
       "event_adjustments": [],
       "target_availability": 99.5,
       "expected_monitored_objects": [],
       "locale": "en_US"
   }

In the following section we will focus principally on understanding how
the calculation periods are determined. To identify them, three elements
from the request are used:

1) The Time Frame in *time_range*
2) The *calculation_period*
3) The *ranges* from the *time_period*

In our example request above, the *time_range* is:

* From: Friday, January 1st 2010 at 00:00am
* To: Monday, January 25th 2010 at 00:00 am

Consequently, the events taken into account are all the events that occur
within the *time_range*. In order to determine the availability during the
initial period of the time range, SLM reports use the information that is
stored in the **previous_hard_state** property of the first event occurring
after *time_range.from* (see :ref:`slmd-icingadb-retention-interaction` for details).

Once we have identified the *time_range*, we use the
*calculation_period* to split the range into the desired calculation
blocks. The final availability will be calculated for each one of these
blocks independently.

The *calculation_period* of our request is set to **weekly**/**monday**.
This means that we want availability aggregated by groups of seven days
starting on Mondays. As noted above, our *time_range* spans from January
1st to January 25th, for a total of 25 days. If we split it into blocks
of one week, we will obtain 5 availability periods:

#. From Friday, January 1st to Sunday, January 3rd
#. From Monday, January 4th to Sunday, January 10th
#. From Monday, January 11th to Sunday, January 17th
#. From Monday, January 18th to Sunday, January 24th
#. Monday, January 25th

Since only full calculation periods are taken into account, the first
and last blocks will be discarded since they last less than a full week.
Consequently, the final calculation will include only three periods for
a total of 21 days.

The last step is to exclude from each of the availability periods the
times that do not match the requested *time_period* *ranges*:

#. Monday from 08:00 to 18:00
#. Tuesday from 08:00 to 12:00

If we apply this to the three availability periods in the previous step,
we will arrive at the definitive calculation blocks:

* The first availability period will include the aggregated statistics
  of:

  * Monday, January 4th from 08:00 to 18:00
  * Tuesday, January 5th from 08:00 to 12:00

* The second will include the aggregated statistics of:

  * Monday, January 11th from 08:00 to 18:00
  * Tuesday, January 12th from 08:00 to 12:00

* The third and final one will include the aggregated statistics of:

  * Monday, January 18th from 08:00 to 18:00
  * Tuesday, January 19th from 08:00 to 12:00

Phase Two - Calculate the Availabilities for each period
````````````````````````````````````````````````````````

In the preceding section, we determined the required availability
periods. Now, let’s create a sequence of monitoring events and see how
they impact the availability calculations.

Like we did in the previous section, for the purpose of explaining this
phase, we will make some assumptions:

* All the events originate from the same host.
* The calculations are performed in hours instead of milliseconds.
* The downtimes are ignored (see section
  :ref:`slmd-how-downtime-works` for downtime calculation).

Here is our example sequence of events:

#. Thursday January, 7th at 00:00 -> hard_state DOWN
#. Monday, January 11th at 10:00 -> hard_state UP
#. Monday, January 11th at 20:00 -> hard_state DOWN
#. Tuesday, January 12th at 10:00 -> hard_state UP
#. Tuesday, January 12th at 11:00 -> hard_state DOWN
#. Wednesday, January 13th at 00:00 -> hard_state UP

First, let’s calculate the availability for the **first period**, which
includes:

* Monday, January 4th from 08:00 to 18:00
* Tuesday, January 5th from 08:00 to 12:00

Since the first event is after Monday the 4th at 8:00AM, we do not know
the state of the host at the beginning of the period. Consequently, we
will use the *initial_state* field, which represents the state of the
host at beginning of the report, if it cannot be determined by analyzing the
events in the request.

In our case this value is **0** (i.e., **UP**) and, as the
*initial_state* is always of type *hard_state*, from the beginning of the
period, the host is in state *hard_up*. The *initial_state* always
defaults to **0** in the reports generated via NetEye GUI.

The state of the host never changes during the first period, because
there are no events to alter it. Thus we have:

* 10 hours of state hard_up* on Monday, January 4th from 08:00 to
  18:00
* 4 hours of state hard_up* on Tuesday, January 5th from 08:00 to
  12:00

In summary, the availability results for the **first period** are:

* total time: 12 hours
* hard_up time: 12 hours (100% of the total time)

The **second availability period** includes:

* Monday, January 11th from 08:00 to 18:00
* Tuesday, January 12th from 08:00 to 12:00

The only event preceding the period takes place on Thursday, January
7th, at 00:00 sets the host state to *hard_down*. So on Monday, January
11th at 08:00 the state is *hard_down*.

At 10:00 on Monday, January 11th we receive a *hard_up* event, and now
the host state becomes *hard_up*.

Subsequently, we have two events on Tuesday, January 12th, the first of
which sets the state to *hard_up* at 10:00, and the second of which sets
the state to *hard_down* one hour later.

So, we have:

* 2 hours of state *hard_down* on Monday, January 11th from 08:00 to
  10:00
* 8 hours of state *hard_up* on Monday, January 11th from 10:00 to
  18:00
* 2 hours of state *hard_up* on Tuesday, 12th January from 08:00 to
  10:00
* 1 hours of state *hard_down* on Tuesday, 12th January from 10:00 to
  11:00
* 1 hours of state *hard_up* on Tuesday, 12th January from 11:00 to
  12:00

In summary, the availability results for the **second period** are:

* Total time: 12 hours
* *hard_up* time: 9 hours (75% of the total time)
* *hard_down* time: 3 hours (25% of the total time)

The **third availability period** includes:

* Monday, January 18th from 08:00 to 18:00
* Tuesday, January 19th from 08:00 to 12:00

During this time there are no events, so the state for the entire period
is that of the last event received before the time period. Since this is
the one received on Wednesday, January 13th at 00:00 that sets the state
to *hard_up*, the availability result for the **third period** are:

* Total time: 12 hours.
* *hard_up* time: 12 hours (100% of the total time).

.. warning:: The Icinga DB SLA retention policy settings may affect SLM availability
  reports. Please see :ref:`Interaction with Icinga DB SLA retention policy <slmd-icingadb-retention-interaction>`
  for more details.

.. _slmd-how-downtime-works:

How the Downtime Calculations Work
++++++++++++++++++++++++++++++++++

When calculating availability, the SLM module can optionally take into
account planned downtimes for hosts and services. You can enable this
behavior by using the *consider_downtime* key in the calculation
request, and setting it to **true**, then adding a *downtimes* array. We
will refer to the following example request:

.. code:: json

   {
      "output_format": "json",
      "time_period": "{ ... }",
      "time_zone": "Europe/Rome",
      "calculation_period": "{ ... }",
      "time_range": {
           "from": "1555000000000",
           "to": "1561000000000"
       },
      "initial_state": "0",
      "consider_downtime": true,
      "consider_event_adjustments": false,
      "events": [
          {
              "host_name": "host1.example.com",
              "service_description": "disk_agent",
              "timestamp": "1560000000000",
              "state": "0",
              "type": "hard_state"
          },
          {
              "host_name": "host1.example.com",
              "service_description": "disk_agent",
              "timestamp": "1560000500000",
              "state": "-1",
              "type": "dt_end"
          }
      ],
      "last_hard_states": [
          {
              "host_name": "host1.example.com",
              "service_description": "disk_agent",
              "timestamp": "1560000000000",
              "state": "0"
          }
      ],
      "downtimes":[
          {
               "depth": 2,
               "host_name": "host1.example.com",
               "service_description": "disk_agent"
          }
      ],
      "event_adjustments": [],
      "target_availability": 99.5,
      "expected_monitored_objects": [
           {
               "host_name": "host1.example.com",
               "service_description": "disk_agent"
           }
      ],
      "locale": "en_US",
      "average_availability": {
           "show_calendar": true
      }
   }

Due to the fact that the *consider_downtime* property above is **true**,
the availability calculation will take into account any listed
downtimes.

During the calculation, important data is provided in the *downtimes*
field of the request. This data is a list of the downtime status at the
beginning of the *time_range* for each monitored host or service.

As downtimes can be nested, the *depth* value is used to represent their
nesting level (check the table below and the underneath paragraphs for
an example of nested downtimes). A *depth* value of zero implies that
there was **no downtime** during the interval in question.

For example, in the example above, we know that:

* At the epoch instant 1555000000000 (which corresponds to
  *time_range.from*) the service *disk_agent* on host
  ‘host1.example.com’ was in planned downtime.
* In addition, we know that the downtime depth was 2.

If a service or host is not included in the *downtimes* list, it will by
default be considered as not in downtime.

Consider this hypothetical ordered sequence of events for a given host
to clarify how the states and downtime depth are calculated:

.. csv-table:: Downtime Event Sequence Example
   :header: "Time Instant", "Received Event State Type", "Received
       Event State", "Calculated Availability State", "Downtime
       Depth"

   1000, hard_state, 0, operative.hard_up, 0
   2000, dt_start, -1, in_downtime.hard_up, 1
   3000, dt_start, -1, in_downtime.hard_up, 2
   4000, hard_state, 2, in_downtime.hard_down, 2
   5000, dt_end, -1, in_downtime.hard_down, 1
   6000, hard_state, 0,	in_downtime.hard_up, 1
   7000, dt_end, -1, operative.hard_up, 0

The explanation for the data sequence above is:

* From time 1000 to 2000: The downtime depth is zero, so the host is
  not in downtime and we have 1000ms of availability state *hard_up*
  in the *operative* mode.
* From 2000 to 3000: A downtime period is starting, so the depth is
  increased by 1 and the host is now in planned downtime. So we have
  1000ms of availability state *hard_up* but in the *in_downtime*
  mode.
* From 3000 to 4000: Another downtime period is starting, so the depth
  is now increased to 2 and the host is still in downtime. We have an
  additional 1000ms of availability state *hard_up* in the
  *in_downtime* mode.
* From 4000 to 5000: We received a *hard_state* 2. The downtime depth
  does not change and the host is still in planned downtime. We have
  now 1000ms of availability state in the *in_downtime* mode, but the
  calculated state is now *hard_down*.
* From 5000 to 6000: We received dt_end*, so the downtime depth is
  decreased by 1 but the host is still in planned downtime. We have
  an additional 1000ms of availability state *hard_down* in the
  *in_downtime* mode.
* From 6000 to 7000: We received *hard_state* 0 and the host is still
  in planned downtime. We have 1000ms of availability state *hard_up*
  in the *in_downtime* mode.
* After 7000: We received *dt_end*, so the downtime depth is decreased
  by 1 and the host is no longer in downtime because the depth value
  is now zero.  From this moment on, the calculated status will be
  state *hard_up* in the *operative* mode.

When the *consider_downtime* property is instead **false**, then:

#. The availability states are always considered as *operative*.
   Consequently, no time will be accounted as *in_downtime*.

#. Any received *dt_start* and *dt_end* events will be reported in the
   *events.skipped* section of the response. For instance, the above
   event sequence example would have generated this *skipped* report:

.. code:: json

   {
       "events": {
           "skipped": {
             "dt_start": 2,
             "dt_end": 2
           },
           "unknown": 0
       }
   }

How the Event Adjustments Calculations Work
+++++++++++++++++++++++++++++++++++++++++++

When calculating availability, the SLM module can optionally take into
account **event adjustments** for hosts and services. You can enable
this behavior by setting the *consider_event_adjustments* flag in the
calculation request to **true**, and then adding an *event_adjustments*
array at the top level of the input as in this example:

.. code:: json

   {
      "output_format": "json",
      "time_period": "{ ... }",
      "time_zone": "Europe/Rome",
      "calculation_period": "{ ... }",
      "time_range": {
           "from": "1555000000000",
           "to": "1561000000000"
       },
      "initial_state": "0",
      "consider_downtime": false,
      "consider_event_adjustments": true,
      "events": [
          { }
      ],
      "downtimes":[],
      "event_adjustments": [
         {
           "host_name": "test-logmanager",
           "service_description": null,
           "start": "1559661092000",
           "end": "1559661093000",
           "event_type": "up"
         },
         {
           "host_name": "docker-jenkins-node-01",
           "service_description": null,
           "start": "1559661092000",
           "end": "1559661096000",
           "event_type": "down"
         }
      ],
      "target_availability": 99.5,
      "expected_monitored_objects": [],
      "locale": "en_US",
      "average_availability": {
           "show_calendar": true
      }
   }

Because the *consider_event_adjustments* flag in the above example is
set to **true**, the availability calculation will take into account any
adjustments listed in the *event_adjustment* field of the request.

An event adjustment sets the status of a monitoring object to a specific
value over a given time range, regardless of the events that actually
happened within that time range.

After the event adjustment *end* instant, the status of the calculation
will be the one of the last event received before that instant.

For instance, in the example above, the status of the host
*test-logmanager* will be changed to **hard ok** between the instants
1559661092000 and 1559661093000 if the *events* array were to contain
entries that indicating a different status.

Event adjustments cannot overlap or be nested.


How the timeframe availability calculations work
++++++++++++++++++++++++++++++++++++++++++++++++

In addition to the availability of each calculation period, the SLM Module
provides also the global timeframe availability of an object calculated as the
weighted mean of the availabilities of the single calculation periods.

For example, if we have these availabilities for a host in a timeframe
from the 1st to the 15th of January:

.. csv-table:: Availabilities in a report with daily calculation period
   :header: "Availability period", "Availability percentage"

   "from January 1st to 3rd", 100%
   "from January 7th to 8th", 40%
   "January 11th", 100%

Then the host timeframe availability will be calculated as:
((100% * 3) + (40% * 2) + (100% * 1)) / 6 = 80%

.. note:: The timeframe availability calculation takes into account only the complete
   calculation periods.
   For example, performing a monthly report from the 10th of April to
   the 15th of July will generate a timeframe availability that involves
   only the months of May and June.


.. _slmd-icingadb-retention-interaction:

Interaction with Icinga DB SLA retention policy
++++++++++++++++++++++++++++++++++++++++++++++++

The Icinga DB SLA retention policy settings may affect the SLM availability
Reports. This happens because the availability calculations of the SLM module relies on
the state change events present in the Icinga DB SLA tables ``sla_history_state`` and ``sla_history_downtime``
and the retention policy may delete older events which are still relevant for determining
the availability of a Host or Service.

Moreover, there is the possibility that the outcome of an SLM Report will differ depending
on the time it was generated, since the Icinga DB retention policy continuously
removes events older than the defined age in days. For this reason, please configure the
days in the Icinga DB SLA retention policy, with a value sufficient to avoid impacting
any of your SLM Reports as described in the :ref:`icingadb-retention-and-slm-reports` section.

Additionally, in case some events are missing from the history, remember that you can
always create proper :ref:`slm-event-adjustment` to fix the problem.

To overcome some issues deriving from the deletion of old historical events, the SLM
availability calculation uses two additional pieces of information present in Icinga DB,
which we explain below:

#. The **previous_hard_state** property of the first event occurring after the start of the
   Report Time Frame

#. The **last_hard_state** Runtime Attribute of Hosts and Services

.. rubric:: The previous_hard_state property

Each event present in the Icinga DB SLA database has a **previous_hard_state** property, which
represents the hard state of the monitored object before the event timestamp.

This information is used by the SLM module to determine the initial state of a monitored object at the beginning
of the Report Time Frame: for each monitored object, SLM looks for the first event that occurs after the start
of the Report Time Frame and uses the value of the **previous_hard_state** property of that event to
construct an artificial event having ``timestamp`` set to **0** and ``state`` set to the value of the
**previous_hard_state** property.

This event is added to the ``events`` array of the availability calculation, and allows SLM to
determine the actual initial state of the monitored object at the beginning of the Report Time Frame.

.. rubric:: The last_hard_state Runtime Attribute

SLM availability calculation takes, as optional parameter, an array of ``last_hard_states``
taken from Icinga 2's attributes (see Tables `Runtime Attributes` of Hosts and Services in
`Icinga2 documentation
<https://icinga.com/docs/icinga-2/latest/doc/09-object-types/>`_ for details) of
hosts and services. This information can integrate and complete the information given by
the *events* array and represents which was the last hard state of a Monitored
Object. This parameter is useful when no state change events of a monitored object are available,
but information about the last hard state is still stored in the
Monitored Object status.

The following are the hard states considered by the SLM module:

* for hosts: ``UP`` and ``DOWN``
* for services: ``OK``, ``WARNING``, ``CRITICAL``, and ``UNKNOWN``


The *last_hard_state* of a monitored object is only taken into account
when no other events of type **hard state** are passed for that monitored object.

Consider as an example the following request:

.. code:: json

   {
      "output_format": "json",
      "time_period": "{ ... }",
      "time_zone": "Europe/Rome",
      "calculation_period": "{ ... }",
      "time_range": {
           "from": "1555000000000",
           "to": "1561000000000"
       },
      "initial_state": "0",
      "consider_downtime": false,
      "consider_event_adjustments": true,
      "events": [
          {
              "host_name": "host1.example.com",
              "service_description": null,
              "timestamp": "0",
              "state": "2",
              "type": "hard_state"
          },
          {
              "host_name": "host1.example.com",
              "service_description": null,
              "timestamp": "1560000000000",
              "state": "0",
              "type": "hard_state"
          }
      ],
      "last_hard_states": [
          {
              "host_name": "host1.example.com",
              "service_description": null,
              "timestamp": "0",
              "state": "0"
          },
          {
              "host_name": "host2.example.com",
              "service_description": null,
              "timestamp": "0",
              "state": "0"
          }
      ],
      "downtimes":[],
      "event_adjustments": [],
      "target_availability": 99.5,
      "expected_monitored_objects": [],
      "locale": "en_US",
      "average_availability": {
           "show_calendar": true
      }
   }

The `last_hard_state` of the host *host1.example.com* will be discarded since a
**hard_state** event type for the host *host1.example.com* is already present in the
*events* array.

On the contrary, since no hard events are present for host *host2.example.com*,
the *last_hard_state* of the host *host2.example.com* will be treated as an actual
hard event and will affect the availability calculation. This last hard state will allow
to compute the availability of host *host2.example.com* even though no event for that
host is present in *events*.

Note that the timestamp of the *last_hard_state* is always set to **0**, indicating that we assume
that the state of the monitored object was the one specified in the *last_hard_state* since the beginning of time.

In this example, the status of the host *host2.example.com* will be considered as **hard up**
for the entire Report Time Frame, because the *last_hard_state* indicates state **hard up**
since timestamp `0` and no other events are present for this host.

Example Scenarios
`````````````````

To help you understand how the SLM availability computation works together with the Icinga DB retention
policy, please have a look at the following scenarios.

.. note:: For the sake of simplicity in these scenarios we will **not** take into account
   the :ref:`Operational Times and Calculation Periods <glossary>`.

.. to do: add link to monitoring object definition

All scenarios depicted below show a service that changes to an ``OK`` status, but the same
reasoning applies to any Monitored Object transitioning to any hard state mentioned in the
previous section.

.. rubric:: Scenario 1

In Scenario 1, for the Monitored Object in consideration we only have 1 state change
(**A** changes to *OK*), occurring outside the retention period. This state change is then
not available in the state history. Instead, the *last_hard_state* is always available,
hence this information is used to compute the availability of the Monitored Object
throughout the Report Time Frame.

* The reported availability is therefore: **100%**

.. figure:: /feature-modules/slm/advanced-topics/img/scenario-1.svg

   Scenario 1

.. rubric:: Scenario 2

In Scenario 2, we have one state change (**A**, *Hard Critical*) occurring before the retention period
and one state change (**B**, *OK*) occurring within the retention period.

**A** occurred before the retention period, but the *previous_hard_state* property present in
event **B** tells us that before **B** the Monitored Object was in Hard Critical state.

* The reported availability is therefore: **25%**

.. figure:: /feature-modules/slm/advanced-topics/img/scenario-2.svg

   Scenario 2

.. rubric:: Scenario 3

In Scenario 3, we have two state changes (**A**, *Hard Critical*, and **B**, *OK*, in this
order) occurring before the retention period, but within the Report Time Frame.

**A** is no longer available, so, thanks to the *last_hard_state* we only know that the
Monitored Object is currently *OK*. Since we do not have other data,
SLM assumes that the Monitored Object has always been available.

* The reported availability is therefore: **100%**

.. figure:: /feature-modules/slm/advanced-topics/img/scenario-3.svg

   Scenario 3

.. rubric:: Scenario 4

In Scenario 4, we have one state change (**A**, *Hard Critical*) occurring before the
retention period but within the Report Time Frame, and another (**B**, *OK*) occurring
within the retention period and within the Report Time Frame.

**A** is no longer available during the computation, while **B** is, because it
occurred within the retention period. Event **B** also tells us that before **B** the
Monitored Object was in Hard Critical (based on the *previous_hard_state* property of event **B**).

Since in the computation we only know that the Monitored Object was available after state
change **B**, SLM assumes that before state change **B** the Monitored Object was available.

* The reported availability is therefore: **25%**

.. figure:: /feature-modules/slm/advanced-topics/img/scenario-4.svg

   Scenario 4

.. rubric:: Scenario 5

In Scenario 5, we have state change **A** (*Hard Critical*) occurring before the retention period
and before the Report Time Frame, and Downtime **B** (which has a duration of 25% of the
Report Time Frame) occurring before the retention period but within the Report Time Frame.

All events have been deleted by the retention policy, so the *last_hard_state* of the Monitored Object is taken
into account. The last hard state (**A**) is *Hard Critical*, so, in the absence of other events, SLM considers
the Monitored Object as *Hard Critical* for the entire Report Time Frame.

* The reported availability is therefore: **0%**

.. figure:: /feature-modules/slm/advanced-topics/img/scenario-5.svg

   Scenario 5

.. rubric:: Scenario 6

In Scenario 6, the Monitored Object considered was created (**A**) after the end
of the Report Time Frame. Hence also its *last_hard_state* (**B**) occurred after the
end of the Report Time Frame.

In this case, the SLM availability calculation sees that the *previous_hard_state* of
**B** is *Pending*, so the only event taken into consideration for the Report Time Frame is the *Pending* state,
which is assumed to start at timestamp **0**.

Since a *Pending* state does not provide information about the availability of the Monitored Object, SLM
cannot say anything about the availability of the Monitored Object in the Report Time Frame.

* SLM will report: **No events associated with this monitored object**

.. figure:: /feature-modules/slm/advanced-topics/img/scenario-6.svg

   Scenario 6

Ignored events
++++++++++++++

Some types of events are ignored by the calculation algorithm. This
happens when the event belongs to one of the following categories:

* **skipped**: any occurrence of the following event types that have
  no impact on availability:

  * *notify*
  * *comment*
  * *comment_deleted*
  * *ack*
  * *ack_deleted*
  * *dt_comment*
  * *dt_comment_deleted*
  * *flapping*
  * *flapping_deleted*
  * *dt_start* (only if consider_downtime* is set to **false**)
  * *dt_end* (only if consider_downtime* is set to **false**)
  * **unknown**: Any event that is of a type not known to the Backend

An End-to-End Calculation Example
+++++++++++++++++++++++++++++++++

As mentioned at the beginning of the chapter, the
**/api/availability_calculation_full** endpoint receives *HTTP POST*
requests in JSON format. In this section we show an example of how the
availability is calculated, using the following valid request body:

.. code:: json

   {
       "output_format": "json",
       "time_period": {
           "display_name": "Time period test name",
           "object_name": "timeperiod_test",
           "ranges": {
               "monday": "08:00-18:00",
               "tuesday": "08:00-12:00,13:00-18:00"
           }
       },
       "time_zone": "Europe/Rome",
       "calculation_period": {
           "type": "weekly",
           "start": "monday"
       },
       "time_range": {
           "from": "1555000000000",
           "to": "1561000000000"
       },
       "initial_state": "0",
       "consider_downtime" : false,
       "consider_event_adjustments": false,
       "events": [
           {
               "host_name": "host1.example.com",
               "service_description": "disk_agent",
               "timestamp": "1559720464000",
               "state": "0",
               "type": "hard_state"
           },
           {
               "host_name": "host1.example.com",
               "service_description": null,
               "timestamp": "1559661092000",
               "state": "0",
               "type": "hard_state"
           }
       ],
       "downtimes": [],
       "event_adjustments": [],
       "target_availability": 99.5,
       "expected_monitored_objects": [
           {
               "host_name": "host1.example.com",
               "service_description": "disk_agent"
           },
           {
               "host_name": "host1.example.com",
               "service_description": null
           }
       ],
       "locale": "en_US",
       "average_availability": {
           "show_calendar": true
       }
   }

This request triggers a calculation that:

#. Takes into account only those states on **Mondays** between
   **08:00** and **18:00**, and on **Tuesdays** between **08:00** and
   **12:00** and between **13:00** and **18:00**.
#. Parses all the dates and times with **Europe/Rome** as the
   *time_zone*.
#. Aggregates the result by week, with the first day of the week set
   to **Monday**, according to the *calculation_period*.
#. Performs the calculations for the *time_range* going from **1555000000000**
   (corresponding to Thursday April 11th 2019 16:26:40 PM UTC) to
   **1561000000000** (Thursday June 20th, 2019 03:06:40 AM UTC), as
   Unix times expressed in milliseconds.
#. Defaults to state **0**, if a state cannot be determined, for
   instance because there are no events in the indicated time range.
#. Excludes all downtime events, including *dt_start* and *dt_end*
   from the calculations, since *consider_downtime* is **false**.
#. Excludes all event adjustments since *consider_event_adjustment* is
   **false**.
#. Processes a list of *events* that contains two events occurring at
   the **1559720464000** and **1559661092000** Unix time.

The Service Level Management module GUI automatically converts dates
into Unix times. If you are using the REST API directly, you will need
to convert the desired time into epoch milliseconds. You can get the
current time in milliseconds (or any desired time by using the “-d”
parameter) with the following commands::

   # date +%s%3N      # get the current time in milliseconds
   1561031493613
   # date -d 2019-06-20 -d 08:01:55AM +%s%3N
   1561010515000

Event types and states are defined by Icinga and included in their
reference documentation, section `Monitoring Basics for Hosts and
Services <https://icinga.com/docs/icinga2/latest/doc/03-monitoring-basics/#hosts-and-services>`_.

To test an availability request, either construct one or cut and paste
the request above into a shell as a shell variable (e.g., export
DATA=‘<paste>’), then call the REST API with curl like this::

   curl -Ss -X POST -H "Content-Type:application/json" --data "$DATA" http://slmd.neteyelocal:4949/api/availability_calculation_full

The resulting (unformatted) JSON response should be similar to the one
shown here:

.. code:: json

   {
     "monitored_objects": [
       {
         "host_name": "host1.example.com",
         "service_description": null,
         "calculation_periods": [
           {
             "from": 1555279200000,
             "to": 1555884000000,
             "states_ms": {
               "OPERATIVE": {
                 "HARD_UP": 68400000,
                 "HARD_DOWN": 0,
                 "HARD_UNREACHABLE": 0,
                 "SOFT_DOWN": 0,
                 "SOFT_UNREACHABLE": 0
               },
               "IN_DOWNTIME": {
                 "HARD_UP": 0,
                 "HARD_DOWN": 0,
                 "HARD_UNREACHABLE": 0,
                 "SOFT_DOWN": 0,
                 "SOFT_UNREACHABLE": 0
               },
               "TOTAL": 68400000
             }
           },
           { "..." : "..."  },
         ]
       },
       {
         "host_name": "host1.example.com",
         "service_description": "disk_agent",
         "calculation_periods": [
           {
             "from": 1555279200000,
             "to": 1555884000000,
             "states_ms": {
               "OPERATIVE": {
                 "HARD_OK": 68400000,
                 "HARD_WARNING": 0,
                 "HARD_CRITICAL": 0,
                 "HARD_UNKNOWN": 0,
                 "SOFT_WARNING": 0,
                 "SOFT_CRITICAL": 0,
                 "SOFT_UNKNOWN": 0
               },
               "IN_DOWNTIME": {
                 "HARD_OK": 0,
                 "HARD_WARNING": 0,
                 "HARD_CRITICAL": 0,
                 "HARD_UNKNOWN": 0,
                 "SOFT_WARNING": 0,
                 "SOFT_CRITICAL": 0,
                 "SOFT_UNKNOWN": 0
               },
               "TOTAL": 68400000
             }
           },
           { "..." : "..."  },
           {
             "from": 1560117600000,
             "to": 1560722400000,
             "states_ms": {
               "OPERATIVE": {
                 "HARD_OK": 68400000,
                 "HARD_WARNING": 0,
                 "HARD_CRITICAL": 0,
                 "HARD_UNKNOWN": 0,
                 "SOFT_WARNING": 0,
                 "SOFT_CRITICAL": 0,
                 "SOFT_UNKNOWN": 0
               },
               "IN_DOWNTIME": {
                 "HARD_OK": 0,
                 "HARD_WARNING": 0,
                 "HARD_CRITICAL": 0,
                 "HARD_UNKNOWN": 0,
                 "SOFT_WARNING": 0,
                 "SOFT_CRITICAL": 0,
                 "SOFT_UNKNOWN": 0
               },
               "TOTAL": 68400000
             }
           }
         ]
       }
     ],
     "events": {
         "skipped": {
           "ack": 1,
           "ack_deleted": 12
         },
         "unknown": 1
     }
   }

Remember that in the response, only periods that fall entirely into the
calculation period are included; therefore, in the above response some
days will be excluded because they are not part of a full week that
starts on a Monday.

The *events* field of the response contains a report of the events that
the Backend has *skipped* on purpose or was not able to process because
it is of type *unknown*. In this example, we have 13 skipped events (1
of type **ack** and 12 of type **ack_deleted**) and one event that was
completely **unknown**. Detailed information about unknown events can be
found in the application logs.

.. warning:: If you are constructing such a structure by hand, note
   that it is easy to make mistakes which will be rejected by the SLM
   service. For instance, formatting the hour as **8:00** instead of
   **08:00** will return an error. Additionally, errors when creating
   timestamps will typically lead to the return of an empty set of
   results like this: ``{"periods":[], "events": { "skipped": {},
   "unknown": 0 }}``

Implementation Notes

-  All timestamps used throughout the SLM module are expressed as Unix
   time in milliseconds.
-  The **from** field of a time range is **inclusive**, while the **to**
   field instead is **exclusive**.
