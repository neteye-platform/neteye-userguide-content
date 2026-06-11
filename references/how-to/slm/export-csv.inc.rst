How To Export SLM Data to a CSV File
------------------------------------

You may want to integrate report data into your existing reporting or
data warehousing solutions. You can export this data in CSV format,
which is a standard machine readable format handled by all common data
processing and business intelligence apps. This allows you to further
process report data tailored to your business, for instance by adding
statistics or graphics, or correlating it with other data sources.

This How To you will show you the procedure for exporting SLM data from
an existing report to a CSV (comma separated values) file.

**Prerequisites**

- The SLM module must be installed under :menuselection:`User Guide ->
  -Initial Configuration --> Installing Additional Modules` (see
  :ref:`Installing Additional Modules <neteye-install-modules>`)

- The SLM Report you want to export must already be defined in the
  Reporting module

**The Export Script**

The script *export_csv_report.php* can be used to export SLM report data
from a shell. Its required parameters are:

:*-u*: User name (any valid Icinga user)
:*-p*: Password
:*-i*: The Report ID (see below)
:*-f*: A *valid* path ending in a filename. If the path does not
       exist, an error will be returned. If the script runs
       successfully, the file will contain the report data in CSV
       format.

You can get further information about its other available parameters as
follows.

.. code:: bash

   # php /usr/share/neteye/reporting/scripts/export_csv_report.php --help

To obtain the report ID, navigate within the NetEye interface until you
arrive at the desired report. It should have a URL similar to the one
here (use the number at the end of the URL as the value for the *-i*
parameter)::

   https://<host>/neteye/reporting/reports#!/neteye/reporting/report?id=16

Once you have all the required information, you can call the export
script as in this example::

.. code:: bash

   # cd /usr/share/neteye/reporting/scripts/
   # php ./export_csv_report.php -u root -p pass -i 16 -f /home/my-report.csv

And finally, the resulting exported file should look like this::

.. code:: bash

   # cat /home/my-report.csv

.. container:: codeblock

   .. code::

      Contract,"Object Type","Object Name",Timeperiod,Availability
      "Contract A",Hosts,webserver,2019/08/26,100.00%
      "Contract B",Hosts,email-server,2019/08/26,99.70%
      "Contract C",Services,disk-check,2019/08/26,98.30%
