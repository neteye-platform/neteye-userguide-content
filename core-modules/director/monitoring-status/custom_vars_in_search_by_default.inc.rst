.. _custom_vars_in_search_by_default:

Include Custom Variables in Search Results by Default
`````````````````````````````````````````````````````

By default, custom variables are not used for filtering monitoring search results.
To change the default behavior and choose which custom variables to include
automatically, you can use the dedicated panel in the Neteye module configuration
section: simply write variable names (as displayed inside Hosts or Services) separated by commas, without any spaces.

.. _figure-neteye-module-configuration:

.. figure:: /core-modules/director/monitoring-status/img/neteye-module-configuration-01.png
   :alt: form to configure custom vars in search

   Specifying custom variables to include by default.

You can do this by going to **Configuration > Modules > neteye > Configuration**
(:numref:`figure-neteye-module-configuration`) and scroll down to the
"Icinga Monitoring - Search Custom Vars" section. Here you can insert the names
of the custom variables you want to extend the search results with by default,
for both hosts and services.

The variables are saved in the Neteye module config.ini file, and now when you
launch a search in the monitoring hosts or services list you'll see that the
filter is automatically updated and search results are linked to
the custom variables you've chosen.

.. _figure-monitoring-search-with-variables:

.. figure:: /core-modules/director/monitoring-status/img/monitoring-search-with-variables-01.png
   :alt: filter includes custom variables by default

   Custom variables are included in the filter automatically.
