
Creating SLA Contracts
~~~~~~~~~~~~~~~~~~~~~~

:term:`Average Availability` provides aggregated
statistics to support the verification of SLA for customers with more
complex contract definitions. Specifically, every report includes
by default the measure of the Average Availability of all hosts and
services in the time period. In order to enable Average Availability,
you should tick the *Include Average Availability* checkbox as described
below. You can override this setting by changing the same variable
in the report creation section--see :ref:`slm-create-report`.

Once you have defined an SLA type, you can begin creating Service
Level Agreement contracts. Click on :menuselection:`SLM / Contracts`
and enter appropriate values for the following options:

* **Name:** The name of this contract
* **Description:** A more user-friendly description of the contract. This
  description can be displayed in the report and supports formatting through
  `GitHub Flavored Markdown <https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax>`__.
* **Customer:** You can set in the *Customer* tab the customer whose
  monitored objects (typically hosts and services) will be included in
  the availability report. Only customers with the same role as the
  logged in user are displayed.
* **SLA Type:** The type of SLA you defined in the section above
* **Consider Event Adjustments:** This checkbox should be set by an
  administrator if you want to allow event adjustments :ref:`to be
  considered <slm-event-adjustment>` when generating a report.
* **Include Average Availability:** This checkbox should be set if
  you want to include average availability when generating a report.
* **Render Contract Description in Report:** This checkbox should be
  set if you want to insert contract description in rendered report.
* **Objects Type:** can be set to *host* or service* for including
  *respectively only host or service objects into the current
  ***Contract** or to *all* for considering both hosts and services.
* **Objects Filter:** A set of monitored objects determined by an
  Icinga `filter expression`. To create the filter expression, it is
  recommended to use the search filter builder in the Icinga DB Overview
  and then copy the generated expression.

  It is important to check that the filter expression actually returns
  at least one monitored object.
* **IcingaDB Views:** Depending on the choice of the *Object types*,
  here will be shown in parentheses the count of hosts, services, or
  both, that match the *object filter*. A click on the *Hosts* link or
  *Services* link will take you to the related `IcingaDB Overview`.
