
In order to configure a resource contract, you need to have defined a
customer like described :ref:`in the corresponding section above
<slm-create-sla-customer>`.  You also need to have set up suitable
dashboard(s) in Grafana that should be linked to the contract.

A user can define **Resource Contracts** for its resources to generate
the SLM Resource reports. To add a new **Resource Contracts** for SLM,
go to **SLM > Resource Contracts** and enter appropriate values for the
following options:

* **Name:** The name of this contract
* **Description:** A more user-friendly description of the contract
* **Customer:** You can set in the *Customer* tab the customer whose
  analytical dashboard in Grafana will be included in the resource
  report.  The customers will appear the dropdown, if they have
  **Analytics Module** access, and an assigned role that is in common
  with the SLM user.
* **Dashboard:** An analytical dashboard (static) of a customer in
  Grafana

To prevent a user from creating very large resource reports, there is a
restriction in place, that sets the maximum number of panels that can be
included in the report. A user cannot add a new *Resource Contract*, if
the number of panels in the selected dashboard exceeds the limit.

This limit can be increased manually, by updating the
**dashboard_panel_size** value in below SLM Module config file
:file:`/neteye/shared/icingaweb2/conf/modules/slm/config.ini`

Please be warned that increasing this limit will lead to a proportional
decrease in performance.
