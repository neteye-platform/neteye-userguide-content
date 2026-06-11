
The Service Level Management Module (**SLM**) allows to setup contracts
between one SLM customer and the service provider, with the purpose to
sending a periodic report to the customer. Reports over specified period of time
can be created and scheduled for generation in :ref:`Reporting Module <slm-create-report>`,
due to functionality integrated from SLM module.

There are two types of contracts available:

-  The **Availability contract** measures the availability of hosts and
   services within a given period of time, to verify if the level of
   availability required by the customer has been met. An availability
   contract is defined by a customer, a SLA type and a set of monitored
   objects.

-  The **Resource Contract** shows a dashboard with diagrams that show
   the load of the monitored objects within a given time range.
   Dashboards are Grafana-based and need to be created for each
   contract. A resource contract is defined by a customer and a set of
   diagrams.

While the set up of a contract is quite straightforward, and it is
described in dedicated sections below, it is important to highlight a
few points that should be understood correctly, in order to avoid
possible sources of problems. They are described in the next section,
that you can look up for reference.

Important concepts
~~~~~~~~~~~~~~~~~~

-  **Users, customers, and their permissions**

   When configuring new object in the SLM module, it is important to
   highlight the importance of *permissions* in the management of
   customers. Indeed, NetEye users with access to the SLM module can see
   and assign to a customer only roles that they belong to.

   For example, if a user **Jake** has **B** and **C** roles, then he
   can only see and assign roles **B** or **C** to a customer. The only
   exception is for users with Role **Administrative access** in NetEye,
   which can assign every role.

   This affects both Availability and Resource contracts: if in the SLM
   Module there are contracts involving a role that is not assigned to a
   user, then they will not be seen by the user.

   It is therefore important to assign appropriate roles to a user of
   the SLM Module, to allow them to create and manage the contracts of
   his customers.

-  **Module permissions**

   While the role of users and customers is important to understand
   their access to contracts, but also the *module permissions* are
   relevant and need to be clearly understood.

   -  *Full Module Access*. It is essentially a shortcut and enables all
      the permissions below. User with *full module access* can see all
      the content of the module related to his role.
   -  *General Module Access*. This permission gives only the ability to
      load the module configuration and provide only *View* access to
      *Event Adjustment*, This permission is mandatory for enabling the
      following permissions. It is also necessary to enable the SLM extension
      of the Reporting module. To give *Add/Edit/Delete* permissions you
      need to enable slm/admin.
   -  *slm/admin*. With this permission it is possible to view and edit
      everything that the user’s role allows to see.
   -  *slm/report-adjustment-override*. Granting this permission allows
      to modify the ``Consider Event Adjustments`` field in the
      Reporting module, provided the Reporting’s SLM extensions have
      been enabled.

.. note:: Users with *slm/admin* or *slm/report-adjustment-override*
   permissions but without *General Module Access* can neither see nor
   access the SLM module (and the SLM extensions of the Reporting
   module), but this is the expected behaviour: the *General Module
   Access* is required, to load the configuration of the SLM module
   and activate the Extensions.

.. _operational-time-explained:

-  **Operational Time explained**

   The operational time does not need to indicate a single contiguous
   extent of time. For instance, it may be defined as “Business Hours”
   (i.e., “Monday through Friday, 9:00AM to 5:00PM”), which would
   exclude evening and early morning hours. You would construct such a
   Time Period in Director by specifying each individual contiguous
   *Time Range* separately, e.g. first “monday 9:00AM to 5:00PM”, then
   “tuesday 9:00AM to 5:00PM”, etc.

   When calculating availability, the monitored object’s initial state
   is valid until the first state change event (if one exists) during
   that Time Range, and the last state change event occurring in the
   Time Range is valid until the end of the Time Range. Thus given the
   “Business Hours” example above where the initial state of a service
   is **OK** on Monday at 9:00AM, and a single state change event of
   type **CRITICAL** occurs at 4:30PM, then the resulting availability
   will be 7 and a half hours of **OK** and 30 minutes of **CRITICAL**.

-  **How Downtime affects calculation**

   A downtime is a scheduled period in which a host or service is not
   available. Suppose we have an overall operational time of 10 seconds
   where a series of state change events result in:

   -  1 second where the host is *DOWN*
   -  7 seconds with the host in an *OK* state
   -  2 more seconds where the host is *DOWN*

   And let’s also assume that at the first second the downtime was
   unexpected, but the final 2 seconds of this period was scheduled
   downtime.

   -  If the *Downtime* box is checked, the availability will be
      calculated as *(OK + DOWNTIME)/OPERATIONAL TIME* = *(7s +
      2s)/10s*, therefore **9/10** or **90%**.

   -  If instead the box is not checked, the availability will be:
      *OK/OPERATIONAL TIME* = *7s / 10s*, therefore **7/10** or **70%**.
