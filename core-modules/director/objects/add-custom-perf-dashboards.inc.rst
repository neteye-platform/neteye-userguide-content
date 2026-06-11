.. _grafana-performance-mapping:

Mapping performance graphs to Services and Checkcommands
````````````````````````````````````````````````````````

When adding new checkcommands to NetEye, the default representation of performance data, as
a unitless counter, is often not adequate for display in the monitoring objects detail page.

In NetEye this performance graph visualization can be adapted by linking the name of a checkcommand
or a service name to a ITOA Dashboard.

The following procedure describes how to do it:

#. Create a new dashboard related to the performance data of the desired check in ITOA `Main Org.`
   with as many panels as you need.
   In the queries, the variables `$hostname`, `$service` and `$command` can be used.
#. Go to the mappings configuration of Grafana module. From NetEye main
   menu `Configuration > Modules > Grafana Graphs`
#. Open the form to add a new mapping by clicking on the `Add New Grafana Graph` button.

    - The `Name` field must be equal to either the check name or the service name. This is the
      field that is used to find the right monitored object where to show the mapped performance graph.
    - The `Dashboard` field must contain the exact name of the dashboard created in the ITOA `Main Org.`.
    - The `Dashboard-UID` field must contain the exact UID of the dashboard created in the ITOA `Main Org.`.


.. warning:: The :command:`neteye install` configures the mapping for most checkcommands
             out of the box. If a mapping has been modified for a checkcommand, this mapping
             will be overwritten with the NetEye default configuration.

If the default Performance Graph is not sufficient for your visualization needs, you can customize
and adapt it by providing your own dashboard. Check out :ref:`grafana-perf-graph-custom` section for
more information.
