
An outage is a period of time during which a :term:`Monitored Object`
(i.e., a host or a service) or a :term:`Business Process`
is in a **non-available** state. In the simplest case, an outage is
defined as the period of time between two consecutive **available**
statuses, during which an event takes place, that influences the :term:`Availability`;
the states that affect Availability negatively and that are used for the
calculation of outages are **HARD** events only.

Each Outage has an associated **Duration**, which is the total amount of
time during which the Monitored Object or Business Process was in a
non-available state.

During each calculation period, the **sum** of the duration of *all
outages* plus the duration of *Available* statuses **must equal** the
duration of the calculation period itself.

Outages appear as part of each *SLM report*, if configured, and it is
possible to configure to show only the number of outages, or a detailed
list of outages for each monitored object in the report. It is also possible
to include :term:`Outage Annotation` for every outage, which will then be
rendered in the *SLM report*, if defined. More information can be found
in the :ref:`Outage configuration <monitor-sla-outages>` section.

Whenever the availability is below the required *target availability*,
the monitored host or service is highlighted. You can drill down on any
calculation period with an Availability below 100%, to a new page,
presenting a detailed report of the related events.

In case an :term:`Operational Time`
has been defined in a SLA, the definition and computation of an outage
change slightly. In this case, indeed, Outages are recorded *only* when
they fall **within an Operational Time**; when the state of
non-availability takes place outside an Operational Time, it is called
**Unavailability Period**. In other words, an Unavailability Period is
*an interval of time during which a service, host, or Business Process
is not available* and becomes an Outage if it falls at least partially
within an Operational Time.

In the remainder of this section, we show examples of how Outages are
calculated with Operational time and how they are considered within and
across Calculation Periods.

Records of Outages with Operational Time
++++++++++++++++++++++++++++++++++++++++

This section enumerates all the basic cases that are taken into account
for the calculation of outages, by considering a scenario with one
Calculation Period composed of two Operational Time Ranges: the first
from **8AM to 9AM** and the second from **10AM to 11AM**.

More complex scenarios can be reproduced by suitably combining the
following cases.

Throughout this section, in all the diagrams and in the text, ``OT``
stands for **Operational Time** and reports all the *Operational Time
Ranges*, while ``UnPs`` represent **Unavailability Periods**, i.e., the
intervals during which a host or service is unavailable.

.. rubric:: Unavailability Periods outside the Operational Time


::

       7AM        8AM        9AM       10AM      11AM     12AM
        |----------|----------|----------|---------|--------|
   OT              |----------|          |---------|
   UnPs    1---1   2---2
             |       |
             |       \-> 1 outage of 30 minutes
             \-> no outage

In this example the Unavailability Period ``1`` [*7:20AM*-*7:40AM*]
starts and ends outside any Operational Time Range, so it causes no
outage.

The Unavailability Period ``2`` [*8AM*-*8:30AM*] instead, falls
completely inside the first Operational Time Range, so it causes an
outage that starts at *8AM* and ends at *8:30AM*, with a duration of
**30min**.

**Reported Outage**:

* Outage start: **8AM**
* Outage end: *8:30AM**
* Outage duration: **30min**

.. rubric:: Unavailability Periods starting or ending outside Operational Times


::

       7AM        8AM        9AM       10AM      11AM     12AM
        |----------|----------|----------|---------|--------|
   OT              |----------|          |---------|
   UnPs      1---------------------1
                   |
                   \-> 1 outage of 1 hour

The UnP ``1`` [*7:30AM*-*9:30AM*] starts before the OT range
[*8AM*-*9AM*] and ends after the OT range [*8AM*-*9AM*].

Since Outages exist only during the Operational Time, the UnP causes an
Outage that starts together with the OT at *8AM*, which is the beginning
of the OT range [*8AM*-*9AM*] and ends at *9AM*, which is the end of the
OT range [*8AM*-*9AM*].

The duration of the Outage is **1h**, which is the amount of Operational
Time affected by the Outage.

**Reported Outage**:

* Outage start: **8AM**
* Outage end: **9AM**
* Outage duration: **1h**

.. rubric:: Unavailability Periods across different Operational Time ranges


::

       7AM        8AM        9AM       10AM      11AM     12AM
        |----------|----------|----------|---------|--------|
   OT              |----------|          |---------|
   UnPs      1--------------------------------1
                   |
                   \-> 1 single outage of 90 minutes

The UnP ``1`` [*7:30AM*-*10:30AM*] starts before the first OT range
[*8AM*-*9AM*], and ends during the second OT range [*10AM*-*11AM*].

This means that the Unavailability Period causes an Outage that starts
at *8AM* (the start of OT range [*8AM*-*9AM*]) and ends at *10:30AM*,
after the second OT range starts. The duration of the Outage is **1h
30min**, since the Outage affected the full OT range [*8AM*-*9AM*] and
the first *30min* of the OT range [*10AM*-*11AM*].

**Reported Outage**:

* Outage start: **8AM**
* Outage end: *10:30AM**
* Outage duration: **1h 30min**

.. rubric:: Temporary Uptimes between Operational Time ranges


::

       7AM        8AM        9AM       10AM      11AM     12AM
        |----------|----------|----------|---------|--------|
   OT              |----------|          |---------|
   UnPs    1---------------------1   2-----------------2
                               ↓       ↓
                          1 single outage of 2 hours

