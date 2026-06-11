How To Calculate Availability using the SLM Module
--------------------------------------------------

In this how-to, you will learn how to set up an availability report
using SLA types in the Service Level Management (SLM) module. A Service
Level Agreement (SLA) is a contract between your company and a customer.
If you offer similar SLAs to multiple customers, the standard elements
of the agreement should be separated from the customers’ data: the SLM
module makes this separation explicit during the configuration phase.

The following scenario will be used as guidance throughout the howto.
Suppose that for your company (called **ACME**) you need to monitor
the availability of services on three hosts of your network
(acme_linux_01,acme_linux_02, and acme_windows_01) and make sure that
those services are available **99.9%** of the time during **working
hours** (9AM to 5PM, or 9:00-17:00) from Monday to Friday, measured
weekly.

There are three elements in the SLM module that you need to configure:

* **SLA Type:** Basic SLA information that can be reused across many
  customers
* **Customer:** A table of customers and of their data
* **Contract:** The pairing of a customer and an SLA type for a
  specific set of monitored objects

**In a nutshell**. In order to create a SLA, the first step is to define
the Time Period needed in the Director, then use that Time Period to set
up one type of SLA Type, add customers data, and finally define the
Contract.

**Step #1: Configure a Time Period**

Follow the menu :menuselection:`Icinga Director / Timeperiods /
Timeperiods` and click on *Add*. Since the scenario defined above
requires availability during working hours, create one Time Period and
call it *Working Hours* (use the same for the Display Name field).

In this howto, the other fields will not be covered, so go ahead and
click on *Add* at the bottom of the form.

Now move to the **Ranges** tab and add the five days of the week and
hours for each like this:

+-----------+-------------+
| Day(s)    | Timeperiods |
+===========+=============+
| Monday    | 09:00-17:00 |
+-----------+-------------+
| Tuesday   | 09:00-17:00 |
+-----------+-------------+
| Wednesday | 09:00-17:00 |
+-----------+-------------+
| Thursday  | 09:00-17:00 |
+-----------+-------------+
| Friday    | 09:00-17:00 |
+-----------+-------------+

When done, you will need to deploy the new time period in Director
(:menuselection:`Icinga Director / Activity log / Deploy pending
changes`).

**Step #2: Configure an SLA Type**

You can now go to the SLM module. After clicking on the **SLM** menu
entry you should see a list of any SLA types you have already defined.
Let’s create a new SLA type that fulfils the required criteria by
clicking on *Add*. Enter a name, such as **Working Hours by Week**, and
a description.

Next, select the time period defined in Step #1 from the *Time Period*
drop-down. Then select *Weekly* from the Calculation Period drop-down,
since the availability is measured per week, Finally, enter the target
availability (99.9% in our case) and click on the *Store*
button. You’ve now created an :ref:`SLA type <slm-create-sla-types>`
that you can later reuse across multiple customers.

**Step #3: Add a Customer**

Next you need to create a Customer. Switch to the **Customers** tab,
where you will see a list of existing customers. Let’s add a new one,
say **Acme, S.p.A.**, along with a short description.

That’s it for this tab! Store the data and let’s go to the next step.

**Step #4: Add a Contract**

Now you need to link an SLA type to the customer and specify which hosts
and services are covered in the SLA.

Go to the **Contracts** tab and click on the *Add* action. Enter a name
like **Acme Weekly Availability** and a description. From the Customer
drop-down, choose the *Acme, S.p.A.* customer and then from the SLA Type
drop-down choose *Working Hours by Week*, as defined in Step #2.

All that’s left for configuration is to define the set of monitored
objects. These are specified by using a `filtering expression`.  In
our scenario, you can use the following filtering expression, as long
as no other host names begin with the string “acme”::

   host_name=acme*

When done, click on the **Store** button to save the configuration.

**Step #5: Create the Availability Report**

You are almost done: Once the SLA contract is set up, you can create
and print an availability report on demand. Follow the instructions to
:ref:`create a report <slm-create-report>`, making sure to set the
report type to **SLM Report** and the customer to **Acme, S.p.A.**.

The monitoring data needed to detail availability will be automatically
calculated and then imported for all SLA contracts defined in Step #4
for that customer.
