The Reporting module is the central component for creating reports
over a specified timeperiod and schedule their generation.

Reporting module is available as part of the NetEye Core, providing
the ability to create **Icinga DB Reports** - host and service availability
reports based on the monitoring database (Icinga DB). In this case users can create reports of the following types:
* **Host SLA**
* **Total Host SLA**
* **Service SLA**
* **Total Service SLA**

On top of that the Reporting module offers a set of functionality integrated
from other modules, like :ref:`Service Level Management <monitor-slm-concepts>`,
that is to be installed as an :ref:`additional feature module <neteye-modules>`. With this being installed,
the Reporting module functionality is extended to create an **SLM report** of a certain type.

The SLM integration gives users the possibility to generate
reports based on **Availability Contracts** and on **Resource Contracts**,
respectively named as **SLM Reports** and **SLM Resource Reports**.


Access to the Reports list
``````````````````````````

The list of reports, visible under :menuselection:`Reporting / Reports`, is filtered
out based on user's roles and permissions. The filtering logic applied by the SLM and
the IDO modules is slightly different: let's see in details how the list is
rendered, for both categories.


SLM reports
+++++++++++

Users with only Reporting **General Module Access** permissions
(defined in :menuselection:`Configuration / Access Control / Roles`), will see a filtered list of
SLM reports. The filter is based on the **Customer** selected in report definition.
A logged-in user will see only their own
**Customer** (i.e. customers with the same or inherited role as the logged-in user) reports.

|NE| admin and reporting admin users can see and can create/modify all SLM reports.

.. note::

   The availability of data for SLM reports is directly affected by the **IcingaDB SLA retention policy**.
   If you cannot generate a report for a specific timeframe, it may be because the historical data is no longer available.

   For more details, please refer to the :doc:`IcingaDB Retention and SLM Reports </core-modules/director/advanced-topics/icinga-retention>`
   documentation.


Icinga DB reports
+++++++++++++++++

Reports are filtered by **tenants**. This means that the logged-in user can see or create
(based on the user's permission defined in :menuselection:`Configuration / Access Control / Roles`)
reports linked to its tenant.

The reports, that are not linked to any tenant, are visible to all users,
regardless of the tenant they belong to.

Once a report is created, only the |NE| admin can modify the tenant it is associated with.
