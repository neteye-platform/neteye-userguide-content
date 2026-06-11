Availability Reports
~~~~~~~~~~~~~~~~~~~~

The SLM module is compatible with Icinga’s :ref:`Reporting module <reporting-module>`. One use of the data provided via SLM is
for creating availability reports for the monitored objects included
in each customer contract.

.. note:: Before configuring a new report, make sure appropriate permissions are granted to
   the user’s NetEye role. If the user has to to define a new report he should have
   at least the `General module access` enabled both for SLM Module and for Reporting, and the
   `reporting/reports` permission enabled under the `Reporting` section.

To create an availability report, you will need to:

* Configure one or more customers, SLA types, and contracts :ref:`in
  the SLM module <slm-configuration>`

* Create a new report in the Reporting module and set the following
  fields, which are all compulsory:

  * **Name**: A name that uniquely identifies the report
  * **Timeframe**: Selecting a value here defines for how much time
    the report will be generated. This value must be higher than the
    *calculation period* defined in the SLA Type for which the report
    is generated, otherwise it will lead to an *empty report* (see
    next section).
  * **Report**: Set this to *SLM Report*, whereupon the form will add
    the following field
  * **Customers**: Choose the customer you want to create the report
    for
  * **Contract**: Choose the contract corresponding to the data to be
    processed in the report. In the *Contract* dropdown, you will be
    able to select only those contracts where the *Calculation Period*
    is defined to be smaller than the selected *Timeframe*. This
    ensures that the report will have an appropriate number of pages.
  * **Consider Event Adjustments**: With this drop-down, users with
    the appropriate :ref:`permission
    <slm-event-adjustment-gui-permissions>` can choose whether or not
    to consider user-defined event adjustments in this report. There
    are three possible choices:

    * `<Yes/No> (inherited from “< ContractName >”)`: This option
      contains the ``Consider Event Adjustment`` flag value (i.e. Yes
      or No) with the contract name from which the value is inherited.
    * *Yes*: Override that value, forcing event adjustments to be
      considered
    * *No*: Override that value, forcing event adjustments to be
      ignored

  * **Include Average Availability**: With this drop-down, users can
    choose whether or not to include average availability in this report.
    There are three possible choices:

    * `<Yes/No> (inherited from “< ContractName >”)`: This option
      contains the ``Include Average Availability`` flag value (i.e. Yes
      or No) with the contract name from which the value is inherited.
    * *Yes*: Override that value, forcing average availability to be
      included
    * *No*: Override that value, forcing average availability to be
      ignored

  * **Show Outages Count**: Show how many Outages are defined per calculation period.
  * **Show Outages List**: Show Outages in the report.
    If set to *Yes* the following fields will appear:

    * **Show Outages List Limit**: Set the maximum number of outages to show
      per calculation period.
    * **Sort Outages by**: Users can sort Outages by *Duration* or by *Start/End Date*.
      If not specified, Outages are sorted by *Duration*.
    * **Outages order**: Users can invert the Outages sorting order by
      selecting the *Ascending* or *Descending* option in this field.
      *Descending* is the default order.

After you click on “Create Report”, the report will appear in the list
of available reports.

Within each report, you can read the details related to the selected
contract and its monitored objects. This information is typically
divided into hosts and services, and represents their percentage of
availability. The availability of a monitored object will be green if it
is above the threshold defined in its SLA Type, and red if not. In
addition, all monitored objects that did not record any events during
the reporting period will be listed separately.

You should ensure that the filter expression used in the **Objects
Filter** field on the **Contract** tab returns at least one monitored
object (e.g., at least one host or service).

Invalid Report Configurations
`````````````````````````````

While the SLM module does not allow users to create incorrect report
configurations, there are circumstances in which reports may seemingly
contain wrong data, namely when the report is **empty** or **very
large**. The reasons behind these two cases, along with solutions, are
explained in the next sections.

Configurations Leading to Empty Reports
+++++++++++++++++++++++++++++++++++++++

If the report’s *time frame* and the contract’s *calculation period*
aren’t compatible, the report generated will be empty. This can happen
when:

* The **Calculation Period** is greater than the **Time Frame**.  For
  example, setting the calculation period to *monthly_12* and defining
  the time frame to be from **01.01.2019** to **01.06.2019**. This
  would be like trying to fit 12 months inside 6 months.

* The **time frame** doesn’t contain at least one entire and valid
  **calculation period**.  For instance, when you define a report with
  a *monthly* calculation period, while the time frame is defined to
  start on **02.07.2019** and finish on **29.08.2019**. Here, neither
  the time within July nor the time within August represents a
  complete month.

If you find you have created a report definition matching one of these
cases, you can fix it with one of the following solutions:

* Make the time frame defined for the specific report longer
* Select a different **SLA Type** in the contract form, with a smaller
  calculation period
* Select a smaller calculation period in the definition of the SLA
  type associated with the contract used for creating the report

Configurations Leading to Very Large Reports
++++++++++++++++++++++++++++++++++++++++++++

If the combination of a report’s *time frame* and the contract’s
*calculation period* would lead to a number of *calculation period*
slots higher than a pre-determined limit, it strongly implies that the
report produced would have an excessive number of pages.

NetEye attempts to avoid this situation by preventing a user from
creating very large reports. In general, reports consisting of
hundreds of pages are not useful. However, should you wish to override
this upper bound for the allowed number of *calculation periods*, you
can change this limit in the SLM module’s configuration page
(:menuselection:`Configuration / Modules / slm / Configuration`) with
the field *Maximum report size*.  If you do so, please note that
increasing this limit will lead to a proportional decrease in
performance.
