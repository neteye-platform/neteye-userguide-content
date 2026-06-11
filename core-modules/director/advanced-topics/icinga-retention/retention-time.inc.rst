
This section describes how to configure and apply the **retention
time** for monitoring data stored in **Icinga DB**.

The retention time is by default set to **550 days**, meaning that the
historical data stored in Icinga DB are kept for 550 days and deleted
afterwards. This affects the monitoring history used to populate SLM
reports. In Icinga DB, retention is configured through two policies,
which can be adjusted later according to the user's needs.

.. _icingadb-retention-and-slm-reports:

Icinga DB Retention and SLM Reports
-----------------------------------

In **Icinga DB**, retention is managed through two policies:

* **History retention policy in days**
* **SLA retention policy in days**

.. note:: Only the **SLA retention policy** affects **Service Level Management (SLM)** report generation.
   You cannot generate SLM reports for *time frames* older than this retention period.

How To Set The Retention Time
`````````````````````````````

To configure or modify the values, go to *Configuration > Modules >
Neteye > Configuration*.

-  **Step 1**. Insert a value in days for the *History retention policy
   in days*, which by default is **550**.

-  **Step 2**. Insert a value in days for the *SLA retention policy in
   days*, which by default is **550**.

-  **Step 3**. Click on the **Save Changes** button.

-  **Step 4**. Go to the command line and run the
   :command:`neteye install` script.


.. warning:: Only after *Step 4* will the settings be applied and
   enabled, so make sure to complete successfully all the steps!


How To Disable the Retention Time
`````````````````````````````````

The retention time can be disabled, meaning that no data will ever be
deleted. To do so, set the retention policy value to **0** (**zero**).

If you want to disable retention entirely, set both *History retention
policy in days* and *SLA retention policy in days* to **0**.

.. warning:: Disabling the retention time is discouraged, because the
   disk space required by the Databases might grow quickly if the
   monitoring activities on NetEye create a lot of input.
