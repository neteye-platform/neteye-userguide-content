.. _vsphere-monitoring-integration:

vSphereDB Monitoring Integration
````````````````````````````````

To easily and reliably check the status of a Monitored Object it is fundamental
to find all its Monitoring information in a single point. The VMD module integrates
the information related to your Virtualization infrastructure next to each Host in the Icinga DB module.

To integrate the VMD information into the Icinga DB module, navigate to
:menuselection:`VMD / Configuration / Monitoring` and then add a new integration
by clicking the :guilabel:`Add` button.

Configure the parameters related to your vCenter and Icinga DB backend (**Icinga DB Resource** and
**Source Type**), then you only need to configure a mapping that relates each Monitored Host with a VMD Object
(Host or Virtual Machine).

For example if you have a VM called **my_example_vm** whose hostname is ``example.com`` and
the Monitoring Host representing this VM has hostname ``example.com``, you need to map
the **Virtual Machine Property** *Guest Hostname* with the **Monitored Host Property** *Hostname*.

You can add more integrations for all the other property mappings that exists in your infrastructure.

If your integration has been correctly configured, you will see a new section like in :numref:`figure-vspheredb-monitoring-integration`
in the status page of those Monitored Host mapped with a VMD Object.

.. _figure-vspheredb-monitoring-integration:

.. figure::  /core-modules/director/img/vspheredb-monitoring-integration.png
   :alt: vSphereDB Monitoring Integration

   vSphereDB Monitoring Integration Section.

.. _vspheredb-dashboards:

vSphereDB Dashboards
````````````````````

The NetEye VMD Module installs out-of-the-box dashboards in :ref:`itoa-module-description`,
which give you easy access to both an overview and detailed information of your
virtualization infrastructure.

In particular, the dashboards are available in the **Main Org.** Grafana Organization
under the folder ``neteye-vspheredb-graphs`` and include the following dashboards:

* **Top VMs**: shows which Virtual Machines are responsible for the most traffic on their Virtual Interfaces in the given time period

* **Virtual Machine Details**: gives you insights on the System-level performance of each Virtual Machine


Since InfluxDB is used to store data used by the Dashboards, please
:ref:`configure the vCenter(s) <vspheredb-vcenter-performance-data>`
in |ne| VMD to write the Performance Data of your vCenter(s).

.. _vspheredb-vcenter-performance-data:

Configure a vCenter to write Performance Data
+++++++++++++++++++++++++++++++++++++++++++++

In **Single-Tenant environments** it is fine to write the Performance Data of all your vCenters
in a single InfluxDB database.

In **Multi-Tenant environments**, instead, it is necessary that
Performance Data of vCenters belonging to different tenants be written
on separate databases.

The procedure to follow for these two cases is different and is described below.

.. rubric:: Single-Tenant environment

Supposing our vCenter is named **example_vCenter**, proceed as follows:

#. Navigate to :menuselection:`VMD / example_vCenter`

#. Click the :guilabel:`Edit` button

#. In the **Ship Performance Data** form set:

   * **Performance Data Consumer** to *influx-perfdata*

   * **Database** to *vspheredb*

     .. note:: Both *influx-perfdata* and *vspheredb* are created by
	default by NetEye.

#. Save the changes

.. rubric:: Multi-Tenant environment

The procedure in this case is longer because each tenant should access
a different database with a different user.

In the following example we will assume that the we need to configure
the vCenter **example_vCenter** within the tenant **example_tenant**.

#. Create the InfluxDB database **vspheredb_example_tenant** and an InfluxDB user with the same name
   (if they do not exist already):

   #. Connect to the influxdb console with:

      .. code:: bash

	 neteye# influx -host influxdb.neteyelocal -ssl -username root -password "$(cat /root/.pwd_influxdb_root)"

   #. Create the database:

      .. code:: sql

	 CREATE DATABASE "vspheredb_example_tenant"

   #. Create the user:

      .. code:: sql

	 CREATE USER "vspheredb_example_tenant" WITH PASSWORD '<securepassword>'

      .. note:: Substitute ``<securepassword>`` with a secure password and save it in the file
	 :file:`/root/.pwd_influxdb_vspheredb_example_tenant` to avoid forgetting it.

   #. Grant the user **vspheredb_example_tenant** permissions on the database **vspheredb_example_tenant**:

      .. code:: sql

	 GRANT ALL ON "vspheredb_example_tenant" TO "vspheredb_example_tenant"

#. Create the Performance Data Consumer **influx-perfdata-example_tenant**:

   #. Go to :menuselection:`VMD / Configuration / Performance Data`

   #. Click on the :guilabel:`Add` button

   #. In the form set:

      * **Name** to *influx-perfdata-example_tenant*

      * **Implementation** to *InfluxDB*

      * **Base URL** to *https://influxdb.neteyelocal:8086*

      * **Username** to *vspheredb_example_tenant* (the InfluxDB user created above)

      * **Password** to the password of the **vspheredb_example_tenant** user (you should have saved it in
	the file :file:`/root/.pwd_influxdb_vspheredb_example_tenant`)

   #. Save the changes

#. Navigate to :menuselection:`VMD / example_vCenter`

#. Click on the :guilabel:`Edit` button

#. In the **Ship Performance Data** form set:

   * **Performance Data Consumer** to *influx-perfdata-example_tenant*

   * **Database** to *vspheredb_example_tenant*

#. Save the changes

You should then repeat this procedure for every tenant of which you
want to enable the Dashboards.
