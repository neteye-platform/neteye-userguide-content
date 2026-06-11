
Defining SLA Types
~~~~~~~~~~~~~~~~~~

An SLA is a commitment between a service provider and a client, defining
particular aspects of a service. Within the SLM module, an SLA type can
be associated with a customer contract and defines limits for metrics to
be guaranteed by the service provider as well as the exact temporal
boundaries during which the metrics must be guaranteed.

.. review whole paragraph because slm panel order has changed

When the SLM module is first launched, the **SLA Type** panel is
focused, displaying a row for each configured SLA type. Additional
panels allow to define :ref:`Contracts <slm-create-sla-contract>` and
:ref:`Customers <slm-create-sla-customer>` respectively. Search
functionality is available for all three panels, but the text being
searched will be restricted to the *Name* and *Contract* columns in
the first two panels.

.. note:: Before you can successfully set up SLA types and contracts,
   you need to have defined a few other objects in the Directory,
   namely one or more **TimePeriod**\ s (as *Operational Time*) and a
   **filter expression** (as *Object Filter*). An *Object Filter* is
   used to define for which host(s) or service(s) the contract is
   defined; in other words, on these hosts or services it will be
   calculated the availability required by the customer. Examples of
   valid input for the *Object Filter* are: ``host.name~*neteye*`` or
   ``service.name~health*``.

Before you can create an SLA contract to see the availability of
monitored objects for a customer, you must first define the parameters
for the SLAs you intend to use. You can do this at :menuselection:`SLM
/ SLA Types`, using the follow options:

* **Name:** The name of this SLA type (e.g., “Gold level”)
* **Description:** A more user-friendly description of the SLA type
* **Operational Time:** The exact time(s) during which all elements
  necessary for a monitored object to function properly should in fact
  be in operation. The operational time is precisely defined by a
  *TimePeriod* object in Director. This field lets you select either a
  pre-defined Timeperiod object or one that you have created at
  :menuselection:`Icinga Director / Timeperiods / Timeperiods`.

* **Calculation Period:** The unit of time over which the data will be
  aggregated into service level reports. For instance, if you want an
  availability report for the current year, you might want it broken
  down into “Monthly” or “Weekly” subsections.
* **Availability %:** The target percentage of SLA availability for
  the calculation period. For hosts, only *Down* states have a
  negative impact on availability. For services instead, both
  *Critical* and *Unknown* (but not *Warning*) will decrease
  availability.
* **Downtime:** When this box is checked, the scheduled downtimes of
  monitored objects will be taken into account for any related
  availability calculations. When downtime is in effect, the related
  monitored object is considered *available*, regardless of its actual
  state during that period. Once the scheduled downtime ends, the
  object’s state will be reset to the value of its most recent state
  change.

At the moment the only supported TimePeriod values (i.e., Ranges) are
exact dates and names of weekdays. There is also currently no support
for **excluded ranges** and **included ranges**.

More precise definitions of :term:`Calculation Period`,
:term:`Availability`, :term:`Downtime`, and other terms can be found
in the :ref:`glossary <glossary>`, while an example of the algorithm
on which the SLM is based, is shown in :ref:`How the availability is
calculated <slm-availability-calculation>`.

The following table defines each **calculation period** more precisely:

+---------------+------------------------------------------------------+
| Calculation   | Unit of Time                                         |
| Period        |                                                      |
+===============+======================================================+
| daily         | **from** 00:00 **to** 24:00 of that same day         |
+---------------+------------------------------------------------------+
| weekly_sunday | **from** 00:00 on Sunday **to** 24:00 on Saturday    |
+---------------+------------------------------------------------------+
| weekly_monday | **from** 00:00 on Monday **to** 24:00 on Sunday      |
+---------------+------------------------------------------------------+
| monthly_1     | **from** 00:00 on the first day of one month **to**  |
|               | 24:00 on the last day of the same month              |
+---------------+------------------------------------------------------+
| monthly_2     | **from** 00:00 on the first day of one month **to**  |
|               | 24:00 on the last day of the subsequent month        |
+---------------+------------------------------------------------------+
| monthly_3     | **from** 00:00 on the first day of one month **to**  |
|               | 24:00 on the last day of the third month             |
+---------------+------------------------------------------------------+
| monthly_4     | **from** 00:00 on the first day of one month **to**  |
|               | 24:00 on the last day of the fourth month            |
+---------------+------------------------------------------------------+
| monthly_6     | **from** 00:00 on the first day of one month **to**  |
|               | 24:00 on the last day of the sixth month             |
+---------------+------------------------------------------------------+
| monthly_12    | **from** 00:00 on the firstday of one month **to**   |
|               | 24:00 on the last day of the twelfth month           |
+---------------+------------------------------------------------------+

.. note:: The “last day” of a month may be the the 28th or 29th for
   February, 30th or 31st otherwise.
