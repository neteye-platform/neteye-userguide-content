
This section describes how to configure and applying the **retention
time** for Icinga Data Output (IDO) DB.

The retention time is by default set to **550 days**, meaning that the
data stored in IDO DB are kept for 550 days and deleted afterwards. This
will affect the monitoring data history, that are used for populating
SLM reports. This value can be changed at a later point,
customize it based on user's needs.

Background
``````````

In the **4.13** release a new setting has been introduced, called
**retention policy**. The reason for its introduction is that the all
data (warnings, logs, output of checks, and so on) are stored in the
Database, which can dramatically grow over time. This setting will be
enabled by default in **new installation only**, while existing
installation must configure it and enable it, following the steps
described next. The *default* value given to this setting is **550
days**, but can be changed at a later point.

How To Set The Retention Time
`````````````````````````````

To configure or modify the value, go to *Configuration > Modules >
Neteye > Configuration*.

-  **Step 1**. Insert a value in days for the *Default retention policy
   age*, which by default is **550**, i.e., one year.

-  **Step 2**. Click on the **Save Changes** button.

-  **Step 3**. Go to the command line and run the
   :command:`neteye install` script.


.. warning:: Only after *Step 3* the setting will be applied and
   enabled, so make sure to complete successfully all the steps!


How To Disable the Retention Time
`````````````````````````````````

The retention time can be disabled, meaning that no data will ever be
deleted. To do so, set the value of retention time to **0** (**zero**).

.. warning:: Disabling the retention time is discouraged, because the
   disk space required by the Databases might grow quickly if the
   monitoring activities on NetEye create a lot of input.

.. _icingadb-retention-and-slm-reports:

IcingaDB Retention and SLM Reports
----------------------------------

In addition to the IDO retention, **IcingaDB** has its own retention policy
that specifically affects  **Service Level Management (SLM)** reports. This
policy determines how long the historical data needed to calculate SLA compliance is kept.

The primary settings are:

* **History retention policy in days**: By default, this is set to **550 days** (1 year and 6 months).
* **SLA retention policy in days**: By default, this is set to **550 days** (1 year and 6 months).

.. note:: Only the **SLA retention policy** influences the generation of SLM reports.
   You cannot generate reports for timeframes older than this retention period.
