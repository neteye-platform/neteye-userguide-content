Customizing Grafana
~~~~~~~~~~~~~~~~~~~

.. _grafana-conf-files:

Grafana Configuration Files
```````````````````````````

The main Grafana configuration file that can be modified is:

``/neteye/shared/grafana/conf/grafana.ini``

In addition, the Grafana provisioning system can be configured as described in the
official `Grafana documentation <https://grafana.com/docs/grafana/latest/administration/provisioning>`__.

Provisioning configuration files must be placed in the corresponding directory:

``/neteye/shared/grafana/conf/provisioning/``

Each subdirectory within this path contains a ``sample.yaml`` file. This file must
**not** be modified directly. Instead, it must be copied and the copied file can be
edited as required.




.. _grafana-perf-graph-custom:

Customizing Performance Graph
`````````````````````````````

If the `default Performance Graph <https://github.com/NETWAYS/icingaweb2-module-grafana/blob/v3.1.1/doc/04-graph-configuration.md>`__ for a check command is not suitable for your needs,
you can adapt it by providing your own dashboard.

First of all,
`create the desired ITOA dashboard <https://github.com/NETWAYS/icingaweb2-module-grafana/blob/v3.1.1/doc/06-create-grafana-dashboards-influxdb.md>`__
in the Main Org.

.. note:: You are advised to not modify the preconfigured dashboards in the `neteye-performance-graphs`
          folder, as they will be overwritten at the next `neteye install` execution.

Finally you have to update the mapping from check command to dashboard
in the :menuselection:`Configuration / Performance Graph` section.

If you want to change an existing graph, you must update the **Dashboard UID** field using
the UID of the previously created dashboard.

To add a new graph please refer to
`Icingaweb2 Module Grafana doc <https://github.com/Mikesch-mp/icingaweb2-module-grafana/blob/v1.4.2/doc/04-graph-configuration.md>`__


.. _grafana-perf-graph-custom-advanced-automated:

.. rubric:: Automating Creation of Custom Performance Graphs

If you need to automate the creation of custom performance graphs, you can do so by using a preconfigured JSON file in the following way:

#. Create an ITOA dashboard with the desired panel
#. Name the dashboard the same as the check command
#. Export the dashboard in JSON format by clicking :menuselection:`Share / Export / View JSON`
#. Save the JSON as :file:`/usr/share/grafana/public/dashboards/neteye-performance-graphs/neteye/<check-command>.json`
#. At this point, a couple of changes to the JSON are needed:

   #. Remove the `uid` field at the root level
   #. Set the `id` field at the root level to `null`
   #. Set the `datasource` field of the panel to `null`

#. To customize any `option <https://github.com/NETWAYS/icingaweb2-module-grafana/blob/v3.1.1/doc/04-graph-configuration.md>`__ of the performance graph, you can do so by adding an ini file in :file:`/usr/share/grafana/public/dashboards/neteye-performance-graphs/neteye/<check-command>.ini`.

   #. The ini file must have the same name as the check command
   #. Create a section with the same name as the check command
   #. Add the options you want to customize in the section as key-value pairs
   #. Example: :file:`/usr/share/grafana/public/dashboards/neteye-performance-graphs/neteye/my_check_command.ini`

      .. code-block:: ini

         [my_check_command]
         panelId = "1,2"
         customVars = "&os=$os$"

#. Run the :ref:`neteye install command <neteye-install>` to apply the changes

      .. code-block:: bash

         # neteye install
