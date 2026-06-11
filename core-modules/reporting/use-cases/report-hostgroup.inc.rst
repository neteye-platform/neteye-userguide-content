.. _create-report-hostgroup:

How To Create a Report for a Hostgroup
--------------------------------------

This How To will help you define and produce a basic report for a Linux
hostgroup. It assumes you already have a set of Linux systems being
monitored which are configured as a single hostgroup.

Start the Reporting module and select the **Open reports** action, which
will display the list of existing reports in the **Reports** tab.

Before you can define a report, you must first make sure that the *Time
Frame* you want already exists. If you cannot find one among the
defaults, you will need to switch to the **Time Frames** tab to create
one.

A time frame consists of a descriptive name to distinguish it from
other time frames, a start date/time, and an end date/time. These
dates/times can be either *absolute* by choosing it with the date
picker widget, or *relative* expressed as a text string. You can find
more information in the documentation of `Supported PHP Time Formats
<https://www.php.net/manual/en/datetime.formats.php>`_.

Examples include:

* *now* (only as an *end* date/time)
* *-4 hours*
* -1 year*
* *monday last week midnight*
* *first day of January this year midnight*

When done, click on the **Create Time Frame** button.

You can now return to the **Reports** tab and select the **New Report**
action to create a new report definition.

In the form, fill in the fields for *Name*, *Time Frame* and *Report*.
The *Report* field corresponds to the type of report to create:

* **Host SLA:** A report based on whether a host or hostgroup meets an
  SLA threshold.
* **Service SLA:** A report based on whether a service or servicegroup
  meets an SLA threshold.
* **System:** A *phpinfo* report that displays PHP and server
  settings.

You can produce a Host or Service SLA report with just the information
for these three fields and the remaining defaults, although this will
include all hosts or all services in your report. To select a subset
of hosts or services, add a filtering expression using the
:ref:`monitoring reference guide <monitoring-module-restrictions>`.

Since this How To is to create a Linux Hostgroup report, select the
*Host SLA* report type and fill in the monitoring expression for your
group, e.g.: *hostgroup_name=LinuxServers*. Then select a *Breakdown*
like “Day” and a *Threshold* like “99.5”. Click on the **Create Report**
button to define the report with the chosen parameters and add it to the
list.

Click on your newly created report to see the current SLA for each host
in your hostgroup, modify your report parameters, or schedule the report
to be run at regular intervals.
