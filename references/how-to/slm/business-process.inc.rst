How To Show the Availability of a Business Process
--------------------------------------------------

A typical scenario for availability reporting is to show the logical
availability of all the monitored objects (e.g., hosts and services)
defined by an explicit Business Process model. So for instance if you
have two redundant core network switches sized for failover, you can
model them with a logical OR and report their joint availability rather
than reporting on them separately.

In this How To you will create a sample business process, set up some
report parameters, and generate a report covering the services that are
part of that business process.

**Prerequisites**

- The Business Process module must be enabled under
  :menuselection:`Configuration / Modules / businessprocess`
- The SLM module must be installed under :menuselection:`User Guide -->
  Initial Configuration --> Installing Additional Modules` (see
  :ref:`Installing Additional Modules <neteye-install-modules>`)

.. _slm-bp-availability-one:

**Step #1: Set up a Business Process (or Reuse an Existing One)**

First, let’s configure a business process to use as an example for
checking availability. Open **Business Process** from the left side menu
and select **Create**. In the panel that appears, enter the following
information:

* **Name:** Company.com
* **Title:** ERP Services
* **Description:** The services that compose ERP for Company.com
* **Backend:** *Keep the default backend*
* **State Type:** *Keep ‘Use SOFT states’*
* **Add to menu:** *Change this to ‘No’*

Next, create three child nodes for our new business process with the
following structure:

* ERP System
* Inventory Server (Host)
* Supply Chain Server (Host)

If you’ve never created a business process node before, you can follow
the official Icinga tutorial.

.. _slm-bp-availability-two:

**Step #2: Configure Checks in Director**

Next, go to the Icinga Director module and set up the following:

* If it doesn’t already exist, create and deploy a new host template
  of type *generic-host*.
* Create a host inheriting from that template named
  business-process-checker*, with the *Host address* set to
  127.0.0.1* (if using a cluster, set it to the public facing
  cluster IP).
* Open :menuselection:`Icinga Director / Commands / External Commands`
  and search for icingacli-businessprocess-neteye*. In its **Fields**
  tab, make sure the Business Process Process ID* field is set to
  *mandatory*.
* If it doesn’t already exist, create and deploy a new top-level
  service template named *business-process-service-check*, setting its
  check command to *icingacli-businessprocess-neteye*.
* Create a service named business-process-check* on that service
  template and link it to your previously created
  *business-process-checker* host, specifying the top-level business
  process node *ERP System* (from Step #1) in the field Business
  Process Process ID*.
* Deploy the Director configuration (:menuselection:`Icinga Director /
  Activity Log / Deploy`)
* Go to :menuselection:`Overview / Hosts`, and check that both the
  *business-process-checker* host and its service are up and running.

.. _slm-bp-availability-three:

**Step #3: Configure Time Periods in Director**

In the Director module, define the *Time Period* during which our
business process is contractually required to be up and running. In this
example, it will be the entire week excluding Saturday and Sunday.

-  Go to :menuselection:`Icinga Director / Timeperiods / Timeperiods`
-  Create a new TimePeriod called “24x5”
-  On the **Ranges** tab, set the time ranges as follows (Director will
   sort them alphabetically)::

      Friday:      00:00-24:00
      Monday:      00:00-24:00
      Thursday:    00:00-24:00
      Tuesday:     00:00-24:00
      Wednesday:   00:00-24:00

- Then deploy the Director configuration (:menuselection:`Icinga
  Director / Activity Log / Deploy`) so that the time ranges can be
  used in the next step.

.. _slm-bp-availability-four:

**Step #4: Set up Service Level Management**

Now you can set up the processing step that will link the underlying
BP service availability data to the reporting parameters. This step is
carried out in the :ref:`SLM module <slm-configuration>`:

-  In the **Customers** tab, add a customer such as “Company.com”
-  In the **SLA Types** tab, create a new instance with the following
   parameters:

   -  **Name:** Gold
   -  **Operational Time:** *24x5* (the value from the previous step)
   -  **Calculation Period:** *Daily*
   -  **Availability:** 99.5
   -  **Downtime:** *(not checked)*

-  In the **Contracts** tab, add a new contract with:

   -  **Name:** Company 24x5 BP Availability
   -  **Description:** (same as above)
   -  **Customer:** *Company.com*
   -  **SLA Type:** *Gold*
   -  **Objects Filter:** “service.name=business-process-check”
      (you could also run this as a single top-level host/service check
      by using “host.name=business-process-checker”)

Once you add an objects filter, you will immediately see links to the
hosts and services it maps to, allowing you to double check that those
you expected are there. Please ensure that at least one host or service
is returned.

.. _slm-bp-availability-five:

**Step #5: Create the Report**

Finally, let’s define the report elements for the business process.

After naming the report, you will need a *Time Frame* that specifies the
starting and ending dates/times for the report. If none of the existing
Time Frames fit your needs, go to the **Time Frames** panel, and add a
new one by selecting the “New Timeframe” action. Enter the following
parameters (remember that the calculation period must be shorter than
the time frame):

* **Name:** “Last week including today”
* **Start:** “last week”
* **End:** “tomorrow”

Next, define a new report by specifying the type and any parameters
necessary for that type:

* **Name:** Business Process Report
* *Timeframe:** *Last week including today*
* **Report:** *SLM Report*
* **Customer:** *Company.com*

You can now view the new report by clicking on its name in the table.
You should see the availability of the Business Process host
*business-process-checker* and its associated service
*business-process-check* that you configured in :ref:`Step 2
<slm-bp-availability-two>` above.