In this case the UnP ``1`` [*7:20AM*-*9:20AM*] entirely covers the OT
range [*8AM*-*9AM*] and the UnP ``2`` [*9:40AM*-*11:30AM*] entirely
covers the subsequent OT range [*10AM*-*11AM*].

The resulting Outage is a **single Outage** that starts at *8AM* and
ends at *11AM*, with a duration of **2h**. Only one Outage is reported
because **during the Operational Time**, the Monitored Object or
Business Process was **continuously unavailable**, with the short
availability window completely outside the OT. The short interval during
which the Monitored Object or Business Process was available
[*9:20AM*-*9:40 AM*] is disregarded as it is outside the OT.

**Reported Outage**:

* Outage start: **8AM**
* Outage end: **11AM**
* Outage duration: **2h**

.. rubric:: Temporary Uptimes during Operational Time ranges


::

       7AM        8AM        9AM       10AM      11AM     12AM
        |----------|----------|----------|---------|--------|
   OT              |----------|          |---------|
   UnPs    1----------------1      2----------2
                   |                  |
                   |                  \-> 1 outage of 30 minutes
                   \-> 1 outage of 50 minutes

Here the UnP ``1`` [*7:20AM*-*8:50AM*] ends just before the end of the
first OT range [*8AM*-*9AM*], and the UnP ``2`` [*9:30AM*-*10:30AM*]
overlaps with the beginning of the second OT range [*10AM*-*11AM*].

This causes **2** different Outages, because during both Operational
Time Ranges the Monitored Object or Business Process changes status
(here, it becomes available). The first Outage starts at *8AM* and ens
at *8:50AM*, with a duration of **50min**. The 2nd Outage starts at
*10AM* and ends at *10:30AM*, with a duration of **30min**

**1st Reported Outage**:

* Outage start: **8AM**
* Outage end: *8:50AM**
* Outage duration: **50min**

**2nd Reported Outage**:

* Outage start: **10AM**
* Outage end: *10:30AM**
* Outage duration: **30min**

.. rubric:: Unavailability Periods starting or ending on OT range start or end


::

       7AM        8AM        9AM       10AM      11AM     12AM
        |----------|----------|----------|---------|--------|
   OTs             |----------|          |---------|
   UnPs    1------------------1          2-----------------2
                             ↓            ↓
                          1 single outage of 2 hours


In case an Unavailability Period starts or ends exactly in the same
instant when an OT range starts or ends, then the start or end of the
Unavailability Period will be always considered as happening **outside**
the OT range. This case is very similar to the first case presented,
with the only difference that two events coincide with the start or end
f an Operational Time Range.

The UnP ``1`` [*7:20AM*-*9AM*] ends exactly in the same moment when the
first OT range [*8AM*-*9AM*] ends, while UnP ``2`` [*10AM*-*11:50AM*]
starts exactly in the same moment when the second OT range
[*10AM*-*11AM*] starts.

This causes a single Outage that starts at *8AM* and ends at *11AM*,
with a duration of **2h**.

This happens because the end of UnP ``1`` [*7:20AM*-*9AM*] is considered
as happening after the first OT range [*8AM*-*9AM*] and the start of UnP
``2`` [*10AM*-*11:50AM*] is considered as happening before the second OT
range [*10AM*-*11AM*]. So the Monitored Object or Business Process was
continuously unavailable for the whole Operational Time.

**Reported Outage**:

* Outage start: **8AM**
* Outage end: **11AM**
* Outage duration: **2h**

Outages and Calculation Periods.
++++++++++++++++++++++++++++++++

The previous cases take into account Operational Time Ranges, during
which the interval between two consecutive Time Ranges is disregarded,
so an Unavailability Period that spans multiple OT Ranges is considered
as a single Outage.

In this section, we define how Outages are considered when
:term:`Calculation Periods<Calculation Period>` are taken into account.

The simplest case, when one Unavalability Period (and therefore the
Outage) is entirely included in a Calculation Period, results in a
single Outage reported in that Calculation Period.

However, when an Unavailability Period spans across two Calculation
Periods, although Calculation Periods are by default contiguous time
intervals, the Outage will be split into **two** outages, one for the
first Calculation Period, one for the second. The same applies when
multiple Calculation Periods are involved, like the following diagram
shows. Here, ``CP`` are Calculation Periods and ``OUT`` are the recorded
Outages.

::

       Jan 2020   Feb        Mar       Apr       May      June
        |----------|----------|----------|---------|--------|
   CPs  |----------|----------|----------|---------|--------|
   UnPs   1-------------1         2----------------------2
   OUT    |---O1---|-O2-|         |--O3--|---O4----|--O5-|

In this example, although there are only *two* UnP, **five** Outages
will be reported, one for each month in which the Monitored Object or
Business Process was not available. In summary (note that the actual
*Duration* in the report will be in HH:MM:SS, here we use days for
simplicity), the first Unavailability Period results in:

**Reported Outage** (January):

* Outage start: **7th of January**,
* Outage end: **31st of January**
* Outage duration: **25 days**

**Reported Outage** (February):

* Outage start: **1st of February**
* Outage end: **14th of February**
* Outage duration: **14 days**

while the second Unavailability Period results in:

**Reported Outage** (March):

* Outage start: **10th of March**
* Outage end: **31st of March**
* Outage duration: **21 days**


**Reported Outage** (April):

* Outage start: **1st of April**
* Outage end: **30th of April**
* Outage duration: **30 days**

**Reported Outage** (May):

* Outage start: **1st of May**
* Outage end: **19th of may**
* Outage duration: **19 days**
