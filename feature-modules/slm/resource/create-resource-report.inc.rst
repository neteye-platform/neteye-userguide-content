
.. _monitor-slm-resoure-reports:

Resource Reports
~~~~~~~~~~~~~~~~

.. note:: Before configuring a new report, make sure appropriate permissions are granted to
   the user’s NetEye role. If the user has to to define a new report he should have
   at least the `General module access` enabled both for SLM Module and for Reporting, and the
   `reporting/reports` permission enabled under the `Reporting` section.

To create an *SLM resource* report, you will need to:

* Configure one or more customers and resource contracts :ref:`in the
  SLM module <slm-configuration>`
* Create a new report in the Reporting module and set the following
  fields, which are all compulsory:

  * **Name**: A name that uniquely identifies the report
  * **Timeframe**: Selecting a value here defines for how much time
    the report will be generated.
  * **Report**: Set this to *SLM Resource Report*, whereupon the form
    will add the following field.
  * **Customers**: Choose for which customer you want to create the
    report.  In *Customers* drop-down menu, you will be able to select
    only customers who either have access to the **Analytics Module**
    or have a role in common with the SLM user himself.
  * **Resource Contracts**: Choose the contract, linked to the
    analytics dashboard for a selected customer.

After you click on “Create Report”, the report will appear in the list
of available reports.

Within each report, you can read the details related to the selected
contract. The *SLM Resource Reports* will contain all the static panels
including panels inside the rows of a Grafana dashboard linked to the
selected resource contract.
